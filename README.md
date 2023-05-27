# Analyzing-Spotify-and-YouTube-Songs-Using-Python-MySQL
Data science project - Analyzing Spotify and YouTube Songs Using Python &amp; MySQL

# Project Description
This project will help you understand how a real-world database is analyzed using SQL, how to get maximum available insights from the dataset, pre-process the data using Python for better upcoming performance, how a structured query language helps us retrieve useful information from the database

The Project will consist of 2 modules:
- Module 1: Data Pre-processing data using Python
- Module 2: Analyzing data using SQL


![info.jpeg](images/project.jpeg)

***

## Data Source
Data was provided by HiCounselor.

# Data Analysis Concepts Utilized In This Project 

Data Preprocessing Using Python Programming and connecting to Database : 
- Droping unnecessary columns
- Droping null values 
- Droping duplicate values
- Data type conversion 
- Renaming the column
- Droping records which starts with special characters
- Creating new column on the basis of some conditions
- Saving newly cleaned dataset into csv file format
- Converting dataframe into SQL Table, Uploading Table into phpmyadmin server and solving queries. 

Using SQL Queries To Solve Problem Statements :
- Aggregating the data
- Grouping the data
- Ordering the data 
- Using addition of two columns to show as a calculated column

***

# Module 1: Data Preprocessing using python:
Data Pre-processing is one of the important steps in data analytics because data that is not processed can lead to different unwanted results when the data will be used for further applications. This task includes sub-tasks such as handling null values, deletion or transformation of irrelevant values, datatype transformation, removing duplicates, etc

- Task 1 : Column removal: Streamlining data for analysis (Eliminate unnecessary columns from the dataset for our analysis by removing Url_spotify, Uri, Key, Url_youtube, and Description).
- Task 2 : Null Value Analysis :Assesing data completeness and column-wise null sum (Examine the dataset for the presence of null values and calculate the total count of null values for each column, providing insights into the data's completeness and potential data quality issues).
- Task 3 : Null Value Handling (Manage null values in data for improved data quality and analysis and return dataframe).
- Task 4 : Duplicate Check And First Value Retention : Data deduplication for accuracy(Identify and eliminate duplicate records in the dataset while retaining the first occurrence of each unique value. This ensures data integrity by removing redundant information and maintaining the original data structure).
- Task 5 : Millisecond to Minute Conversion : Enhancing Time Duration Clarity(Convert the duration in milliseconds to minutes, facilitating a clearer comprehension and representation of time intervals in a more user-friendly format).
- Task 6 : Column Renaming : Enhancing clarity with duration in minutes(Change the name of the modified column to "Duration_min" to accurately reflect the conversion from milliseconds to minutes, providing a more descriptive and meaningful representation of the data).
- Task 7 : Exclusion of irrelevant tracks : Filtering out '?' prefix track names(Eliminate track names that are deemed irrelevant and begin with the "?" character, ensuring the dataset only includes relevant and meaningful track information for further analysis or processing).
- Task 8 : Energy to Liveness Ration Calculation : Analysing the relationship and storing results(Compute the Energy to Liveness ratio for each track, quantifying the relationship between energy and liveliness attributes. The resulting ratios are then stored in a column named 'EnergyLiveness' for further analysis or interpretation).
- Task 9 : Data Type Conversion : Transforming 'Views' to float for enhanced usability(Modify the data type of the 'views' column to float, enabling numerical operations and facilitating its utilization in subsequent analysis or calculations requiring floating-point values.
- Task 10 : Platform Dominance Analysis : Identifying most played platform and creating 'Most_Playedon' column(Analyze the 'views' and 'stream' columns to determine the dominant platform (YouTube or Spotify) on which a song track was most played. Create a new column called 'most_playedon' with values 'Spotify' or 'YouTube' indicating the platform with the highest play count for each song track).
- Task 11 : Data Export and Download : Saving and accessing 'Cleaned_dataset.csv'(Export the data to a CSV file named "cleaned_dataset.csv" and enable downloading by providing a clickable file name, allowing users to access and retrieve the file with ease).
- Task 12 : Module 1 completion : Creating MySQL Table from exported 'Cleaned_dataset.csv'(Create a MySQL table named "cleaned_dataset" by utilizing the exported file, "cleaned_dataset.csv". Follow these steps:  1. Download the CSV file.  2. Create the table using the CSV file, either through an online editor or by executing SQL commands.  3. Click "Run Test" to conclude Module 1).

# All python codes for Data-Preprocessing and converting to new csv file.


```python

import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings("ignore")


# First step is to get dataframe by calling read_csv function of pandas
data=pd.read_csv('Uncleaned_employees_final_dataset.csv')

# Now we should check null values in our dataframe
data.isnull().sum()

# As we have null values in 2 columns , So we will remove those records from our dataframe
data.dropna(inplace=True)

# Now we should remove duplicate records from our dataframe
data.drop_duplicates(keep="first",inplace=True)


# Now we will check if each column has relevant data type or not
data.dtypes

# Here we can see that one column called previous_year_rating is in floating data type , while it should be integer.
# So we will convert this floating data type into integer64.
data["previous_year_rating"]=data["previous_year_rating"].astype("int64")
    

# Now we will save this cleaned dataframe as "Cleaned_employees_final_dataset.csv" in a csv file format.
data.to_csv("Cleaned_employees_final_dataset.csv",index=False) 
```
***


# Module 2: Data Analysis using SQL

- In this module, we worked on performing data analysis on the pre-processed data from the previous module and conducting Data Analysis using SQL.
- We generated queries for given problem statements. We performed queries using SQLITE connection inside python program. 

 
### Here, First we will load our cleaned csv dataset named "Cleaned_employees_final_dataset.csv" into a new dataframe.
```python
data1 = pd.read_csv("Cleaned_employees_final_dataset.csv")

# We will Create a SQLite in-memory database and establish a connection
conn = sqlite3.connect(':memory:')

# Store the DataFrame in a SQL table
data1.to_sql('employee', conn, index=False)
```
***

## Task 1 :Find the average age of employees in each department and gender group. ( Round average age up to two decimal places if needed).
```python
##code
# Solution:
  query = "select department,gender,round(avg(age),2) as average_age from employee group by department,gender"
  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result1)
```


               department gender  average_age
    0           Analytics      f    29.88
    1           Analytics      m    33.54
    2             Finance      f    33.33
    3             Finance      m    33.15
    4                  HR      f    35.57
    5                  HR      m    34.98
    6               Legal      f    33.72
    7               Legal      m    34.69
    8          Operations      f    36.37
    9          Operations      m    36.88
    10        Procurement      f    36.86
    11        Procurement      m    36.73
    12                R&D      f    31.69
    13                R&D      m    33.40
    14  Sales & Marketing      f    34.90
    15  Sales & Marketing      m    35.90
    16         Technology      f    35.76
    17         Technology      m    35.87


***

## Task 2 :List the top 3 departments with the highest average training scores(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "select department,round(avg(avg_training_score),2)as avg_training_score from employee group by department order by avg_training_score desc limit 3"
  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

       department  avg_training_score
    0   Analytics               84.59
    1         R&D               84.42
    2  Technology               79.81


***

## Task 3 :Find the percentage of employees who have won awards in each region.(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "WITH CTE(REGION,TOTAL) AS (SELECT REGION,COUNT(*) FROM employee WHERE AWARDS_WON=1 GROUP BY REGION) SELECT REGION,ROUND(TOTAL*100/(SELECT COUNT(*) FROM employee WHERE AWARDS_WON=1),2) AS AWARDS_PERCENTAGE FROM CTE"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

           region  AWARDS_PERCENTAGE
    0   region_10              2.71
    1   region_11              1.63
    2   region_12              0.81
    3   region_13              4.88
    4   region_14              1.08
    5   region_15              6.23
    6   region_16              1.90
    7   region_17              3.79
    8   region_19              0.54
    9    region_2             20.33
    10  region_20              1.63
    11  region_21              0.27
    12  region_22              9.21
    13  region_23              2.17
    14  region_24              0.27
    15  region_25              1.36
    16  region_26              2.71
    17  region_27              5.15
    18  region_28              1.63
    19  region_29              2.17
    20   region_3              1.63
    21  region_30              0.54
    22  region_31              2.98
    23  region_32              1.36
    24  region_33              0.54
    25  region_34              0.81
    26   region_4              4.88
    27   region_5              1.36
    28   region_6              2.17
    29   region_7             10.30
    30   region_8              1.63
    31   region_9              1.36


***

## Task 4 :Show the number of employees who have met more than 80% of KPIs for each recruitment channel and education level.
```python
##code
# Solution :
  query = "select recruitment_channel,education,count(employee_id)as total_employee from employee where `KPIs_met_more_than_80` =1 group by recruitment_channel,education"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```


      recruitment_channel        education  total_employee
    0               other        Bachelors       2044
    1               other  Below Secondary         38
    2               other  Masters & above       1003
    3            referred        Bachelors        124
    4            referred  Below Secondary          2
    5            referred  Masters & above         33
    6            sourcing        Bachelors       1574
    7            sourcing  Below Secondary         34
    8            sourcing  Masters & above        723


***

## Task 5 :Find the average length of service for employees in each department, considering only employees with previous year ratings greater than or equal   to 4(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "select department,round(avg(length_of_service),2)as average_length_of_service from employee where previous_year_rating>= 4 group by department"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department  average_length_of_service
    0          Analytics                5.53
    1            Finance                5.70
    2                 HR                6.07
    3              Legal                4.79
    4         Operations                6.83
    5        Procurement                6.63
    6                R&D                4.86
    7  Sales & Marketing                6.33
    8         Technology                6.31

***


## Task 6 :List the top 5 regions with the highest average previous year ratings.(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "select region,round(avg(previous_year_rating),2) average_previous_year_rating from employee group by region order by average_previous_year_rating desc limit 5"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

          region  average_previous_year_rating
    0  region_25                 3.58
    1   region_4                 3.53
    2   region_8                 3.50
    3  region_28                 3.49
    4  region_23                 3.47


***

## Task 7 :List the departments with more than 100 employees having a length of service greater than 5 years.
```python
##code
# Solution :
  query = "select department,count(*)as total from employee where length_of_service>5 group by department having total>100"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department     total
    0          Analytics      591
    1            Finance      300
    2                 HR      332
    3         Operations     1720
    4        Procurement     1023
    5                R&D      115
    6  Sales & Marketing     2204
    7         Technology      896

***


## Task 8 :Show the average length of service for employees who have attended more than 3 trainings, grouped by department and gender.(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "select department,gender,round(avg(length_of_service),2)as average_length_of_service from employee where no_of_trainings>3 group by department,gender"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

               department gender  average_length_of_service
    0           Analytics      m     4.80
    1             Finance      m     3.50
    2          Operations      f     7.50
    3          Operations      m     4.25
    4         Procurement      f     4.80
    5         Procurement      m     5.92
    6                 R&D      m     3.71
    7   Sales & Marketing      f     5.00
    8   Sales & Marketing      m     5.44
    9          Technology      f     6.50
    10         Technology      m     7.85

***


## Task 9 :Find the percentage of female employees who have won awards, per department. Also show the number of female employees who won awards and total female employees.(Round average age up to two decimal places if needed).
```python
##code

# Solution :
  query = "select department, round(sum(case when awards_won=1 then 1 else 0 end)/ count(*)*100,2) as percentage_of_female_employees_won_awards,sum(case when awards_won=1 then 1 else 0 end) as female_employees_won_awards,count(*) as total_female_employees from employee where gender="f" group by department"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department  percentage_of_female_employees_won_awards   female_employees_won_awards    total_female_employees
    0          Analytics              2.29                                    3                             131
    1            Finance              2.22                                    4                             180
    2                 HR              1.31                                    4                             306
    3              Legal              4.00                                    1                             25
    4         Operations              3.08                                    42                            1365
    5        Procurement              3.35                                    31                            926
    6                R&D              0.00                                    0                             16
    7  Sales & Marketing              2.14                                    19                            889
    8         Technology              2.20                                    17                            772

***


## Task 10 :Calculate the percentage of employees per department who have a length of service between 5 and 10 years.(Round average age up to two decimal places if needed).
```python
##code
# Solution :
  query = "with cte as (select department,count(*) as a from employee group by department) select e.department,round(count(*)*100/(cte.a),2) from employee e left join cte on cte.department=e.department where length_of_service between 5 and 10 group by e.department"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department  employee_percent_per_dpt
    0          Analytics        47.63
    1            Finance        47.35
    2                 HR        45.73
    3              Legal        37.98
    4         Operations        49.00
    5        Procurement        48.19
    6                R&D        46.49
    7  Sales & Marketing        48.45
    8         Technology        45.92


***

## Task 11 :Find the top 3 regions with the highest number of employees who have met more than 80% of their KPIs and received at least one award, grouped by department and region.
```python
##code
# Solution :
  query = "select department,region,count(*) as total_employees from employee where KPIs_met_more_than_80 and awards_won >=1 group by department,region order by total_employees desc limit 3"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department    region  total_employees
    0  Sales & Marketing  region_2     19
    1        Procurement  region_2     13
    2  Sales & Marketing  region_7     10

*** 

## Task 12 :Calculate the average length of service for employees per education level and gender, considering only those employees who have completed more than 2 trainings and have an average training score greater than 75(Round average age up to two decimal places if needed).
```python
##code

# Solution :
  query = "select education,gender,round(avg(length_of_service),2) as average_service from employee where no_of_trainings > 2 and avg_training_score > 75 group by education,gender"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

             education gender  average_service
    0        Bachelors      f     5.71
    1        Bachelors      m     4.53
    2  Below Secondary      m     1.67
    3  Masters & above      f     6.00
    4  Masters & above      m     6.70

***


## Task 13 :For each department and recruitment channel, find the total number of employees who have met more than 80% of their KPIs, have a previous_year_rating of 5, and have a length of service greater than 10 years.
```python
##code

# Solution :
  query = "select department,recruitment_channel,count(*) as total_employees from employee where KPIs_met_more_than_80 =1 and previous_year_rating = 5 and length_of_service > 10 group by department,recruitment_channel"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)

```
               department recruitment_channel  total_employees
    0           Analytics               other         17
    1           Analytics            sourcing          3
    2             Finance               other          3
    3             Finance            sourcing          4
    4                  HR               other          4
    5                  HR            referred          1
    6                  HR            sourcing          3
    7               Legal               other          2
    8               Legal            sourcing          2
    9          Operations               other         55
    10         Operations            sourcing         39
    11        Procurement               other         24
    12        Procurement            sourcing         14
    13                R&D               other          1
    14                R&D            sourcing          1
    15  Sales & Marketing               other         27
    16  Sales & Marketing            sourcing         19
    17         Technology               other         10
    18         Technology            sourcing         12

***


## Task 14 :Calculate the percentage of employees in each department who have received awards, have a previous_year_rating of 4 or 5, and an average training score above 70, grouped by department and gender(Round average age up to two decimal places if needed).

```python
##code
# Solution :
  query = "with table1 as (select department,gender,count(*) as total2 from employee group by department,gender), table2 as (select department,gender,count(*) as total1 from employee where previous_year_rating in (4,5) and avg_training_score > 70 and awards_won = 1 group by department,gender) select table1.department,table1.gender,ifnull(round(table2.total1 * 100 / (table1.total2),2),0) as percentage_of_employees from table1 left join table2 on table1.department=table2.department and table1.gender=table2.gender"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

               department gender  percentage_of_employees
    0           Analytics      f       1.53
    1           Analytics      m       1.15
    2             Finance      f       1.11
    3             Finance      m       0.19
    4                  HR      f       0.33
    5                  HR      m       0.00
    6               Legal      f       0.00
    7               Legal      m       0.00
    8          Operations      f       0.51
    9          Operations      m       0.36
    10        Procurement      f       0.65
    11        Procurement      m       0.70
    12                R&D      f       0.00
    13                R&D      m       1.41
    14  Sales & Marketing      f       0.22
    15  Sales & Marketing      m       0.19
    16         Technology      f       1.42
    17         Technology      m       1.65


***

## Task 15 :List the top 5 recruitment channels with the highest average length of service for employees who have met more than 80% of their KPIs, have a previous_year_rating of 5, and an age between 25 and 45 years, grouped by department and recruitment channel.(Round average age up to two decimal places if needed).

```python
##code

# Solution :
  query = "select department,recruitment_channel,round(avg(length_of_service),2)as avg_length_service from employee where KPIs_met_more_than_80=1 and previous_year_rating=5 and age between 25 and 45 group by department,recruitment_channel order by avg_length_service desc limit 5"

  result = pd.read_sql_query(query, conn)

# Displaying the query result :
  print(result)
```

              department recruitment_channel  avg_length_service
    0         Operations            referred     6.20
    1         Operations               other     6.08
    2         Operations            sourcing     5.93
    3         Sales & Marketing        other     5.77
    4         Procurement            sourcing    5.65

***
