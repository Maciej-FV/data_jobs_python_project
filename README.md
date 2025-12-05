# **Overview**
---

This Python project has been created following Luke Barousse's Course for Beginners. Link to the course: https://www.youtube.com/watch?v=wUSDVGivd-8.

Thanks to this course I have understood the basics of Python, such as:
- Variables
- Data Types
- Operators
- Functions
- Classes
- Python Standard Libraries
    - datetime
    - random
    - ast
- Libraries for Data Science
    - Numpy
    - Matplotlib
    - Seaborn
    - Pandas


This project focuses on the analysis of the data job market, focusing primarily on the Data Analyst roles in Data field. The main of this project is to help understand and navigate the job market more effectively. It points out the top-paying and in-demand skills that appear in job postings to help job seekers find optimal job opportunities. 

The data used for this entire project is provided by Luke Barousse and is from 2023-2024 period so it might be inaccurate for the job postings from the 2025, but still should be able to help and understand certain trends on the Data job market. 

Using *Python* scripts and libraries, this project explores the most important questions about job postings such as:
- the most demanded skills on the market
- salary
- interaction between high-demand skills and offered salary in Data Analysis.

## **The Questions Answered in this Project**

1. What are the most demanded skills in the top 3 most popular Data jobs?
2. What are the trends for in-demand skills that are expected from Data Analyst?
3. How well do jobs and skills associated with them pay?
4. What are the most optimal combinations of skills to learn for future Data Analysts? (In terms of High Demand Skills and High Paying Positions)

## **Tools Used in this Project**

To create this project, I have learned how to use following tools:

1. **Python**: The basis for the entire analysis.
    - **Pandas Library**: The Python library allowing its user to analyze the data.
    - **Matplotlib Library**: The Python library allowing to visualize the data.
    - **Seaborn Library**: The Python library used to further customize and improve visuals made with Matplotplib.
2. **Jupyter Notebooks**: The tool allowing to run Python scripts, analysis and creating notes throughout the entire process.
3. **Visual Studio Code**: A powerful code editor allowing to execute the Python scripts. 
4. **Git & GitHub**: Allows version control as well as sharing the Python code that stands behind this entire analytical project. 

## **Data Preparation and Clean-up**

This section explains the steps taken for the analysis of the data.

**Importing & Cleaning-up Data**

Each time, I have started by loading in the dataset and importing necessary libraries used for the analysis of the data.
 
    
    #importing libraries
    import ast
    import pandas as pd
    from datasets import load_dataset
    import matplotlib.pyplot as plt
    import seaborn as sns


    #loading data
    dataset = load_dataset('lukebarousse/data_jobs')
    df = dataset['train'].to_pandas()

    #quick cleanup
    df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
    df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x) 

**Filtering for Jobs in a particular country. (USA in this case)**

To narrow down the analysis, I focused on the USA and their job postings because the count of job postings in USA is the highest one. 


    df_US = df[df['job_country'] == 'United States']

**Filtering for Data Analyst Job Postings**

    df_US = df[df['job_title_short'] == 'Data Analyst']

## **The Analysis**
Each Jupyter Notebook in this course project aims at learning new Python concept or investigating particular aspects of the Data job market.

**1. What are the most in-demand skills for the top 3 most popular role in Data Science?**

To find the most in-demand skills for the top 3 most popular Data positions on the job market, I first looked for the Top 3 of the 8 job titles in 'job_title_short' by the amount of job postings, and I got the top 5 skills that are often required for becoming one of those Top 3 positions. 

The notebook with the query [here](/3_Python_Project/2_Skills_Count.ipynb)

**Data Visualization**

    fig, ax = plt.subplots(len(job_titles), 1)

    sns.set_theme(style='ticks')

    for i, job_title in enumerate(job_titles):
        df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
        sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:salmon_r')
        ax[i].set_title(job_title)
        ax[i].set_ylabel('')
        ax[0].set_xlabel('')
        ax[1].set_xlabel('')
        ax[2].set_xlabel('Likelihood Percentage')
        ax[i].legend().set_visible(False)
        ax[i].set_xlim(0, 100)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize= 15)
    fig.tight_layout(h_pad=0.5)

**Results**

![Visualization of Top Skills Likelihood](/3_Python_Project/images/likelihood_of_skills.png)

*Likelihood of Skill being required in job postings in the US*


**Insights:**
- SQL is the most desired skill for both Data Analyst (51%) and Data Engineer (68%) being present in more than half of the job postings for those roles. In case of Data Scientists, the most in-demand skill is Python with 72% of presence in job postings.
- Python is a versatile skill, often present in the job postings for all top 3 Data position on the job market. (Data Analyst - 27%, Data Engineer - 65%, Data Scientist - 72%)

**2. What are the trends of in-demand skills for Data Analysts?**

In order to find the trending skills for Data Analysts in 2023, Data Analyst postings have been filtered and grouped by skills according to the month of the job postings. This allowed to find the Top 5 skills of Data Analysts sorting by month, showing the skills trends in 2023. 

The detailed query is [here](/3_Python_Project/3_Skills_Trend.ipynb)

**Data Visualization**

    df_plot = df_DA_US_percentage.iloc[:, :5]

    sns.lineplot(data=df_plot, dashes=False, palette='dark:salmon_r')
    sns.set_theme(style='ticks')
    sns.despine()

    plt.title('The Trends for Top Skills of Data Analysts in the US')
    plt.ylabel('Likelihood in Job Postings')
    plt.xlabel('2023')
    plt.legend().remove()

    from matplotlib.ticker import PercentFormatter

    ax = plt.gca()
    ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

    for i in range(5):
        plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

**Results**

![The Trends of In-Demand Skills for Data Analysts in the US](/3_Python_Project/images/trends_of_skills_for_DA.png)

*Visualization of the trends of in-demand Data Analytic skills on the job market per month (in 2023)*

**Insights**
- SQL triumphs on the top of the list for the most in-demand skills for Data Analysts in 2023. However it shows a slight decrease in demand in job postings.
- Excel's demand increased around June till August where the demand for this particular skill started dropping only to increase once again in November.
- Visualization applications like Tableau and PowerBI remain stable with minor fluctuations throughout the year of 2023. PowerBI remains less demanded but it slowly gains popularity at the end of the year while Tableau's popularity slightly decreases at the same time.
- Python's demand in Data Analytics is stable. More job offers with Python in demand appear in the period of time between October and December. 


## 3. How well do jobs and skills pay for Data Roles?

Entire query is [here](/3_Python_Project/4_Salary_Analysis.ipynb)

### Salary Analysis

#### Visualization of the Data

    sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
    sns.set_theme(style='ticks')

    plt.title('Salary Distribution in the United States')
    plt.xlabel('Yearly Salary ($USD)')
    plt.ylabel('')
    ax = plt.gca()
    ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
    plt.xlim(0,600000)
    
#### Results 

![Distribution of the Salary by the top 6 Data Roles](/3_Python_Project/images/salary_distribution_by_top6.png) 

*Salary Distribution of the Salary by the Top 6 Data Roles*

#### Insights
- The median salaries improve when achieving the Senior equivalents of the respective roles. However, both Data Analysts and Senior Data Analyst remain at the bottom of the graph with salaries lower than the rest of the Top 6 (Data Engineer, Senior Data Engineer, Data Scientist, Senior Data Scientist).
- Data Scientist has a high variety of outliers with some of them reaching over $500 thousand dollars (but those are quite rare and can be treated as exceptions, especially when such outliers do not appear in the case of its Senior equivalent, which in the terms of median salary earns more than it's non-Senior role.) Those outliers might suggest the high value in experience and advanced data skills.  


### Highest Paid and Most Demanded Skills for Data Analysts

#### Visualization of the Data

    

    fig, ax = plt.subplots(2,1)


    sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], legend=False, hue='median', palette ='dark:salmon_r')
    ax[0].set_title('Top 10 Paid Skills for Data Analyst')
    ax[0].set_ylabel('')
    ax[0].set_xlabel('')
    ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


    sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], legend=False, hue='median', palette = 'dark:salmon_r')
    ax[1].set_xlim(ax[0].get_xlim())
    ax[1].set_title('Top 10 Most Popular Data Analyst Skills')
    ax[1].set_ylabel('')
    ax[1].set_xlabel('Median Salary ($USD)')
    ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

    sns.set_theme(style = 'ticks')
    fig.tight_layout()

#### Results

![Top Paid, Demanded and Popular Skills for Data Analyst](/3_Python_Project/images/top_paid_&_popular_skills.png)

*Two bar graphs visualizing the highest paid skills and most demanded skill for US Data Analyst.*

#### Insights
- The top graph shows that highly specialized skills such as 'dplyr', 'Bitbucket', and 'Gitlab' are paid higher salaries showing that advancing with more technical skills can increase the salary. 
- The bottom graph shows the most popular and in-demand skills that often are the foundation for Data Analyst such as: 'SQL', 'Excel', 'Python', 'Tableau'. They may not offer the highest salaries but they show that foundations are necessary for an employment in Data Analytics.
- There is a clear difference between the most popular and top paid skills in terms of salary. If one wants to improve their salary, one should learn how to use specialized skills for certain niches. 

## 4. What is the most optimal skill to learn for Data Analysts?

Entire query is [here](/3_Python_Project/5_Optimal_Skills.ipynb)

#### Visualize Data

    sns.scatterplot(
    data= df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
    )

    sns.despine()
    sns.set_theme(style='ticks')

    texts=[]
    for i, txt in enumerate(df_DA_skills_high_demand.index):
        texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))


    adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))


    plt.xlabel('Percentage of Data Analysts Job Postings')
    plt.ylabel('Median Yearly Salary')
    plt.title('Most Optimal Skills for Data Analyst in the US')

    ax = plt.gca()
    ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
    ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))


    plt.tight_layout()
    plt.show()

![Most Optimal Skills for Data Analysts in the US](/3_Python_Project/images/most_optimal_skills_DA.png)

*A scatter plot visualizing the most optimal skills for Data Analysts (as in high pay and high demand).*

#### Insights
- The scatter plot shows that the majority of the 'programming' skills (that are colored blue) tend to have a higher salary levels compared to other Data Analysts skills, which suggests that getting to know certain programming languages may positively affect one's salary.
- Analyst tools that are colored green, including PowerBi or Tableau are often present in the job postings  showing that visualization is an important aspect of Data Analytics. Not only those two skills provide a good salary but also versatile opportunities. 
- The database skills that are orange on the graph, such as, Oracle or SQL Server provide some of the highest Data Analyst salaries. This shows a significant demand for management and manipulation of data in the industry.



