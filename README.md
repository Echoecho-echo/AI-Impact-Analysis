# Impact of AI on Students’ Academic Performance — Exploratory Data Analysis

AI has become such a huge part of our existence, in that it has affected so many different aspects, tertiary education being one of them. As a result of its integration into our daily lives, I've decided, as a student, to analyse just how much impact it has on people like myself, university students. Below is an exploratory data analysis investigating whether generative AI usage is correlated with academic improvement or decline among university students, and whether AI dependency contributes to burnout and exam anxiety.

## Research Questions

1. Does AI usage influence academic decline or improvement, and how significant is its impact?
1. Does AI dependency engender burnout and academic anxiety?

**Hypothesis:** If AI usage has an impact on academic anxiety, then it is the leading cause of academic burnout.

## Dataset

- **File:** `ai_student_impact_dataset.xlsx`
- **Size:** 50,000 rows, no missing values
- **Key columns:** `Student_ID`, `Major_Category`, `Year_of_Study`, `Pre_Semester_GPA`, `Post_Semester_GPA`, `Weekly_GenAI_Hours`, `Primary_Use_Case`, `Prompt_Engineering_Skill`, `Tool_Diversity`, `Paid_Subscription`, `Traditional_Study_Hours`, `Perceived_AI_Dependency`, `Institutional_Policy`, `Anxiety_Level_During_Exams`, `Skill_Retention_Score`, `Burnout_Risk_Level`

A cleaned copy of the dataset is exported at the end of the notebook as `ai_impact_on_students.csv`.

## Method

1. **Setup** — load libraries (`pandas`, `matplotlib`, `seaborn`, `statsmodels.formula.api`) and the raw dataset.
1. **Dataset overview** — shape, nulls, dtypes.
1. **GPA distributions** — pre- vs. post-semester GPA histograms/boxplots (left-skewed).
1. **GPA growth vs. AI use** — GPA growth calculated as the % change between mean `Pre_Semester_GPA` and mean `Post_Semester_GPA`; correlation between `Weekly_GenAI_Hours` and `Post_Semester_GPA`.
1. **AI usage by year of study** — weekly GenAI hours across Freshman–Senior.
1. **Prompt engineering skill vs. study habits** — boxplot against traditional study hours.
1. **Skill retention by major** — AI weekly usage vs. `Skill_Retention_Score`, split by `Major_Category`.
1. **Prompt engineering skill vs. GPA** — with AI weekly usage as a factor.
1. **AI dependency vs. traditional study hours** — regression plot and Spearman correlation.
1. **Burnout vs. AI usage** — `Burnout_Risk_Level` mapped to a numeric scale; correlated against AI dependency and exam anxiety using Spearman’s rank correlation (appropriate since these are ordinal variables).
1. **Consolidated correlation heatmap** — all key numeric variables together, plus a bar chart of correlations with `Perceived_AI_Dependency`.
1. **Multiple regression (OLS)** — `Post_Semester_GPA` regressed on `Weekly_GenAI_Hours`, `Traditional_Study_Hours`, `Major_Category`, and `Year_of_Study`, to check whether AI usage still predicts GPA once study habits, major, and year are controlled for.
1. **Summary of key findings.**

## Key Findings

|Finding                                                        |Value                                          |
|---------------------------------------------------------------|-----------------------------------------------|
|Dataset size                                                   |50,000 rows                                    |
|Pre-semester mean GPA                                          |3.15                                           |
|Post-semester mean GPA                                         |3.35                                           |
|GPA growth (pre → post semester)                               |+6.46%                                         |
|Correlation: weekly AI use ↔ Post-semester GPA (Pearson)       |−0.02 (negligible)                             |
|Correlation: AI dependency ↔ traditional study hours (Spearman)|−0.1 (weak negative)                           |
|Correlation: AI dependency ↔ burnout (Spearman)                |0.34 (moderate positive)                       |
|Correlation: AI dependency ↔ exam anxiety (Spearman)           |0.28 (weak positive)                           |
|Regression: `Weekly_GenAI_Hours` → GPA (controlled)            |coef = 1.362e-06, p = 0.996 — not significant  |
|Regression: `Traditional_Study_Hours` → GPA (controlled)       |coef = 0.0133, p < 0.001 — significant         |
|Regression: `Year_of_Study` → GPA (controlled)                 |significant for all years vs. Freshman baseline|
|Regression: `Major_Category` → GPA (controlled)                |only STEM significant (coef = 0.024, p = 0.001)|
|Regression model R²                                            |0.027                                          |

**Takeaways:**

- **AI usage does not meaningfully affect GPA.** The raw correlation between weekly AI hours and post-semester GPA is negligible (−0.02), and the controlled regression confirms this: once major, year of study, and traditional study hours are accounted for, `Weekly_GenAI_Hours` has no significant effect on GPA (p = 0.996). `Traditional_Study_Hours` is the real, significant driver of GPA in this model (p < 0.001).
- **AI dependency is associated with burnout and exam anxiety.** Spearman correlations of 0.34 (burnout) and 0.28 (anxiety) support the hypothesis directionally, though these are moderate/weak correlations, not strong ones, and correlation alone doesn’t establish causation.
- **AI dependency shows a weak negative association with traditional study hours** (−0.1), suggesting AI use isn’t strongly displacing study time, though the direction is consistent with some substitution effect.
- **The regression model’s R² (0.027) is low** — the included variables explain only ~2.7% of GPA variance, so most of what drives GPA isn’t captured by this model. This is a genuine limitation, not just a footnote.
- Overall GPA improved by 6.46% from pre- to post-semester across the sample, which does not support a narrative of AI-driven academic decline.

## Requirements

```
pandas
matplotlib
seaborn
statsmodels   # for the multiple regression (Section 12)
openpyxl      # for reading the .xlsx source file
```

## Usage

1. Place `ai_student_impact_dataset.xlsx` in the same directory as the notebook.
1. Run all cells in order (`Kernel → Restart & Run All`).
1. The cleaned dataset will be exported as `ai_impact_on_students.csv` in the working directory.

## Notes

- `Burnout_Risk_Level` (categorical: Low/Medium/High) is mapped to a numeric `Burnout_Level` column in Section 10 for correlation analysis.
- GPA distributions are left-skewed (median > mean), with pre-semester GPAs trending toward the lower end.
- **Methodology corrections applied in this version:** the GPA growth calculation now compares mean `Pre_Semester_GPA` vs. mean `Post_Semester_GPA` (previously used a `.diff().sum()` approach that telescoped to a single row-pair difference). Correlations involving ordinal variables (`Burnout_Level`, `Perceived_AI_Dependency` vs. study hours, `Anxiety_Level_During_Exams`) now use Spearman’s rank correlation rather than Pearson. A multiple regression (Section 12) was added to control for `Major_Category`, `Year_of_Study`, and `Traditional_Study_Hours` when assessing the AI-hours-to-GPA relationship.
- **Possible next step:** a similar controlled regression on `Skill_Retention_Score` (with `Weekly_GenAI_Hours`, `Perceived_AI_Dependency`, `Prompt_Engineering_Skill`, and `Primary_Use_Case` as predictors) would extend this analysis to Research Question 2’s knowledge-retention angle, which isn’t yet covered by a regression model — only by the correlation heatmap and Section 7 visualisation.