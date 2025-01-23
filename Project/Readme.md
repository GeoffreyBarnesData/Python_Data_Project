# The Analysis

## 1. What are the most demanded skills for the most popular data roles?

To find this information, I filtered out those positions by location, specifically in Canada. Then I filtered for the top job roles. This query showed me the most popular job titles in the data, data analyst, data engineer, and data science. I also found the top skills for these jobs, and isolated them for analysis. Python and SQL, which I am skilled with, are two of the popular choices for job listings.

View my notebook with detailed steps here: [2_skills_demand.ipynb](2_Skills_Demand.ipynb)

### Visualize Data

```python

fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='split_skills', y='skill_percentage', ax=ax[i], title = job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel("")
    ax[i].legend().set_visible(False)

fig.suptitle('Skills Required For Various Job Postings In Canada', fontsize=16)

#This is so that there is no overlap in tick markers and graph names
plt.tight_layout()    

plt.show
```

### Results

![Skills Required For Various Job Postings In Canada](images/Skills_Required_For_Various_Job_Postings_In_Canada.png)

### Insights

-Python is a very versitile skill that is relevant across many different job titles. If you like data but you are unsure of the role you would like to take on, python should be what you learn first.
-SQL is the most in demand skill for data analysis, which is something I'm interested in. I already took a course on SQL so I feel confident with my skills there, so I would be well suited for a data analysis job.
-Data engineering asks of less general skills than data science or data analysis. Cloud based skills that are less common overall have a niche with data engineers.
-Some skills are rare like watson (~4% of roles require this), so its not worth learning right away. It would instead be better to use them as your job requires. General knowledge of coding that can be gained from python can be helpful across multiple different coding languages, so learning python makes sense while searching for a data related position.


## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data

```python

df_plot = df_DA_CA_percent[['sql','python','excel','tableau','power bi']]

sns.lineplot(data=df_plot, dashes =False, palette='tab10')
sns.set_theme(style='ticks')
plt.xticks(rotation=30)
plt.ylabel('likelihood in Job Posting')
plt.xlabel('2023')
plt.legend(bbox_to_anchor=(1.25, 1), loc='upper right', title='skills')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())

plt.show()


```

### Results

![Skills Likelihood in Job Posting](Images\Skills_Likelihood_in_Job_Posting.png)

### Insights:

-I found that SQL is the most commonly requested skill in all months of the year. 
-I also found that the next 4 common options that I have experience in are grouped together over this year's data. Python and excel trade places every once in a while as the number two most requested skill that I am comfortable using.
-I also noticed that tableau dips drastically in February, while SQL peaks in April. Also Excel in October is at its highest, but then falls to around 3% of job postings in December. Power Bi also plummets in December.


# Analysis of the Salaries of the Top 8 Canadian Data Roles

## 3. How well do jobs pay pay for Data Analysts compared to other roles that involve data? How well do individual skills corrolate with higher pay?

### Salary Analysis for Data Related Job Seekers

#### VIsualizing the Data

```python
sns.boxplot(data=df_CA_top8, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='white')
plt.title('Salary Distribution In Canadian Jobs')
plt.xlabel('Annual Salary')
plt.ylabel('')
plt.xlim(0,500000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

![Salary Distribution in the top 8 Canadian Data Jobs](images\Salary_Distribution_in_Canadian_Jobs.png)


```python
df_DA_top_skills = df_DA_top_skills.sort_values('median',ascending=False)


fig, ax = plt.subplots(2,1)
sns.set_theme(style='whitegrid')

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:g_r')
ax[0].legend().remove()
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.barplot(data=df_DA_top_skills, x='median', y=df_DA_top_skills.index, hue='median', ax=ax[1], palette='dark:b')
ax[1].legend().remove()
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
#to make sure the graphs are equal sizes in the x dimention
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

plt.tight_layout()
plt.show()

```

![Top 10 Most In-Demand Skills for Data Analysts](images\Top_10_Highest_Paid_Skills_for_Data_Analysts.png)

### Insights:

-I learned that sometimes, in cleaning the data you don't always have enough good data to gleam the information that you need. I had to switch over to using US data instead, and I was able to create a stunning visual (above) with this data.

-I learned that the highest paying skills are very niche, there are very few positions that require golang. Python for example pays much less, but it gives you more options, as it is widely requested. It is also requested in Data Science positions which I wish to eventually move into.