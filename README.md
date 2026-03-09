# Overview

This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.
The data sourced provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1.What are the skills most in demand for the top 3 most popular data roles?

2.How are in-demand skills trending for Data Analysts?

3.How well do jobs and skills pay for Data Analysts?

4.What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used

-Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
   
    -Pandas Library: This was used to analyze the data.
    
    -Matplotlib Library: I visualized the data.
    
    -Seaborn Library: Helped me create more advanced visuals.

-Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.

-Visual Studio Code: My go-to for executing my Python scripts.

-Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

#### Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

```

### Filter Greece Jobs

To focus my analysis on the Greek job market, I apply filters to the dataset, narrowing down to roles based in Greece.

```python
df_GR = df[df['job_country'] == 'Greece']
```


# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanding skills for the top 3 most popular data roles, I filtered out those positions by which ones were the most popular and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depenting on the role I'm targeting.

View my notebook with detailed steps here:
[2_Skills_Demand.ipynb](Analysis_of_Data_Analyst_Jobs/2_Skills_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc["job_title_short"] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x="skill_percent", y="job_skills", ax=ax[i], hue="skill_count", palette="dark:b_r")

plt.show()
```

### Results

![Visualization of Top Skills for Data Roles](Project/images/skill_demand_all_data_roles.png)*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientist (73%) and Data Engineers (65%).

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Enginners, SQL is the most sought-after skill, appearing in 71% of job postings.

- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau)


## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2025 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2025.
View my notebook with detailed steps here:[3_Skill_Trend.ipynb](3_Skill_Trend.ipynb)
## Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_GR_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette="tab10")

plt.cga().set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in Greece](Project/images/Skill_Trend_DA.png) 
*Graph visualizing the trending top skillss for data analysts in Greece in 2025.*


### Insights

-Alteryx showed the most significant surge in demand, peaking dramatically at approximately 34% of job postings in July.This demand was short-lived and highly concentrated, as the likelihood for Alteryx was near 0% for all other months. This suggests a massive, time-bound hiring effort for a specific project or role requiring this specialized tool.

-Azure showed high, yet spiky, demand. It peaked at approximately 12% in May and again in October.Despite the low overall count, the alternating peaks of Azure 
indicate that cloud skills are essential, with Azure being the primary focus for most of the year.

-AWS maintained a very low profile, only showing demand (approx. 5%) in January.

-Bash command-line skills saw a strong surge in the middle of the year, peaking at approximately 17% in June. This often suggests a need for skills in automation, scripting, and manipulating data pipelines.

-BigQuery was almost non-existent until the end of the year, where it showed a strong, sudden peak of approximately 11% in December. This indicates a potential shift towards or a significant project launch on the Google Cloud platform towards the end of the 2025 cycle.

-All skills, except for Azure, show extended periods where their likelihood in job postings is 0%. This points to a job market that is not seeking a broad, generic Data Analyst but is intensely focused on one or two niche skills at any given moment.

## 3. How well do jobs and skills pay for Data Analyst?

To identify the highest-paying roles and skills, I only got jobs in Greece and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary Analysis.ipynb](4_Salary Analysis.ipynb)

### Salary Analysis 

#### Visualize Data

```python
sns.boxplot(data=df_GR_top6, x="salary_year_avg", y="job_title_short", order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```
#### Results

![Salary Distributions of Data Jobs in Greece](Project/images/Salary_Boxplot.png)
*Box plot visualizing the salary distributions for the top 6 data jobs titles.*

#### Insights

-Machine Learning Engineer (MLE) maintains the highest median salary (approximately $120K), confirming that specialized engineering roles requiring advanced statistical and programming knowledge are typically the highest-compensated.Data Analyst (DA) and Senior Data Scientist (SDS) roles follow closely with high median salaries (around $110K). This indicates strong value placed on business-oriented data roles and clear rewards for seniority.

-The Data Scientist (DS) and Data Engineer (DE) roles show the most compact salary distributions. Their IQR boxes are the narrowest (approximately $80K to $110K for DS), suggesting a highly standardized compensation structure for mid-level professionals in these two core technical roles.

-Conversely, Data Analyst and Software Engineer (SWE) roles exhibit the widest salary dispersion. This wide range (IQR spanning ~$50K to $60K) implies that job experience, company sector, and specific responsibilities cause the largest swings in compensation for these two positions.

-All roles show earning potential extending to or near the $200K mark, primarily driven by the MLE and SDS whiskers, which reach the furthest right. This demonstrates that specialized expertise and senior leadership are rewarded significantly across the data field.he general salary floor for the middle 50% of the market (the start of the IQR box) appears to be around $80K for most data roles (DA, DS, DE, SDS), though the Software Engineer role has a slightly lower starting point (around $70K).

### Highest Paid & Most Demanded Skills for Data

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

#Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x="median", y=df_DA_top_pay.index, hue="median", ax=ax[0], palette="dark:b_r")

#Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x="median", y=df_DA_skills.index, hue="median", ax=ax[1], palette="light:b")

plt.show()

```

#### Results

Here's the breakdown of the highest-paid & most in-demand skills for data analysts in Greece:

![The Highest Paid & Most In-Demand Skills for Data Analysts in Greece](Project/images/Highest_paid_and_most_in_demand_skills_for_data_analysts.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in Greece.*

#### Insights

-The top graph shows specialized technical skills like `aws`, `express`, and `docker` are associated with higher salaries, some reaching up to $1600K, suggesting that advanced technical proficiency can increase earning potential.

-The bottom graph highlights that foundational skills like `Java`, `R`, and `Python` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

-There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [5_Optimal_Skills.ipynb](5_Optimal_Skills.ipynb)

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

df_DA_skills_high_demand.plot(kind="scatter", x="skill_percent", y="median_salary")
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in Greece](Project/images/Most_Optimal_Skills_for_Data_Analyst_in_Greece.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in Greece.*

#### Insights

-The skill `Java` appears to have the highest median salary of nearly $1.4 Million, despite appearing in less than 20% of job postings.

-Skills like `Docker`,`Kubernetes`, `Linux`, `express` and `aws` are clustered together with extremely high median salaries (between $1.4M and $1.6M). This suggests that Data Analysts who possess strong engineering and infrastructure skills are highly valued but rarely sought, indicating that these roles may be specialized Data Engineers or ML Engineers disguised as Data Analysts.

-`Python` and `SQL` are the cornerstone skills for the Data Analyst market in Greece.SQL is the most demanded skill by a wide margin, appearing in approximately 50% of all job postings. While its median salary (around $1 Million) is high, it is lower than highly specialized skills, reflecting its foundational nature.Python offers the best balance of high salary (around $1.2 Million) and high prevalence (around 48% of jobs). Proficiency in Python appears to be the most rewarding skill for the majority of Data Analyst roles.

-`Power BI` and `Tableau` show significant demand (around 25% prevalence) with median salaries ranging from $800K to $1 Million. This confirms that visualization and reporting capabilities are a crucial, expected part of the role.

-`Spreadsheet` and `git` are the skills with the lowest median salaries (below $600K), despite being fairly common. This suggests that while these tools are essential prerequisites, they do not drive salary potential on their own.

-Skills related to cloud services (`Azure`) and specialized database/ETL tools (`SAS`, `Spark`, `Databricks`) typically cluster between the $900K and $1 Million median salary mark, suggesting that while they offer good remuneration, they are still viewed as secondary to core scripting (Python) and query (SQL) skills.

#### Visualizing Different Technologies

We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in Greece](Project/images/Most_Optimal_Skills_for_Data_Analysts_in_Greece_with_coloring_by_technology.png) *A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in Greece with color labels for technology.*

#### Insights
-The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

-Analyst tools (colored orange), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

-The other skills (colored green), such as Flow and Spreadsheet, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.


# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

-Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.

-Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.

-Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

-Skill Demand and Salary Correlation: There is a clear correlation between the demand for specific skills and the salaries these skills command. -Advanced and specialized skills like Python and Java often lead to higher salaries.

-Market Trends: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.

-Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

-Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

-Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

-Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
