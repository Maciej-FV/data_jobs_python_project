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
---
This section explains the steps taken for the analysis of the data.

**Importing & Cleaning-up Data**
Each time, I have started by loading in the dataset and importing necessary libraries used for the analysis of the data.

'code' 
    
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


    df_US = df[df['job_country'] == 'United States]

**Filtering for Data Analyst Job Postings**

    df_US = df[df['job_title_short'] == 'Data Analyst]

## **The Analysis**
Each Jupyter Notebook in this course project aims at learning new Python concept or investigating particular aspects of the Data job market.







