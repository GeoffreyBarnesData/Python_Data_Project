# Overview
In this project I seek to analyze job postings data for specifically data related jobs. This includes Data Analysts, Data Scientists, Data Engineers and many more titles. I am seeking to visualize the information so that I can gleam insights, and practice making presentation ready graphs through the use of matplotlib and seaborn. 

The data I am using comes from Luke Barousse's data that he collected for his Data Analytics with Python course. I will use the data differently than him, filtering out for Canadian jobs where the data is good enough to use. If not, I will switch over to the American data.

# The Questions
1. What are the most in demand skills for the most popular data roles?
2. Are any skills becoming obsolete? Are any skills on the rise?
3. How well do jobs pay for various titles? How much does each skill net you?
4. Specifically for Data Analysts, what are the most valuable skills to learn?

# Tools I Used
1. Python
2. matplotlib
3. seaborn
4. pandas
5. ast
6. NumPy
7. Git and GitHub

# Data Preparation and Cleanup
To prepare the data, the first thing I did was to drop NA values that were unnecessary. For example:

```Python
#Cleaning the data
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

I also filtered out the data for Canadian or United States roles, as well as filtering for only Data Analysis Roles. Here's another example: 

```Python
#I only want to see American jobs in Data Analysis, so I'll filter for them
df_DA_US = df[(df['job_country'] == 'United States') & (df['job_title_short'] == 'Data Analyst')].copy()
```

I also filtered the skills required, and narrowed them down to a more managable list ranked by how often they appear in job listings.

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

![Skills Required For Various Job Postings In Canada](Images\Skills_Required_For_Various_Job_Postings_In_Canada.png)

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

![Salary Distribution in the top 8 Canadian Data Jobs](Images\Salary_Distribution_in_Canadian_Jobs.png)


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

![Top 10 Most In-Demand Skills for Data Analysts](Images\Top_10_Highest_Paid_Skills_for_Data_Analysts.png)

### Insights:

-I learned that sometimes, in cleaning the data you don't always have enough good data to gleam the information that you need. I had to switch over to using US data instead, and I was able to create a stunning visual (above) with this data.

-I learned that the highest paying skills are very niche, there are very few positions that require golang. Python for example pays much less, but it gives you more options, as it is widely requested. It is also requested in Data Science positions which I wish to eventually move into.

# The Analysis

## What is the most optimal skill to focus on?

### Insights:
- With this scatter plot, we can see that most of the 'programming' skills (blue in the scatterplot) tend to cluster at higher salaries than other skills. Skills like word and powerpoint are less likely to land you a high paying job. Due to how easy they are to learn and use, it is still worth having some skill in those if needed. We also learned that visualization tools like Power BI are associated with some high paying positions, Power BI as an example is in the middle of the graph, so it pays moderately well and is often but not always requested.

- Python is at the top of the graph slightly to the right. This means it is a good skill to learn for higher pay, and can frequently get you a job, but the most frequently requested skill is SQL. I already have done a course on SQL so I am comfortable with it, so that bodes well for me. With the combination of my python skills and my SQL skills I should expect a higher paying job than the average, but because of my work experience I would most likely get a junior position. 

- The analyst tool that is both in demand and pays well is tableau. I will likely need only one of these tools to visualize data, so tableau is the most logical place to start. Excel, primarily for its pivot table functions is a frequent requirement. The usefulness of it is in its frequency of use. If I am to work with other people, they are very likely to use excel, so teamwork likely requires excel skills. Luckily I have experience with excel from school and courses I've completed afterwards.

- These insights are based off of US data, and they will not always comport with Canadian data, but I am open to working a remote job for a US based company, so these insights are still useful to me. Also the United States is generally pretty similar to Canada in a lot of aspects, so if I had better Canadian data to work with I would expect it to be similar. In playing with the data I found that the only large difference in job postings between Canada and the United States is the healthcare the company provides. Canadian companies very rarely provide healthcare based on this data but United States companies more often do.

![Most Optimal Skills for Data Analysts in the US](Images\Most_Optimal_Skills_for_Data_Analysts_in_the_US.png)


# What I Learned
- I learned how to deal with problems in the code that don't give clear error messages and do not have straightforward solutions. For instance I loaded in data that I thought would be in the form of a list, but instead it was a string that looked like a list. It looked something like this: '[python, excel, SQL]' and was just a string.
- I learned not to give up on a problem, as commiting to GitHub would cause an unknown error, and I learned to use the console to commit instead, bypassing the error.
- I learned to use AI to find errors in code, and suggest new lines that would fix my problems. This didn't always work but the suggestions I got were helpful to me, as I learned more about libraries like seaborn and learned new options to play around with. This made my graphing skills improve, and I solved the issues I was having.
- I learned how to ask for help with coding problems. I asked people who are experienced with coding, even if not with data specific problems. They gave me insights and asked questions that brought me to solutions and more information about what I was working with. Through attempting to understand problems, I learned more and more about how various libraries work and what can cause them to break and return an error.
- I learned that I don't have to fear warnings, as I can ask if this will effect the code later down the line, and if the answer is no, I don't have to spend time making sure every warning disappears. Warnings can be insightful but they are not like errors. You can work towards your goal and take note of what a warning is telling you. For this project the warnings I got in part 2 did not effect that part, or any other part later down the line.

# Insights
- From this project I learned that Python, SQL, and Tableau are extremely useful skills. I have knowledge of Python and SQL, but I can still go deeper into these languages to better my skill level. 
- Tableau is something I am not too familiar with, so that is a good place to start to continue my learning. This project sent me down a road where I learned that Tableau is easier to learn than a programming language, so a job offer that lists Tableau is within my grasp.
- I also learned that python and SQL are still in demand and are not trending downwards. This means they are not likely to become obsolete skills, and I have another reason to invest more time into learning them.
- I also learned that for higher paying jobs, I may have to learn rare skills that are not frequently requested in many job listings. These skills do not have a general use, so it would be better for me to save my time for Python, SQL and Tableau until I need a rare skill when the time comes.

# Conclusion
I started out not knowing exactly what this data would hold, or what skill level I would be at by the end. I learned Python in university as well as some other data related skills, but I learned a lot of specific knowledge about data related python libraries like pandas. I learned very important coding skills, such as solving errors in the code, and asking both AI and people with work experience for help. I learned to help myself as well, thinking about my code and analyzing what is going wrong, and what could be better. I learned that sometimes, as in the case with Canadian job data, there is no more you can learn from the data, and you have to broaden your search through the data in order to gain insights. I learned that Python, SQL and Tableau are important skills to know if you want a high paying job, and they are frequently requested, so as a job seeker I need to deepen my understanding of these languages and their various libraries. I learned that even though this project took a lot of time and a ton of effort, I was able to complete it and even add onto it further than I expected. I persevered through the tough parts and in the end I was rewarded with experience working with code and dataframes and I was rewarded with insights from the data.