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
