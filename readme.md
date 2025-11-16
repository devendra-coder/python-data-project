# Overview
This project explores the data analyst job market using real job postings, focusing on the most in-demand skills, salary trends, and the optimal skill combinations for career growth. I used Python to clean, analyze, and visualize the dataset, and built insights around how different skills impact both job demand and salary potential. The goal was to understand which skills matter most, how their popularity changes over time, and which ones offer the best long-term returns for someone building a career in data analytics.

# Tools I used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
   - **Pandas Library**: This was used to analyze the data.
   - **Matplotlib Library**: I visualized the data.
   - **Seaborn Library**: Helped me create more advanced visuals.
- **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code**: My go-to for executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

#### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)
    # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

fig.suptitle('Likelihood of Skills Requested in Indian Job Market', fontsize=15)
fig.tight_layout(h_pad=.8)
plt.show()
```

#### Results

![Visualization of top skills in India](2_project/images/skills_demand_in_India.png)


#### Insights

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, SQL is the most sought-after skill, appearing in **68% of job postings**.

- Data Engineers require more specialized technical skills **(AWS, Azure, Spark)** compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

- Python is a versatile skill, highly demanded across all three roles, but most prominently for **Data Scientists (70%) and Data Engineers (61%)**.


# 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [Skills_Trend](2_project/3_Skills_Trend.ipynb)

#### Viualize Data

```Python
from matplotlib.ticker import PercentFormatter

df_plot = df_da_india_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in India')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

# annotate the plot with the top 5 skills using plt.text()
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i], color='black')

plt.show()
```

#### Results

![Trending Top Skills for Data Analysts in India](/2_project/images/top_trending_skills_in_india.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

#### Insights

- SQL remains the most consistently demanded skill throughout the year, although it showed a gradual decrease in demand but now it is again in demand.

- Excel experienced a **significant increase in demand starting around May**, surpassing both Python and Tableau by the end of the year.

- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

# How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [Salary_Analysis](/2_project/4_Salary_Analysis.ipynb)

#### Visualize Data

```Python
sns.boxplot(data=df_india_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

# this is all the same
plt.title('Salary Distributions in India')
plt.xlabel('Yearly Salary (INR)')
plt.ylabel('')
plt.xlim(0, 250000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'₹{int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

#### Result

![Salary Distribution in India](/2_project/images/salary_distribution_in_india.png)
*Box plot visualizing the salary distributions for Data Jobs and their Senior roles 6 data job titles.*

#### Insights

- **Salary levels climb steadily with role advancement :-** The median salary for Data Analyst roles appears close to **₹85K–₹95K**, while Senior Data Scientist roles reach around **₹145K–₹165K**, showing a clear jump of roughly **₹60K–₹70K** between entry-level analytical work and high-impact senior science positions.

- **The spread widens noticeably in advanced technical roles :-** Data Analyst and Senior Data Analyst salaries mostly fall inside a band of roughly **₹60K–₹140K**, while Data Engineer, Senior Data Engineer, and Senior Data Scientist roles stretch out closer to **₹180K–₹230K** at the upper whisker, indicating larger differences driven by skills, company tier, tech stack, and project ownership.

- **Outliers suggest strong upside potential in engineering and science-focused paths :-** Several Data Engineer and Senior Data Engineer points sit above **₹200K**, and a few Senior Data Scientist points approach around **₹230K–₹250K**, implying that top performers or those in premium organizations can earn noticeably beyond the typical range.


### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```Python
fig, ax = plt.subplots(2, 1)  

sns.set_theme(style='ticks')

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_da_top_pay, x='median', y=df_da_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].set_xlim(0, 180000)
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'₹{int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_da_skills, x='median', y=df_da_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (INR)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'₹{int(x/1000)}K'))

plt.tight_layout()
plt.show()
```

#### Result

Here's the breakdown of the highest-paid & most in-demand skills for data analysts in India:

![Demand and Salary](/2_project/images/demand_and_salary.png)
*bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

#### Key Insights

- **High-paying skills are mostly engineering-oriented and backend-focused.**  
   Tools like **PySpark, Linux, GitLab, MySQL, and PostgreSQL** show median salaries around **₹160K–₹175K**, highlighting that analysts who grow toward **data engineering and big-data processing** earn significantly more.

- **In-demand skills lean toward business analytics and reporting tools.**  
   **Power BI, Tableau, Excel, SQL, and Python** rank among the most requested, with salaries mostly in the **₹85K–₹120K** range, showing that companies value **insight generation and dashboarding skills**.

- **There’s a clear gap between market demand and earning potential.**  
   Skills like **PySpark, Neo4j, and Databricks** are **high-pay but not high-demand**, while **Excel, Power BI, and Tableau** are **high-demand but mid-pay**. This suggests an ideal path: **build core analytics skills first, then add big-data and engineering tools** to boost long-term earning power.


# 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [Optimal_Skills](/2_project/5_Optimal_Skills.ipynb)

#### Visualize Data

```Python
from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary (₹INR)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in India')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'₹{int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```

#### Results
![Most Optimal SKills in India](/2_project/images/most_optimal_skills_without_color.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in India.*

#### Insight

### Key Insights

- **Power BI and Tableau offer the best overall advantage**, showing **high median salaries (around ₹108K–₹112K)** with **moderate job share (~20–25%)**, making them strong value skills for analysts focused on visualization and business impact.

- **SQL holds the strongest job share (~48–50%)** while still maintaining a **competitive median salary (~₹97K)**, confirming it as a **core must-have skill** rather than a specialization advantage.

- **AWS and R sit on the lower end of both demand and pay (₹78K–₹82K)**, indicating that while useful, they **won’t drive hiring or compensation** compared to visualization (Power BI, Tableau) and core analytical skills (SQL, Python).

### Visualizing Different Techonologies
Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```Python
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

# Prepare texts for adjustText
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Set axis labels, title, and legend
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary (INR)')
plt.title('Most Optimal Skills for Data Analysts in India')
plt.legend(title='Technology')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'₹{int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
ax.set_xlim(0, 60)

# Adjust layout and display plot 
plt.tight_layout()
plt.show()
```

#### Results

![Most Optimal Skills](/2_project/images/most_optimal_skills.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in India with color labels for technology.*

#### Insights

### Key Insights (Revised)

- **Visualization skills (Power BI, Tableau) clearly outperform others** with **median salaries above ₹105K** while maintaining **steady market demand**, showing how business-facing analytics continues to drive value in India.

- **Programming and core analytics skills (SQL, Python) remain essential**, with **40–50% job share** and **₹95K–₹100K** salaries, making them the foundational requirement for almost every data analyst position.

- **Cloud-related skills (AWS, Azure) currently offer lower salary returns (₹80K–₹95K)** and **limited job demand**, suggesting they are **supportive add-on skills**, not primary growth levers for analysts right now.


# What I Learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:
  - **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
  - **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
  - **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

  # Insights
  This project provided several general insights into the data job market for analysts:

   - **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
  - **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
  - **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

  # Challenges I Faced
  This project was not without its challenges, but it provided good learning opportunities:

  - **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
  - **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
  - **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
