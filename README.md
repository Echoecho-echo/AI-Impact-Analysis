# Impact of AI on Students' Academic Performance - Exploratory Data Analysis
An exploratory data analysis investigating whether generative AI usage is correlated with academic improvement or decline among university students, and whether AI dependency contributes to burnout and exam anxiety.

## Research Questions
1. Does AI usage influence academic decline or improvement, and how significant is its impact?
2. Does AI dependency engender burnout and academic anxiety?

**Hypothesis:** If AI usage has an impact on academic anxiety, then it is the leading cause of academic burnout.

**Dataset**
  File: ai_student_impact_dataset.xlsx
  Size: 50 000, no missing values
  Key columns: Student_ID, Major_Category
Year_of_Study,
Pre_Semester_GPA,
Weekly_GenAI_Hours,
Primary_Use_Case,
Prompt_Engineering_Skill,
Tool_Diversity,
Paid_Subscription,
Traditional_Study_Hours,
Perceived_AI_Dependency,
Institutional_Policy,
Anxiety_Level_During_Exams,
Post_Semester_GPA, Skill_Retention_Score, Burnout_Risk_Level 

A cleaned copy of the dataset is exported at the end of the notebook as ai_impact_on_students.csv, with a newly created column from mapping Burnout_Risk_Level.

**Method**
1. Setup – load libraries (pandas, matplotlib, seaborn, statsmodels.formula.api) and the raw dataset.
2. Dataset overview – shape, nulls, dtypes
3. GPA distributions – pre- vs post-semester GPA histograms/boxplots (left-skewed).
4. GPA growth vs. AI use – GPA growth calculated as the % change between mean of the Pre-semester GPA and Post-semester GPA; correlation between Weekly GenAI Hours and Post-Semester GPA
5. AI usage by year of study – Weekly GenAI hours across Freshman-Senior
6. Prompt engineering skill vs study habits – boxplot against traditional study hours
7. Skill retention by major – AI weekly usage vs Skill_Retention_Score, split by Major_Category
8. Prompt engineering skill vs GPA – with AI weekly usage as a factor
9. AI dependency vs traditional study hours – regression plot and spearman correlation
10. Burnout vs AI usage – Burnout_Risk_Level mapped to a numeric scale; correlated against AI dependency and exam anxiety using the Spearman's rank correlation (appropriate because these are ordinal variables)
11. Consolidated correlation heatmap – all key numeric variables together, plus a bar chart of correlations with Perceived_AI_Dependency
12. Multiple regression (OLS) – to check whether AI usage predicts GPA once study habits, major, and year are controlled for.
13. Answer to the proposed hypothesis.
