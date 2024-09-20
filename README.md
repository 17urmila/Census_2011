# Census_2011
# Project Title
Census Data Standardization and Analysis Pipeline

## Colab notebook link
https://colab.research.google.com/drive/1qXNh1aCcXbNJx2rzY0SY-WROwVWkCMOq#scrollTo=7iyHsjzRe7R2

## Dataset URL

https://docs.google.com/spreadsheets/d/1YWdOUGGFy1h-WKuXtkiKDTm3jcEY4Y7XuSZt_rzXtgc/edit?gid=1084285986#gid=1084285986

## Technologies used
Python, SQL, MongoDB, Streamlit

## Prblem Statement
The task is to clean, process, and analyze census data from a given source. The goal is to ensure uniformity, accuracy, and accessibility of the census data for further analysis and visualization.

### Script to read the csv file 

import pandas as pd
df=pd.read_csv('/content/census.csv')

### Task 1 : Rename the Column names
For uniformity in the dataset we need to rename some columns.

For example, State name  to State/UT, District name  to District.

rename function used to rename the column names.

### Task 2 : Rename State/UT Names
The State/UT names are in all caps in the census data.
For unifomity the names are changed into title case.

For uniformity across dataset, only the first character of each word in the name is in upper case and the rest are in lower case. However, if the word is “and” then it should be all lowercase.

replace and title function used to rename the State/UT names.

### Task 3 : New State/UT formation
In 2014 Telangana was formed after it split from Andhra Pradesh.
The State/UT name is changed from Andhra Pradesh to Telangana for the Districts that comes under Telangana State.
In 2019 Ladakh was formed after it split from Jammu and Kashmir.
The State/UT name is changed from Jammu and Kashmir to Ladakh for the Districts that comes under Ladakh. 

### Task 4 : Find and process Missing Data
Missing data can be found by using isna()/isnull() and sum() function.
Before filling missing data ,the percentage of missing data is calculated and stored in a dataframe called na_df for comparing. 
Missing data can be filled by finding the correct data by using information from other cells.
For example we can calculate the missing data of 'Population' column by adding the values of Columns named 'Male' and 'Female'.
Data filling can be done by using fillna().
##Code for comparing the amount of missing data before and after data cleaning for each column.

--Count of missing data before  data cleaning.
na_count=df.isna().sum()
na_df=na_count.reset_index()
na_df.columns = ['Columns','Missing_data_before_cleaning']
na_df['Missing_data_percent_before_cleaning']=(na_df['Missing_data_before_cleaning']/len(df))/100

na_count1=df.isna().sum()
na_df1=na_count1.reset_index()
na_df1.columns = ['Columns','Missing_values']
na_df['Missing_data_after_cleaning']=na_df1['Missing_values']
na_df['Missing_data_percent_after_cleaning']=(na_df1['Missing_values']/len(df))/100
na_df

### Task 5 : Save Data to MongoDB

--Pymongo installation is done

!python -m pip install "pymongo[srv]"

The processed data is saved to mongoDB with a collection named called 'Census'.

### Task 6 : Database connection and data upload

Sqlalchemy is installed 
--Installing sqlalchemy to insert DataFrame into database

!pip install sqlalchemy

Data is fetched from mongoDB and uploaded to a relational database(SQLite) using python code.

### Task 7 : Run Query on the database and show output on streamlit

--Streamlit installation 

!pip install streamlit -q

--Localtunnel installation

!npm install -g localtunnel

Given queries are written in an app.py file to run on the database and output is shown on streamlit
