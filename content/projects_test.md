+++
categories = []
date = ""
description = ""
draft = true
image = ""
tags = []
title = "Projects_test"
type = "post"

+++
\# 1. The data

This project explores the quality of life across various European countries by measuring various indicators such as GDP, unemployment, underemployment, median income, pollution, average hours worked, percentage of population with low savings, crime, job satisfaction rates, life satisfaction rates and trusr in the police, legal system and politics.

The data was drawn from the Eurostat database: [https://ec.europa.eu/eurostat/data/database](https://ec.europa.eu/eurostat/data/database "https://ec.europa.eu/eurostat/data/database") which contains a wide range of statistical data about the EU.

Each of the 18 tables in the database explores one indicator across 31 countries.

The challenge in this project is 1) to join all or some of tables to compare countries' performance and 2) to draw interesting insights and conclude which EU country might have the best quality of life overall.

\# 2. Load csv files into a Postgres database using pandas and sqlalchemy

Copying csv data into a Postgres database can be time consuming and sometimes tricky task. Pandas to the rescue!

Assuming you have already set up your Postgres server and created a database, the process is quite simple and fast:

\- use pandas to convert the csv file into a dataframe

\- use the sqlalchemy library to connect to the database

\- pass the data from the dataframe into a Postgres table within your database.

Note: I am using the Postgres interface, pgAdmin ([https://www.pgadmin.org/](https://www.pgadmin.org/ "https://www.pgadmin.org/")) on Mac OS which makes it easy to create databases, tables etc and manage your database.

The main steps in the workflow are:

1\. Set up a Postgres server and create a database to store your data (this can be done on pgAdmin or by using the SQL command:

CREATE DATABASE <database_name>.

2\. Switch to Python to import your data:

a) import pandas

b) import the SQLAlchemy Python toolkit (if you don’t already have it, it can be installed with pip install SQLAlchemy (see below)).

3\. Load your CSV data with Pandas.

4\. Use SQLAlchemy to pass the data to Postgres:

Create an engine object with sqlalchemy's create_engine function to connect to the database.

The engine should be configured in this format:

create_engine("<dbms>://<username>:<password>@<hostname>:<port>/<db_name>")

The SQLAlchemy.create_engine will need the following 6 bits of information:

\- The DBMS — In this case, "postgresql"

\- A username for the database

\- The user’s password

\- The hostname for the Postgres server

\- The port for the Postgres server (5432 by default)

\- The name of the database to store the new table

For more info on the engine configuration see: [https://docs.sqlalchemy.org/en/13/core/engines.html](https://docs.sqlalchemy.org/en/13/core/engines.html "https://docs.sqlalchemy.org/en/13/core/engines.html")

Below is what the workflow looks like in code.

\`\`\`python

pip install SQLAlchemy

\`\`\`

    Requirement already satisfied: SQLAlchemy in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (1.3.18)
    
    Note: you may need to restart the kernel to use updated packages.

\`\`\`python

import pandas as pd

from sqlalchemy import create_engine

\#upload the data

df = pd.read_csv("crime_2016.csv")

\# Instantiate sqlachemy.create_engine object

engine = create_engine('postgresql://postgres:lovesql@localhost:5432/practicedb')

\# Save the data from dataframe to postgres table "crime"

\#Set index as False if you don't want to copy over the index

df.to_sql('crime', engine, index=False)

\`\`\`

Once you run the code, you can check that your table has been populated with the CSV data by going to pgAdmin and viewing the table. Note that in the case of the 'crime' table above, this table did not already exist in my database and so python cleverly created a new table in my database and inserted the CSV data into it.

However, you can also create an empty table in your database in advance (in pgAdmin) and insert data into it with python.

Below is an example of the same command as above, just with an added "if_exists=append" argument - if a table already exists in the database, the CSV data would be simply inserted into it (rather than creating a new table).

For this example, the process was repeated muliple times to load 17 different csv files into 17 tables in the database.

\`\`\`python

df = pd.read_csv('env_satisfaction_2013.csv')

df.to_sql('env_satisfaction', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('job_satisfaction_2013.csv')

df.to_sql('job_satisfaction', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('life_expectancy_2016.csv')

df.to_sql('life_expectancy', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('life_satisfaction_2013.csv')

df.to_sql('life_satisfaction', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('median_income_2016.csv')

df.to_sql('median_income', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('perceived_health_2016.csv')

df.to_sql('perceived_health', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('pollution_2016.csv')

df.to_sql('pollution', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('population_2011.csv')

df.to_sql('population', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('underemployment_2016.csv')

df.to_sql('underemployment', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('unemployment_2016.csv')

df.to_sql('unemployment', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('weather.csv')

df.to_sql('weather', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('work_hours_2016.csv')

df.to_sql('work_hours', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('trust_in_legal_2013.csv')

df.to_sql('trust_in_legal', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('trust_in_police_2013.csv')

df.to_sql('trust_in_police', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('low_savings_2016.csv')

df.to_sql('low_savings', engine, index=False, if_exists='append')

\`\`\`

\`\`\`python

df = pd.read_csv('trust_in_politics_2013.csv')

df.to_sql('trust_in_politics', engine, index=False, if_exists='append')

\`\`\`

\# 3. Query the Postgres database

The workflow of SQL querying of the database with python and SQL is as follows:

\- Import packages and functions

\- Create the database engine

\- Connect to the engine

\- Query the database

\- Save query results to a DataFrame

\- Close the connection

Let's explore.

\`\`\`python

\#Display all table names in the DB

table_names = engine.table_names()

print(table_names)

\`\`\`

    \['gdp', 'weather', 'job_satisfaction', 'life_expectancy', 'life_satisfaction', 'median_income', 'perceived_health', 'pollution', 'population', 'underemployment', 'unemployment', 'work_hours', 'low_savings', 'trust_in_politics', 'trust_in_police', 'trust_in_legal', 'crime', 'env_satisfaction'\]

\`\`\`python

\#Display all cols & rows in a specific table

\#import packages and functions

from sqlalchemy import create_engine

import pandas as pd

\#create DB engine

engine = create_engine('postgresql://postgres:lovesql@localhost:5432/practicedb')

\#connect to the engine

con = engine.connect()

\#query the DB - display all rows and cols from a table

rs = con.execute("SELECT * FROM gdp")

\#save the query results to a data frame

df = pd.DataFrame(rs.fetchall())

\#set the DF column names to match the column names in the DB table

df.columns = rs.keys()

\#close the connection

con.close()

\#print the query results

print(df)

\`\`\`

               country        gdp
    
    0           Cyprus    18490.2
    
    1           Latvia    25037.7
    
    2        Lithuania    38849.4
    
    3       Luxembourg      53303
    
    4          Hungary   113903.8
    
    5            Malta    10344.1
    
    6      Netherlands     708337
    
    7          Austria   356237.6
    
    8           Poland   426547.5
    
    9         Portugal   186480.5
    
    10         Romania   170393.6
    
    11        Slovenia    40357.2
    
    12        Slovakia    81226.1
    
    13         Finland     216073
    
    14          Sweden   463147.5
    
    15  United Kingdom  2403382.6
    
    16         Iceland    18646.1
    
    17          Norway   335747.5
    
    18     Switzerland   605753.7
    
    19          Turkey   780224.9
    
    20         Belgium   424660.3
    
    21        Bulgaria    48128.6
    
    22         Czechia   176370.1
    
    23         Denmark   282089.9
    
    24         Germany    3159750
    
    25         Estonia    21682.6
    
    26         Ireland   273238.2
    
    27          Greece   176487.9
    
    28           Spain    1118743
    
    29          France    2228568
    
    30         Croatia    46639.5
    
    31           Italy    1689824

\# 4. Using context managers

When you want to read data from a file, it is better to use the "with" keyword to do so because it automatically closes the file after you leave the scope of the “with statement", rather than having to remember to close the file when you’re done with it (as we've done above). Here's a code example on how to use a context manager for a simple query of our database.

\`\`\`python

from sqlalchemy import create_engine

import pandas as pd

\#create engine

engine = create_engine('postgresql://postgres:lovesql@localhost:5432/practicedb')

\#query the DB with context manager

with engine.connect() as con:

\#query the DB and set the results variable

    rs = con.execute("SELECT country, total_pop FROM population")

\#convert results into DF and fetch first 5 rows

    df = pd.DataFrame(rs.fetchmany(size=5))

\#set up DF column names

    df.columns = rs.keys()

\#display results

df

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>total_pop</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Belgium</td>
    
      <td>11000638</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Bulgaria</td>
    
      <td>7364570</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Estonia</td>
    
      <td>1294455</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Ireland</td>
    
      <td>4574888</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Greece</td>
    
      <td>10816286</td>
    
    </tr>

</tbody>

</table>

</div>

\`\`\`python

\#alternatively query the DB directly with pandas

df = pd.read_sql_query("SELECT * FROM population", engine)

print(df.head())

\`\`\`

        country   total_pop  prct_yng_adt_pop
    
    0   Belgium  11000638.0         18.485146
    
    1  Bulgaria   7364570.0         18.432577
    
    2   Estonia   1294455.0         19.688363
    
    3   Ireland   4574888.0         20.601335
    
    4    Greece  10816286.0         17.604416

\# 5. Advanced querying with pandas

\`\`\`python

df = pd.read_sql_query("SELECT g.country, gdp, total_pop, prct_yng_adt_pop FROM gdp AS g INNER JOIN population AS p ON g.country = p.country", engine)

print(df.head())

\`\`\`

          country       gdp  total_pop  prct_yng_adt_pop
    
    0      Cyprus   18490.2   840407.0         23.322628
    
    1      Latvia   25037.7  2070371.0         20.442858
    
    2   Lithuania   38849.4  3043429.0         20.526781
    
    3  Luxembourg   53303.0   512353.0         18.940652
    
    4     Hungary  113903.8  9937628.0         18.345122

\# 6. Querying  with SQL

Instead of using pandas to query the database, you can also query it directly in SQL as follows:

1\.install the ipython-sql library as shown below.

2\.use sqlalchemy to connect to the database and create an engine.

3\.create an SQL code block by marking it with %%SQL at the start.

4\.proceed with an SQL query in the usual way.

5\. test it with a brief SELECT * (select all) query to check it's working.

Here's what it looks likein code.

\`\`\`python

!pip install ipython-sql

\`\`\`

    Collecting ipython-sql
    
      Downloading ipython_sql-0.4.0-py3-none-any.whl (19 kB)
    
    Requirement already satisfied: sqlalchemy>=0.6.7 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython-sql) (1.3.18)
    
    Requirement already satisfied: six in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython-sql) (1.15.0)
    
    Requirement already satisfied: ipython-genutils>=0.1.0 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython-sql) (0.2.0)
    
    Requirement already satisfied: ipython>=1.0 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython-sql) (7.16.1)
    
    Collecting sqlparse
    
      Downloading sqlparse-0.4.1-py3-none-any.whl (42 kB)
    
    \[K     |████████████████████████████████| 42 kB 487 kB/s eta 0:00:01
    
    \[?25hCollecting prettytable<1
    
      Downloading prettytable-0.7.2.tar.bz2 (21 kB)
    
    Requirement already satisfied: decorator in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (4.4.2)
    
    Requirement already satisfied: jedi>=0.10 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (0.17.1)
    
    Requirement already satisfied: prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (3.0.5)
    
    Requirement already satisfied: pexpect; sys_platform != "win32" in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (4.8.0)
    
    Requirement already satisfied: pickleshare in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (0.7.5)
    
    Requirement already satisfied: backcall in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (0.2.0)
    
    Requirement already satisfied: pygments in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (2.6.1)
    
    Requirement already satisfied: traitlets>=4.2 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (4.3.3)
    
    Requirement already satisfied: setuptools>=18.5 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (49.2.0.post20200714)
    
    Requirement already satisfied: appnope; sys_platform == "darwin" in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from ipython>=1.0->ipython-sql) (0.1.0)
    
    Requirement already satisfied: parso<0.8.0,>=0.7.0 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from jedi>=0.10->ipython>=1.0->ipython-sql) (0.7.0)
    
    Requirement already satisfied: wcwidth in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0->ipython>=1.0->ipython-sql) (0.2.5)
    
    Requirement already satisfied: ptyprocess>=0.5 in /Users/racheldulberg/opt/anaconda3/lib/python3.8/site-packages (from pexpect; sys_platform != "win32"->ipython>=1.0->ipython-sql) (0.6.0)
    
    Building wheels for collected packages: prettytable
    
      Building wheel for prettytable (setup.py) ... \[?25ldone
    
    \[?25h  Created wheel for prettytable: filename=prettytable-0.7.2-py3-none-any.whl size=13698 sha256=773d3c687e794a51d8f6957ca8a221b22aa7d72babdc9d5e2dccc86ed09eb05b
    
      Stored in directory: /Users/racheldulberg/Library/Caches/pip/wheels/46/60/6c/bb25d05df22906786206e901e9354bb3061061191116768bee
    
    Successfully built prettytable
    
    Installing collected packages: sqlparse, prettytable, ipython-sql
    
    Successfully installed ipython-sql-0.4.0 prettytable-0.7.2 sqlparse-0.4.1

\`\`\`python

\#import libraries

import sqlalchemy

from sqlalchemy import create_engine

\#create the engine

engine = create_engine('postgresql://postgres:lovesql@localhost:5432/practicedb')

\`\`\`

\`\`\`python

\#load the previously installed SQL module

%load_ext sql

\#connect to the database with the connection string used in the engine after using the %sql prefix

%sql postgresql://postgres:lovesql@localhost:5432/practicedb

\`\`\`

\`\`\`python

\#test the connection with a simple query

%%sql

SELECT *

FROM gdp

LIMIT 5;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>gdp</th>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>18490.2</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>25037.7</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>38849.4</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>53303</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>113903.8</td>
    
    </tr>

</table>

\# a) Simple table joins

Next, we can explore countries' GDP, total population and the percentage of young adults in the population as these are good indicators for a country's economy size and demographics. To view this information we'll need to join the GDP table to the population table:

\`\`\`python

%%sql

SELECT gdp.country, gdp, total_pop, prct_yng_adt_pop

FROM gdp

INNER JOIN population

ON gdp.country = population.country;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>gdp</th>
    
        <th>total_pop</th>
    
        <th>prct_yng_adt_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>18490.2</td>
    
        <td>840407</td>
    
        <td>23.3226282027637</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>25037.7</td>
    
        <td>2070371</td>
    
        <td>20.442857825964502</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>38849.4</td>
    
        <td>3043429</td>
    
        <td>20.526780812037998</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>53303</td>
    
        <td>512353</td>
    
        <td>18.9406522456197</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>113903.8</td>
    
        <td>9937628</td>
    
        <td>18.3451221961619</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>10344.1</td>
    
        <td>417432</td>
    
        <td>20.590659077406603</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>708337</td>
    
        <td>16655799</td>
    
        <td>18.269979122586697</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>356237.6</td>
    
        <td>8401940</td>
    
        <td>18.6775435197109</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>426547.5</td>
    
        <td>38044565</td>
    
        <td>21.9158084735625</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>186480.5</td>
    
        <td>10562178</td>
    
        <td>17.0740447661458</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>170393.6</td>
    
        <td>20121641</td>
    
        <td>18.775327519261502</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>40357.2</td>
    
        <td>2050189</td>
    
        <td>18.308799822845604</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>81226.1</td>
    
        <td>5397036</td>
    
        <td>21.908599460889302</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>216073</td>
    
        <td>5375276</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>463147.5</td>
    
        <td>9482855</td>
    
        <td>19.3862924193189</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>2403382.6</td>
    
        <td>63182180</td>
    
        <td>19.942316013787398</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>18646.1</td>
    
        <td>315556</td>
    
        <td>21.5933146573033</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>335747.5</td>
    
        <td>4979954</td>
    
        <td>19.5865463817537</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>605753.7</td>
    
        <td>7954662</td>
    
        <td>18.4457868857281</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>780224.9</td>
    
        <td>7954662</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>424660.3</td>
    
        <td>11000638</td>
    
        <td>18.4851460433477</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>48128.6</td>
    
        <td>7364570</td>
    
        <td>18.4325765115954</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>176370.1</td>
    
        <td>10436560</td>
    
        <td>18.862489172677602</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>282089.9</td>
    
        <td>5560628</td>
    
        <td>18.0820044066965</td>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>3159750</td>
    
        <td>80219695</td>
    
        <td>17.105589842993</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>21682.6</td>
    
        <td>1294455</td>
    
        <td>19.688363056266898</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>273238.2</td>
    
        <td>4574888</td>
    
        <td>20.6013349397843</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>176487.9</td>
    
        <td>10816286</td>
    
        <td>17.604416155416</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>1118743</td>
    
        <td>46815910</td>
    
        <td>16.6743314398887</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>2228568</td>
    
        <td>64933400</td>
    
        <td>18.527500485112398</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>46639.5</td>
    
        <td>4284889</td>
    
        <td>18.5512623547541</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>1689824</td>
    
        <td>59433744</td>
    
        <td>15.4749732744415</td>
    
    </tr>

</table>

Below is the same query, but this time we're sorting the results so that the countries with the highest GDP come first (from highest to lowest).

\`\`\`python

%%sql

SELECT gdp.country, gdp, total_pop, prct_yng_adt_pop

FROM gdp

INNER JOIN population

ON gdp.country = population.country

ORDER BY gdp DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>gdp</th>
    
        <th>total_pop</th>
    
        <th>prct_yng_adt_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>3159750</td>
    
        <td>80219695</td>
    
        <td>17.105589842993</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>2403382.6</td>
    
        <td>63182180</td>
    
        <td>19.942316013787398</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>2228568</td>
    
        <td>64933400</td>
    
        <td>18.527500485112398</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>1689824</td>
    
        <td>59433744</td>
    
        <td>15.4749732744415</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>1118743</td>
    
        <td>46815910</td>
    
        <td>16.6743314398887</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>780224.9</td>
    
        <td>7954662</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>708337</td>
    
        <td>16655799</td>
    
        <td>18.269979122586697</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>605753.7</td>
    
        <td>7954662</td>
    
        <td>18.4457868857281</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>463147.5</td>
    
        <td>9482855</td>
    
        <td>19.3862924193189</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>426547.5</td>
    
        <td>38044565</td>
    
        <td>21.9158084735625</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>424660.3</td>
    
        <td>11000638</td>
    
        <td>18.4851460433477</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>356237.6</td>
    
        <td>8401940</td>
    
        <td>18.6775435197109</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>335747.5</td>
    
        <td>4979954</td>
    
        <td>19.5865463817537</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>282089.9</td>
    
        <td>5560628</td>
    
        <td>18.0820044066965</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>273238.2</td>
    
        <td>4574888</td>
    
        <td>20.6013349397843</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>216073</td>
    
        <td>5375276</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>186480.5</td>
    
        <td>10562178</td>
    
        <td>17.0740447661458</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>176487.9</td>
    
        <td>10816286</td>
    
        <td>17.604416155416</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>176370.1</td>
    
        <td>10436560</td>
    
        <td>18.862489172677602</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>170393.6</td>
    
        <td>20121641</td>
    
        <td>18.775327519261502</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>113903.8</td>
    
        <td>9937628</td>
    
        <td>18.3451221961619</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>81226.1</td>
    
        <td>5397036</td>
    
        <td>21.908599460889302</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>53303</td>
    
        <td>512353</td>
    
        <td>18.9406522456197</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>48128.6</td>
    
        <td>7364570</td>
    
        <td>18.4325765115954</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>46639.5</td>
    
        <td>4284889</td>
    
        <td>18.5512623547541</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>40357.2</td>
    
        <td>2050189</td>
    
        <td>18.308799822845604</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>38849.4</td>
    
        <td>3043429</td>
    
        <td>20.526780812037998</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>25037.7</td>
    
        <td>2070371</td>
    
        <td>20.442857825964502</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>21682.6</td>
    
        <td>1294455</td>
    
        <td>19.688363056266898</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>18646.1</td>
    
        <td>315556</td>
    
        <td>21.5933146573033</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>18490.2</td>
    
        <td>840407</td>
    
        <td>23.3226282027637</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>10344.1</td>
    
        <td>417432</td>
    
        <td>20.590659077406603</td>
    
    </tr>

</table>

The above table shows the countries with the highest GDP (Germany, UK, France, Italy). However, we can also sort the countries with the highest population in descending order to see which countries have the biggest population overall (below). Looks like the top 5 countries with the highest GDP also have the highest population numbers.

\`\`\`python

%%sql

SELECT gdp.country, gdp, total_pop, prct_yng_adt_pop

FROM gdp

INNER JOIN population

ON gdp.country = population.country

ORDER BY total_pop DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>gdp</th>
    
        <th>total_pop</th>
    
        <th>prct_yng_adt_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>3159750</td>
    
        <td>80219695</td>
    
        <td>17.105589842993</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>2228568</td>
    
        <td>64933400</td>
    
        <td>18.527500485112398</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>2403382.6</td>
    
        <td>63182180</td>
    
        <td>19.942316013787398</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>1689824</td>
    
        <td>59433744</td>
    
        <td>15.4749732744415</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>1118743</td>
    
        <td>46815910</td>
    
        <td>16.6743314398887</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>426547.5</td>
    
        <td>38044565</td>
    
        <td>21.9158084735625</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>170393.6</td>
    
        <td>20121641</td>
    
        <td>18.775327519261502</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>708337</td>
    
        <td>16655799</td>
    
        <td>18.269979122586697</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>424660.3</td>
    
        <td>11000638</td>
    
        <td>18.4851460433477</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>176487.9</td>
    
        <td>10816286</td>
    
        <td>17.604416155416</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>186480.5</td>
    
        <td>10562178</td>
    
        <td>17.0740447661458</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>176370.1</td>
    
        <td>10436560</td>
    
        <td>18.862489172677602</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>113903.8</td>
    
        <td>9937628</td>
    
        <td>18.3451221961619</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>463147.5</td>
    
        <td>9482855</td>
    
        <td>19.3862924193189</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>356237.6</td>
    
        <td>8401940</td>
    
        <td>18.6775435197109</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>780224.9</td>
    
        <td>7954662</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>605753.7</td>
    
        <td>7954662</td>
    
        <td>18.4457868857281</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>48128.6</td>
    
        <td>7364570</td>
    
        <td>18.4325765115954</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>282089.9</td>
    
        <td>5560628</td>
    
        <td>18.0820044066965</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>81226.1</td>
    
        <td>5397036</td>
    
        <td>21.908599460889302</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>216073</td>
    
        <td>5375276</td>
    
        <td>18.7139599901475</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>335747.5</td>
    
        <td>4979954</td>
    
        <td>19.5865463817537</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>273238.2</td>
    
        <td>4574888</td>
    
        <td>20.6013349397843</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>46639.5</td>
    
        <td>4284889</td>
    
        <td>18.5512623547541</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>38849.4</td>
    
        <td>3043429</td>
    
        <td>20.526780812037998</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>25037.7</td>
    
        <td>2070371</td>
    
        <td>20.442857825964502</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>40357.2</td>
    
        <td>2050189</td>
    
        <td>18.308799822845604</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>21682.6</td>
    
        <td>1294455</td>
    
        <td>19.688363056266898</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>18490.2</td>
    
        <td>840407</td>
    
        <td>23.3226282027637</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>53303</td>
    
        <td>512353</td>
    
        <td>18.9406522456197</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>10344.1</td>
    
        <td>417432</td>
    
        <td>20.590659077406603</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>18646.1</td>
    
        <td>315556</td>
    
        <td>21.5933146573033</td>
    
    </tr>

</table>

\# b) Joining and filtering

Below are a few simple techniques to filter /narrow down the results based on certain criteria. Say, for example, we only want to see those countries where GDP is bigger than EUR200,000 and the percentage of young population is higher than 18%. We can use a WHERE clause (as shown below) to filter the results of the join based on those criteria.

As you'll see, this only shows us 13 out of the original 31 countries.

Again, the results are ordered by percentage of young population in the country (high to low) which we've also rounded up to 2 decimal points using ROUND below.

It's interesting to see that Poland is the country with the highest percentage of young people in the EU, closely followed by Ireland, UK, Norway and Sweden.

\`\`\`python

%%sql

SELECT gdp.country, gdp, total_pop, ROUND(prct_yng_adt_pop,2) AS prct_yng_pop

FROM gdp

INNER JOIN population

ON gdp.country = population.country

WHERE gdp > 200000 AND prct_yng_adt_pop > 18

ORDER BY prct_yng_pop DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    13 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>gdp</th>
    
        <th>total_pop</th>
    
        <th>prct_yng_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>426547.5</td>
    
        <td>38044565</td>
    
        <td>21.92</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>273238.2</td>
    
        <td>4574888</td>
    
        <td>20.60</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>2403382.6</td>
    
        <td>63182180</td>
    
        <td>19.94</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>335747.5</td>
    
        <td>4979954</td>
    
        <td>19.59</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>463147.5</td>
    
        <td>9482855</td>
    
        <td>19.39</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>780224.9</td>
    
        <td>7954662</td>
    
        <td>18.71</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>216073</td>
    
        <td>5375276</td>
    
        <td>18.71</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>356237.6</td>
    
        <td>8401940</td>
    
        <td>18.68</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>2228568</td>
    
        <td>64933400</td>
    
        <td>18.53</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>424660.3</td>
    
        <td>11000638</td>
    
        <td>18.49</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>605753.7</td>
    
        <td>7954662</td>
    
        <td>18.45</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>708337</td>
    
        <td>16655799</td>
    
        <td>18.27</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>282089.9</td>
    
        <td>5560628</td>
    
        <td>18.08</td>
    
    </tr>

</table>

\# c) Case statements

Case statements are great for creating categories from data. For example, say we wanted to categorise the percentage of young people in a country as high/low/medium so that high is over 20%, medium is between 18%-20% and low is under 18%. We simply create a new column in the population table that shows those categories instead of the original numbers. The advantage is that the data is easier to analyse with these categories (rather than looking at random percentages).

We can also use the WHERE calsue again to filter the population table and only display countries with a population over 40m.

This shows us that out of the 5 largest countries in Europe, France and the UK have younger populations than Germany, Italy and Spain.

\`\`\`python

%%sql

SELECT country, total_pop,

CASE WHEN prct_yng_adt_pop > 20 THEN 'high'

WHEN prct_yng_adt_pop BETWEEN 18 AND 20 THEN 'medium'

ELSE 'low' END AS young_pop

FROM population

WHERE total_pop > 40000000

ORDER BY total_pop DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>total_pop</th>
    
        <th>young_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>80219695</td>
    
        <td>low</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>64933400</td>
    
        <td>medium</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>63182180</td>
    
        <td>medium</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>59433744</td>
    
        <td>low</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>46815910</td>
    
        <td>low</td>
    
    </tr>

</table>

\# d) Joining multiple tables

Now that we have a general idea of which countries have the largest population, markets and GDP and which countries have the youngest population, we might want to look at economic indicatots. We can join the relevant tables to extract more insights about the EU countries with the best economic performance according to indicators such as unemployment rate, underemployment rate, median income and the percentage of people with low savings.

Below is an example of joining 4 different table. The unique key we can join on is the country column which is the same across all tables.

We order the results by unemployment rate (ascending) low to high. We can see that Iceland, Chech Republic, germany, Malta and Norway have low unemployment whereas Spain and Greece have very high unemployment.

\`\`\`python

%%sql

SELECT e.country, unemp_rate, med_income_underemp, median_income, prct_low_savings

FROM unemployment AS e

INNER JOIN underemployment AS u

ON e.country = u.country

INNER JOIN median_income AS m

ON e.country = m.country

INNER JOIN low_savings AS l

ON e.country = l.country

ORDER BY unemp_rate, med_income_underemp;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>unemp_rate</th>
    
        <th>med_income_underemp</th>
    
        <th>median_income</th>
    
        <th>prct_low_savings</th>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>3.0</td>
    
        <td>4.3</td>
    
        <td>22193</td>
    
        <td>30.9</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>4.0</td>
    
        <td>6.7</td>
    
        <td>12478</td>
    
        <td>32.1</td>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>4.1</td>
    
        <td>9.6</td>
    
        <td>21152</td>
    
        <td>30.0</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>4.7</td>
    
        <td>7.3</td>
    
        <td>17264</td>
    
        <td>20.8</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>4.7</td>
    
        <td>7.7</td>
    
        <td>27670</td>
    
        <td>18.1</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>4.8</td>
    
        <td>11.3</td>
    
        <td>17296</td>
    
        <td>38.0</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>5.0</td>
    
        <td>5.5</td>
    
        <td>27692</td>
    
        <td>21.0</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>5.1</td>
    
        <td>8.2</td>
    
        <td>8267</td>
    
        <td>50.8</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>5.9</td>
    
        <td>8.2</td>
    
        <td>4724</td>
    
        <td>54.5</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>6.0</td>
    
        <td>8.1</td>
    
        <td>23071</td>
    
        <td>22.6</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>6.0</td>
    
        <td>9.7</td>
    
        <td>21189</td>
    
        <td>22.5</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>6.2</td>
    
        <td>6.4</td>
    
        <td>10865</td>
    
        <td>37.9</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>6.2</td>
    
        <td>10.7</td>
    
        <td>21355</td>
    
        <td>24.5</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>6.3</td>
    
        <td>6.6</td>
    
        <td>28663</td>
    
        <td>21.9</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>6.8</td>
    
        <td>5.8</td>
    
        <td>11867</td>
    
        <td>31.6</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>7.0</td>
    
        <td>8.5</td>
    
        <td>20955</td>
    
        <td>20.7</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>7.6</td>
    
        <td>11.9</td>
    
        <td>6742</td>
    
        <td>54.2</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>7.8</td>
    
        <td>14.6</td>
    
        <td>21335</td>
    
        <td>25.9</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>7.9</td>
    
        <td>10.2</td>
    
        <td>9364</td>
    
        <td>53.2</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>8.0</td>
    
        <td>7.4</td>
    
        <td>15250</td>
    
        <td>41.7</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>8.4</td>
    
        <td>18.2</td>
    
        <td>18286</td>
    
        <td>45.3</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>8.8</td>
    
        <td>11.4</td>
    
        <td>19997</td>
    
        <td>29.4</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>9.6</td>
    
        <td>7.2</td>
    
        <td>9257</td>
    
        <td>60.0</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>9.7</td>
    
        <td>6.5</td>
    
        <td>10466</td>
    
        <td>37.9</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>10.1</td>
    
        <td>8.4</td>
    
        <td>20621</td>
    
        <td>31.8</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>10.9</td>
    
        <td>10.1</td>
    
        <td>6501</td>
    
        <td>34.4</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>11.2</td>
    
        <td>9.1</td>
    
        <td>10805</td>
    
        <td>38.3</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>11.7</td>
    
        <td>12.8</td>
    
        <td>16237</td>
    
        <td>40.4</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>13.0</td>
    
        <td>10.6</td>
    
        <td>16173</td>
    
        <td>56.6</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>13.1</td>
    
        <td>13.0</td>
    
        <td>8985</td>
    
        <td>57.7</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>19.6</td>
    
        <td>14.9</td>
    
        <td>15347</td>
    
        <td>38.7</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>23.6</td>
    
        <td>17.2</td>
    
        <td>9048</td>
    
        <td>53.6</td>
    
    </tr>

</table>

\# e) Simple subqueries

\## Subquery in WHERE clause

Subqueries are a great way to filter results. Say we only wanted to see the top 3 countries with the higher than average median income. We can insert a sub-query into the WHERE as a condition which calculates the average (AVG) median income across the entire median income column for all countries and only displays those countries with a median income higher than that average.

It might be interesting to see the population number next to median income to see how big those markets are. We can do an INNER JOIN to join the population column to the median income table.

We then sort the results by income (high to low) and limit the results to 3.

Interestingly, Norway come up again here in third place - the other countries with the higher than average median income are (not surprisingly) Luxemburg and Switzerland. Also interesting that all three countries have fairly small populations with Luxemburg (at first place for income) having around 0.5m people. Norway is next with around 5m people and Switzerland with nearly 8m.

\`\`\`python

%%sql

SELECT m.country, median_income, total_pop

FROM median_income AS m

INNER JOIN population AS p

ON m.country = p.country

WHERE median_income > (SELECT AVG(median_income)

                       FROM median_income)

ORDER BY median_income DESC

LIMIT 3;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    3 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>median_income</th>
    
        <th>total_pop</th>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>28663</td>
    
        <td>512353</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>27692</td>
    
        <td>7954662</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>27670</td>
    
        <td>4979954</td>
    
    </tr>

</table>

\## Subquery in FROM clause

Let's say we wanted to compare two other economic indicators - which countries have the lowest undereployment and the country's savings rate (the personal saving rate is calculated as percentage of how much people save out of their total disposable income).

Savings rate is an interesting indicator of economic conditions - in the US for example - as of September 2020 - the savings rate was 14.3%. Economic conditions such as economic stability and total income are important in determining savings rates. Periods of high economic uncertainty, such as recessions and economic shocks, tend to induce an increase in the savings rate as people defer current spending to prepare for an uncertain economic future.

In the example below, a subquery creates a sub-set of the low_savings table to only focus on the highest savings rates. We then pull the highest rates column from the subquery into the main results table and join with the underemployment table on the "country" key.

We order the results with the highest savings rate first and limit the results to 5.

Interesting to see that various Eastern European countries and Cyprus have a very high savings rate (60% for Latvia!) and the underemployment rates are fairly high for these 5 countries as well (Croatia the highest with 13% underemployment). So these countries may not rank that highly in our overall quality of life analysis.

\`\`\`python

%%sql

SELECT u.country, med_income_underemp, subquery.max_savings

FROM underemployment as u,

(SELECT low_savings.country, MAX(prct_low_savings) AS max_savings

      FROM low_savings GROUP BY country) AS subquery

WHERE u.country = subquery.country

ORDER BY max_savings DESC

LIMIT 5;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>med_income_underemp</th>
    
        <th>max_savings</th>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>7.2</td>
    
        <td>60.0</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>13.0</td>
    
        <td>57.7</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>10.6</td>
    
        <td>56.6</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>8.2</td>
    
        <td>54.5</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>11.9</td>
    
        <td>54.2</td>
    
    </tr>

</table>

In the 2nd query below, we order the results by underemployment rates (low to high). Interestingly, this brings up 5 other countries, all with fairly low levels of underemployment and much lower savings rates compared to the previous results. Iceland is leading with 4.3% underemployment (and a 30.9% savings rate).

\`\`\`python

%%sql

SELECT u.country, med_income_underemp, subquery.max_savings

FROM underemployment as u,

(SELECT low_savings.country, MAX(prct_low_savings) AS max_savings

      FROM low_savings GROUP BY country) AS subquery

WHERE u.country = subquery.country

ORDER BY med_income_underemp

LIMIT 5;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>med_income_underemp</th>
    
        <th>max_savings</th>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>4.3</td>
    
        <td>30.9</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>5.5</td>
    
        <td>21.0</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>5.8</td>
    
        <td>31.6</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>6.4</td>
    
        <td>37.9</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>6.5</td>
    
        <td>37.9</td>
    
    </tr>

</table>

\# f) More complex joins & subqueries

If we wanted to see other data side by side we can perform a multi-join that links together multiple datasets on the 'country' key as shown below. The results show all EU countries ordered by average hours worked (low to high) with other indicators such as perceived good health, high life satisfaction, life expectancy, job satisfaction and crime rates. This table throws a lot of information at us which is difficult to analyse as is so we'll need to manipulate the data a bit further to gain more insight.

\`\`\`python

%%sql

SELECT w.country, avg_hrs_worked, p.prct_health_verygood, ls.prct_life_satis_high, le.life_expect, js.prct_job_satis_high,es.prct_env_satis_high, c.prct_rpt_crime

FROM work_hours AS w

INNER JOIN perceived_health AS p

ON w.country = p.country

INNER JOIN life_satisfaction AS ls

ON w.country = ls.country

INNER JOIN life_expectancy AS le

ON w.country = le.country

INNER JOIN job_satisfaction AS js

ON w.country = js.country

INNER JOIN env_satisfaction AS es

ON w.country = es.country

INNER JOIN crime AS c

ON w.country = c.country

ORDER BY avg_hrs_worked;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>avg_hrs_worked</th>
    
        <th>prct_health_verygood</th>
    
        <th>prct_life_satis_high</th>
    
        <th>life_expect</th>
    
        <th>prct_job_satis_high</th>
    
        <th>prct_env_satis_high</th>
    
        <th>prct_rpt_crime</th>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>30.3</td>
    
        <td>22.5</td>
    
        <td>26.1</td>
    
        <td>81.7</td>
    
        <td>22.9</td>
    
        <td>31.3</td>
    
        <td>16.9</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>32.9</td>
    
        <td>26.5</td>
    
        <td>42.7</td>
    
        <td>80.9</td>
    
        <td>44.4</td>
    
        <td>52.2</td>
    
        <td>8.4</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>33.8</td>
    
        <td>28.8</td>
    
        <td>35.6</td>
    
        <td>82.5</td>
    
        <td>39.1</td>
    
        <td>48.1</td>
    
        <td>4.6</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>34.7</td>
    
        <td>33.2</td>
    
        <td>38.5</td>
    
        <td>83.7</td>
    
        <td>36.6</td>
    
        <td>41.8</td>
    
        <td>10.9</td>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>35.1</td>
    
        <td>18.0</td>
    
        <td>25.0</td>
    
        <td>81.0</td>
    
        <td>25.0</td>
    
        <td>40.9</td>
    
        <td>14.1</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>35.9</td>
    
        <td>41.7</td>
    
        <td>30.6</td>
    
        <td>81.8</td>
    
        <td>28.3</td>
    
        <td>43.7</td>
    
        <td>9.9</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>36.4</td>
    
        <td>29.0</td>
    
        <td>34.4</td>
    
        <td>82.4</td>
    
        <td>34.5</td>
    
        <td>34.7</td>
    
        <td>12.7</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>36.5</td>
    
        <td>31.9</td>
    
        <td>37.9</td>
    
        <td>81.8</td>
    
        <td>42.2</td>
    
        <td>57.2</td>
    
        <td>12.4</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>36.6</td>
    
        <td>30.8</td>
    
        <td>27.8</td>
    
        <td>81.2</td>
    
        <td>28.0</td>
    
        <td>38.2</td>
    
        <td>16.8</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>36.7</td>
    
        <td>20.2</td>
    
        <td>38.6</td>
    
        <td>81.5</td>
    
        <td>40.8</td>
    
        <td>34.7</td>
    
        <td>6.5</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>37.0</td>
    
        <td>10.5</td>
    
        <td>14.2</td>
    
        <td>83.4</td>
    
        <td>20.2</td>
    
        <td>11.0</td>
    
        <td>14.7</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>37.0</td>
    
        <td>29.8</td>
    
        <td>20.9</td>
    
        <td>81.5</td>
    
        <td>23.0</td>
    
        <td>22.2</td>
    
        <td>13.4</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>37.3</td>
    
        <td>21.9</td>
    
        <td>16.1</td>
    
        <td>82.7</td>
    
        <td>20.0</td>
    
        <td>27.9</td>
    
        <td>14.8</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>37.5</td>
    
        <td>23.4</td>
    
        <td>25.7</td>
    
        <td>82.7</td>
    
        <td>30.4</td>
    
        <td>33.9</td>
    
        <td>12.2</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>37.7</td>
    
        <td>15.6</td>
    
        <td>18.4</td>
    
        <td>83.5</td>
    
        <td>19.4</td>
    
        <td>23.7</td>
    
        <td>10.3</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>38.4</td>
    
        <td>9.3</td>
    
        <td>13.5</td>
    
        <td>78.0</td>
    
        <td>26.6</td>
    
        <td>18.1</td>
    
        <td>9.2</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>38.5</td>
    
        <td>6.6</td>
    
        <td>18.8</td>
    
        <td>74.9</td>
    
        <td>29.6</td>
    
        <td>42.1</td>
    
        <td>3.4</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>38.7</td>
    
        <td>5.1</td>
    
        <td>12.6</td>
    
        <td>74.9</td>
    
        <td>25.8</td>
    
        <td>26.2</td>
    
        <td>10.0</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>38.7</td>
    
        <td>22.3</td>
    
        <td>21.9</td>
    
        <td>82.6</td>
    
        <td>27.2</td>
    
        <td>31.0</td>
    
        <td>10.4</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>39.2</td>
    
        <td>44.1</td>
    
        <td>14.2</td>
    
        <td>82.7</td>
    
        <td>28.2</td>
    
        <td>13.4</td>
    
        <td>9.8</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>39.3</td>
    
        <td>20.3</td>
    
        <td>20.4</td>
    
        <td>81.2</td>
    
        <td>29.1</td>
    
        <td>42.3</td>
    
        <td>8.5</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>39.3</td>
    
        <td>38.3</td>
    
        <td>38.1</td>
    
        <td>82.2</td>
    
        <td>42.3</td>
    
        <td>29.2</td>
    
        <td>2.2</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>39.4</td>
    
        <td>8.7</td>
    
        <td>13.8</td>
    
        <td>81.3</td>
    
        <td>24.5</td>
    
        <td>19.6</td>
    
        <td>7.8</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>39.4</td>
    
        <td>25.5</td>
    
        <td>15.0</td>
    
        <td>78.2</td>
    
        <td>25.6</td>
    
        <td>22.4</td>
    
        <td>3.0</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>39.7</td>
    
        <td>17.6</td>
    
        <td>11.3</td>
    
        <td>76.2</td>
    
        <td>23.0</td>
    
        <td>15.8</td>
    
        <td>9.7</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>39.9</td>
    
        <td>26.6</td>
    
        <td>19.7</td>
    
        <td>75.3</td>
    
        <td>20.4</td>
    
        <td>29.1</td>
    
        <td>14.1</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>40.1</td>
    
        <td>22.4</td>
    
        <td>25.0</td>
    
        <td>77.3</td>
    
        <td>29.2</td>
    
        <td>22.8</td>
    
        <td>6.9</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>40.3</td>
    
        <td>18.6</td>
    
        <td>21.3</td>
    
        <td>79.1</td>
    
        <td>29.6</td>
    
        <td>33.5</td>
    
        <td>11.7</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>40.7</td>
    
        <td>15.9</td>
    
        <td>29.4</td>
    
        <td>78.0</td>
    
        <td>32.0</td>
    
        <td>39.7</td>
    
        <td>5.6</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>40.8</td>
    
        <td>19.1</td>
    
        <td>5.9</td>
    
        <td>74.9</td>
    
        <td>16.1</td>
    
        <td>8.4</td>
    
        <td>25.0</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>42.3</td>
    
        <td>45.0</td>
    
        <td>12.8</td>
    
        <td>81.5</td>
    
        <td>14.0</td>
    
        <td>18.3</td>
    
        <td>11.8</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>46.8</td>
    
        <td>6.5</td>
    
        <td>11.1</td>
    
        <td>78.1</td>
    
        <td>18.3</td>
    
        <td>18.3</td>
    
        <td>10.7</td>
    
    </tr>

</table>

\## Filtering with aggregate functions & subqueries

If we wanted to explore work hours more we can run an aggregate function to find the average number of hours worked in the EU and filter our results based on that number (38 hours).

\`\`\`python

%%sql

SELECT AVG(avg_hrs_worked)FROM work_hours;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    1 rows affected.

<table>

    <tr>
    
        <th>avg</th>
    
    </tr>
    
    <tr>
    
        <td>38.0281250000000000</td>
    
    </tr>

</table>

What if we wanted to see the top 5 countries with a high percentage of reported "very good health" - but only for countries with short working weeks?

Now that we've calculated the average number of weekly hours worked across the EU,  we can create a sub-query to filter the "good health" countries by those countries with short working weeks (ie that work less than the average weekly hours). We use IN in the WHERE clause to filter the "good health"country by this condition.

Note that hours worked are not shown in the results as such, this is only used as a filter.

We order the results by percentage of population who report being in very good health (high to low) and limit the result to 5. Ireland seems to come first on these two indicators with Switzerland in 2nd place (very good health & short working week).

\`\`\`python

%%sql

SELECT country, prct_health_verygood

FROM perceived_health

WHERE country IN (SELECT country

                  FROM work_hours
    
                  WHERE avg_hrs_worked < 38)

ORDER BY prct_health_verygood DESC

LIMIT 5;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>prct_health_verygood</th>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>41.7</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>33.2</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>31.9</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>30.8</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>29.8</td>
    
    </tr>

</table>

If we wanted to look at life satisfaction scores (currently shown as % of people who responded as "high", "medium" and "low" in each EU country) - we could explore how each country's response differs from the average across the EU.

First, calculate the average % across the EU of high life satisfaction scores. Looks like on average 23% of EU citizens give a "high" life satisfaction score.

\`\`\`python

%%sql

SELECT AVG(prct_life_satis_high) FROM life_satisfaction;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    1 rows affected.

<table>

    <tr>
    
        <th>avg</th>
    
    </tr>
    
    <tr>
    
        <td>23.0406250000000000</td>
    
    </tr>

</table>

Next, let's see how each country scores against the average by creating a new column ("diff") that calculates the difference between the country's % of people that said they have "high" life satisfaction and the average across the EU.

Looks like Denmark scores the highest above average (by almost 20%) for this indicator and Bulgaria scores the lowest below average (by 17.1%). This comparative calculation makes it easier to analyse each country's results rather than simply looking at them in isolation.

\`\`\`python

%%sql

SELECT country, prct_life_satis_high,

SUM(l.prct_life_satis_high - 23) AS diff

FROM life_satisfaction AS l

GROUP BY country

ORDER BY diff DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    32 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>prct_life_satis_high</th>
    
        <th>diff</th>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>42.7</td>
    
        <td>19.7</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>38.6</td>
    
        <td>15.6</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>38.5</td>
    
        <td>15.5</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>38.1</td>
    
        <td>15.1</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>37.9</td>
    
        <td>14.9</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>35.6</td>
    
        <td>12.6</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>34.4</td>
    
        <td>11.4</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>30.6</td>
    
        <td>7.6</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>29.4</td>
    
        <td>6.4</td>
    
    </tr>
    
    <tr>
    
        <td>United Kingdom</td>
    
        <td>27.8</td>
    
        <td>4.8</td>
    
    </tr>
    
    <tr>
    
        <td>Netherlands</td>
    
        <td>26.1</td>
    
        <td>3.1</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>25.7</td>
    
        <td>2.7</td>
    
    </tr>
    
    <tr>
    
        <td>Germany</td>
    
        <td>25.0</td>
    
        <td>2.0</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>25.0</td>
    
        <td>2.0</td>
    
    </tr>
    
    <tr>
    
        <td>Malta</td>
    
        <td>21.9</td>
    
        <td>-1.1</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>21.3</td>
    
        <td>-1.7</td>
    
    </tr>
    
    <tr>
    
        <td>Belgium</td>
    
        <td>20.9</td>
    
        <td>-2.1</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>20.4</td>
    
        <td>-2.6</td>
    
    </tr>
    
    <tr>
    
        <td>Romania</td>
    
        <td>19.7</td>
    
        <td>-3.3</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>18.8</td>
    
        <td>-4.2</td>
    
    </tr>
    
    <tr>
    
        <td>Spain</td>
    
        <td>18.4</td>
    
        <td>-4.6</td>
    
    </tr>
    
    <tr>
    
        <td>France</td>
    
        <td>16.1</td>
    
        <td>-6.9</td>
    
    </tr>
    
    <tr>
    
        <td>Croatia</td>
    
        <td>15.0</td>
    
        <td>-8.0</td>
    
    </tr>
    
    <tr>
    
        <td>Italy</td>
    
        <td>14.2</td>
    
        <td>-8.8</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>14.2</td>
    
        <td>-8.8</td>
    
    </tr>
    
    <tr>
    
        <td>Portugal</td>
    
        <td>13.8</td>
    
        <td>-9.2</td>
    
    </tr>
    
    <tr>
    
        <td>Estonia</td>
    
        <td>13.5</td>
    
        <td>-9.5</td>
    
    </tr>
    
    <tr>
    
        <td>Greece</td>
    
        <td>12.8</td>
    
        <td>-10.2</td>
    
    </tr>
    
    <tr>
    
        <td>Latvia</td>
    
        <td>12.6</td>
    
        <td>-10.4</td>
    
    </tr>
    
    <tr>
    
        <td>Hungary</td>
    
        <td>11.3</td>
    
        <td>-11.7</td>
    
    </tr>
    
    <tr>
    
        <td>Turkey</td>
    
        <td>11.1</td>
    
        <td>-11.9</td>
    
    </tr>
    
    <tr>
    
        <td>Bulgaria</td>
    
        <td>5.9</td>
    
        <td>-17.1</td>
    
    </tr>

</table>

\## Correlated subqueries

Let's see if countries with higher than average life satisfaction also have higher than average life expectancy.

First, calculate the average life expectancy across the EU. Looks like that around 80 years.

Next, let's take a look at the countries' "high life satisfaction" score (filtering by those where more than 37% of the population rated life satisfaction as "high", based on the top 5 countries in the table above).

Then let's display the life expectancy for those countries.

Finally, calculate the difference between the life expectancy for each of those countries and the average (80) and place the results in a new column (life_expect_diff).

Looks like life expectancy for the highest "high life satisfaction" countries is also above average with Switzerland leading the charge on life expectancy (3.7 years above the EU average) although Iceland, Austria, Finland and Denmark are also ranked highly on both life satisfaction and life expectancy.

\`\`\`python

%%sql

SELECT AVG(life_expect) FROM life_expectancy;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    1 rows affected.

<table>

    <tr>
    
        <th>avg</th>
    
    </tr>
    
    <tr>
    
        <td>80.2718750000000000</td>
    
    </tr>

</table>

\`\`\`python

%%sql

SELECT country, prct_life_satis_high,

    (SELECT MAX(life_expect) FROM life_expectancy AS le
    
     WHERE le.country = ls.country) AS high_life_expect, 
    
    SUM((SELECT life_expect FROM life_expectancy AS le
    
     WHERE le.country = ls.country) - 80) AS life_expect_diff

FROM life_satisfaction AS ls

WHERE prct_life_satis_high > 37

GROUP BY ls.country

ORDER BY high_life_expect DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    5 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>prct_life_satis_high</th>
    
        <th>high_life_expect</th>
    
        <th>life_expect_diff</th>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>38.5</td>
    
        <td>83.7</td>
    
        <td>3.7</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>38.1</td>
    
        <td>82.2</td>
    
        <td>2.2</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>37.9</td>
    
        <td>81.8</td>
    
        <td>1.8</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>38.6</td>
    
        <td>81.5</td>
    
        <td>1.5</td>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>42.7</td>
    
        <td>80.9</td>
    
        <td>0.9</td>
    
    </tr>

</table>

\## Nested subqueries

Finally, let's see if we can identify countries with both a higher than average job satisfaction score and a short work week.

We know from a previous calculation (above) that the average EU hours worked per week is 38 hours so we'll be filtering our results to only show hours_worked that are less than or equal to the average.

We also need to calculate the average % across the EU population who rank their job satisfaction as "high". Looks like that's 28% on average. So we'll be filtering for countries who score higher than average on job satisfaction as well.

As we can see below, this is achieved through a nested query (within the first SELECT statement) which filters the hours worked column to only show hours under "38".

The results show the above average "high job satisfaction" countries and their corresponding avg weekly hours worked.

As you'll see, when hours worked are over 38 per week, the table notes a "None" value so we can disregard those countries (even though their job satisfation rates are higher than average).

Interesting that Denmark performs best on both indicators: it's got the highest job satisfaction score and the lowest number of hours worked per week. But Norway, Switzerland, Ireland, Sweden, Austria and Finland also have good scores on both indicators.

\`\`\`python

%%sql

SELECT AVG(prct_job_satis_high) FROM job_satisfaction;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    1 rows affected.

<table>

    <tr>
    
        <th>avg</th>
    
    </tr>
    
    <tr>
    
        <td>28.0093750000000000</td>
    
    </tr>

</table>

\`\`\`python

%%sql

SELECT country, prct_job_satis_high,

    (SELECT avg_hrs_worked FROM work_hours AS w
    
     WHERE js.country = w.country 
    
         AND avg_hrs_worked <= 38 ) AS hours_worked

FROM job_satisfaction AS js

WHERE prct_job_satis_high > 28

GROUP BY js.country

ORDER BY hours_worked, prct_job_satis_high DESC;

\`\`\`

     * postgresql://postgres:***@localhost:5432/practicedb
    
    15 rows affected.

<table>

    <tr>
    
        <th>country</th>
    
        <th>prct_job_satis_high</th>
    
        <th>hours_worked</th>
    
    </tr>
    
    <tr>
    
        <td>Denmark</td>
    
        <td>44.4</td>
    
        <td>32.9</td>
    
    </tr>
    
    <tr>
    
        <td>Norway</td>
    
        <td>39.1</td>
    
        <td>33.8</td>
    
    </tr>
    
    <tr>
    
        <td>Switzerland</td>
    
        <td>36.6</td>
    
        <td>34.7</td>
    
    </tr>
    
    <tr>
    
        <td>Ireland</td>
    
        <td>28.3</td>
    
        <td>35.9</td>
    
    </tr>
    
    <tr>
    
        <td>Sweden</td>
    
        <td>34.5</td>
    
        <td>36.4</td>
    
    </tr>
    
    <tr>
    
        <td>Austria</td>
    
        <td>42.2</td>
    
        <td>36.5</td>
    
    </tr>
    
    <tr>
    
        <td>Finland</td>
    
        <td>40.8</td>
    
        <td>36.7</td>
    
    </tr>
    
    <tr>
    
        <td>Luxembourg</td>
    
        <td>30.4</td>
    
        <td>37.5</td>
    
    </tr>
    
    <tr>
    
        <td>Iceland</td>
    
        <td>42.3</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Poland</td>
    
        <td>32.0</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Czechia</td>
    
        <td>29.6</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Lithuania</td>
    
        <td>29.6</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Slovakia</td>
    
        <td>29.2</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Slovenia</td>
    
        <td>29.1</td>
    
        <td>None</td>
    
    </tr>
    
    <tr>
    
        <td>Cyprus</td>
    
        <td>28.2</td>
    
        <td>None</td>
    
    </tr>

</table>

\# 7. Exploratory data analysis in pandas/SQL & visualisation in matplotlib & seaborn

We can now visualise our key findings and see if we can gain further insights.

In order to create visualisation, we need to:

\- import the relevant python libraries (below)

\- create an engine to our database

\- pass the SQL query into pandas' read_sql_query() function to create a dataframe

\- plot the dataframe in matplotlib.

\## a) Bar charts

First, let's visualise the above SQL query which compared EU countries according to 3 key indicators: gdp, population number and percentge of young people. As above, we filter only for those countries where the percentage of young people out of the entire population exceeds 18%. We also only display the top 10 countries by GDP (low to high).

Below is a simple visualisation of only the GDP column in our query. Here's what that looks like in code.

\`\`\`python

from sqlalchemy import create_engine

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

import numpy as np

engine = create_engine('postgresql://postgres:lovesql@localhost:5432/practicedb')

df = pd.read_sql_query("SELECT gdp.country, gdp, total_pop, ROUND(prct_yng_adt_pop,2) AS prct_yng_pop FROM gdp INNER JOIN population ON gdp.country = population.country WHERE gdp > 200000 AND prct_yng_adt_pop > 18 ORDER BY gdp DESC", engine)

df = df.head(10)

fig, ax = plt.subplots(figsize=(20,10))

ax.bar(df\['country'\], df\['gdp'\])

ax.set_xticklabels(df.country, rotation=90)

ax.set_ylabel("GDP ($ millions)")

ax.set_title("GDP of EU countries with highest % of young population")

plt.show()

\`\`\`

!\[png\](/images/output_70_0.png)

\`\`\`python

print(df)

\`\`\`

              country        gdp   total_pop  prct_yng_pop
    
    0  United Kingdom  2403382.6  63182180.0         19.94
    
    1          France  2228568.0  64933400.0         18.53
    
    2          Turkey   780224.9   7954662.0         18.71
    
    3     Netherlands   708337.0  16655799.0         18.27
    
    4     Switzerland   605753.7   7954662.0         18.45
    
    5          Sweden   463147.5   9482855.0         19.39
    
    6          Poland   426547.5  38044565.0         21.92
    
    7         Belgium   424660.3  11000638.0         18.49
    
    8         Austria   356237.6   8401940.0         18.68
    
    9          Norway   335747.5   4979954.0         19.59

\## b) Subplots with shared x axis and multiple indicators

As you can see above, our query also has population numbers and percetage of young people so it would be interesting to see all three columns visualised side by side. Because the measurement units are different for each column, it would be better to plot this in three separate figures. Matplotlib's plt.subplots() is a great tool for plotting multiple figures which share an axis side by side.

It seems the UK and France have a much higher GDP and population number than other EU countries with similar percentage of young people.

\`\`\`python

\#set subplots as 3 rows and 1 col, set fig size and share x axis

fig, ax = plt.subplots(3,1, figsize=(20,10), sharex=True)

\#add data to the figure in the first row and set color

ax\[0\].bar(df\["country"\],df\["gdp"\], color='b')

\#set x axis label and rotation

ax\[0\].set_xticklabels(df.country, rotation=90)

\#set y label

ax\[0\].set_ylabel("GDP ($ millions)")

\#add data to fig in the second row

ax\[1\].bar(df\["country"\], df\["total_pop"\], color='r')

\#set y label for fig in second row

ax\[1\].set_ylabel("Population (10's of millions)")

\##add data to fig in the third row

ax\[2\].bar(df\["country"\], df\["prct_yng_pop"\], color='g')

\#set y label for fig in third row

ax\[2\].set_ylabel("% of young population")

\#display plot

plt.show()

\`\`\`

!\[png\](output_73_0.png)

\## c) Scatter plots & Stacked bar charts

Say we wanted to dive deeper into the economic data and compare unemployment rates and underemployment rates in EU countries with above average median income. We can run an SQL query again and convert that into a dataframe to plot a stacked bar chart which will combine the employment and underemployment rate for each of the relevant countries. The results will be ordered by employment rate (low to high).

\`\`\`python

\#create another dataframe named 'emp'

emp = pd.read_sql_query("SELECT e.country, unemp_rate, med_income_underemp, median_income FROM unemployment AS e INNER JOIN underemployment AS u ON e.country = u.country INNER JOIN median_income AS m ON e.country = m.country WHERE median_income > (SELECT AVG(median_income) FROM median_income) ORDER BY unemp_rate", engine)

print(emp)

\`\`\`

               country  unemp_rate  med_income_underemp  median_income
    
    0          Iceland         3.0                  4.3        22193.0
    
    1          Germany         4.1                  9.6        21152.0
    
    2           Norway         4.7                  7.7        27670.0
    
    3            Malta         4.7                  7.3        17264.0
    
    4   United Kingdom         4.8                 11.3        17296.0
    
    5      Switzerland         5.0                  5.5        27692.0
    
    6          Austria         6.0                  8.1        23071.0
    
    7      Netherlands         6.0                  9.7        21189.0
    
    8          Denmark         6.2                 10.7        21355.0
    
    9       Luxembourg         6.3                  6.6        28663.0
    
    10          Sweden         7.0                  8.5        20955.0
    
    11         Belgium         7.8                 14.6        21335.0
    
    12         Ireland         8.4                 18.2        18286.0
    
    13         Finland         8.8                 11.4        19997.0
    
    14          France        10.1                  8.4        20621.0
    
    15           Italy        11.7                 12.8        16237.0
    
    16          Cyprus        13.0                 10.6        16173.0

We can see that some of the countries in the query results have pretty high employment rates and so we'll redefine the dataframe to only include the top 10 countries with the lowest unemployment rates.

\`\`\`python

emp = emp.head(10)

emp

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>unemp_rate</th>
    
      <th>med_income_underemp</th>
    
      <th>median_income</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Iceland</td>
    
      <td>3.0</td>
    
      <td>4.3</td>
    
      <td>22193.0</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Germany</td>
    
      <td>4.1</td>
    
      <td>9.6</td>
    
      <td>21152.0</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Norway</td>
    
      <td>4.7</td>
    
      <td>7.7</td>
    
      <td>27670.0</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Malta</td>
    
      <td>4.7</td>
    
      <td>7.3</td>
    
      <td>17264.0</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>United Kingdom</td>
    
      <td>4.8</td>
    
      <td>11.3</td>
    
      <td>17296.0</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Switzerland</td>
    
      <td>5.0</td>
    
      <td>5.5</td>
    
      <td>27692.0</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Austria</td>
    
      <td>6.0</td>
    
      <td>8.1</td>
    
      <td>23071.0</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Netherlands</td>
    
      <td>6.0</td>
    
      <td>9.7</td>
    
      <td>21189.0</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Denmark</td>
    
      <td>6.2</td>
    
      <td>10.7</td>
    
      <td>21355.0</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>Luxembourg</td>
    
      <td>6.3</td>
    
      <td>6.6</td>
    
      <td>28663.0</td>
    
    </tr>

</tbody>

</table>

</div>

\`\`\`python

\#run basic statistical calculations on the median_income col

emp\['median_income'\].describe()

\`\`\`

    count       10.000000
    
    mean     22754.500000
    
    std       4093.152582
    
    min      17264.000000
    
    25%      21161.250000
    
    50%      21774.000000
    
    75%      26520.250000
    
    max      28663.000000
    
    Name: median_income, dtype: float64

\`\`\`python

\#let's see if there's a correlation between median income and unemployment rates

\#import the query into a dataframe again (this time without filtering for higher than average median income)

emp = pd.read_sql_query("SELECT e.country, unemp_rate, med_income_underemp, median_income FROM unemployment AS e INNER JOIN underemployment AS u ON e.country = u.country INNER JOIN median_income AS m ON e.country = m.country ORDER BY unemp_rate", engine)

fig, ax = plt.subplots(figsize=(20,10))

ax.scatter(emp\["unemp_rate"\], emp\["median_income"\])

ax.set_xlabel("Unemployment rate")

ax.set_ylabel("Median income ($)")

plt.show()

\`\`\`

!\[png\](output_79_0.png)

It looks like there is a negative correlation between unemployment and median income - median income seems to decrease as unemployment rates rise, but this trend is more obvious once unemployment rate is lower than 5% or higher than 10%. Let's see if the same relationship exists between median income and underemployment rates. We can plot both indicators (unemployment and underemployment) on the same figure to compare.

\`\`\`python

emp = pd.read_sql_query("SELECT e.country, unemp_rate, med_income_underemp, median_income FROM unemployment AS e INNER JOIN underemployment AS u ON e.country = u.country INNER JOIN median_income AS m ON e.country = m.country ORDER BY unemp_rate", engine)

fig, ax = plt.subplots(figsize=(20,10))

\#plot income to unemployment correlation

ax.scatter(emp\["unemp_rate"\], emp\["median_income"\], color="blue", label='unemployment')

\#plot income to underemployment correlation

ax.scatter(emp\["med_income_underemp"\],emp\["median_income"\], color="red", label='underemployment')

\#set axis labels

ax.set_xlabel("Unemployment & underemployment rate %")

ax.set_ylabel("Median income ($)")

\#set legend

ax.legend()

\#display plot

plt.show()

\`\`\`

!\[png\](output_81_0.png)

There does seem to be a negative correlation between median income and both employment and underemployment rates, although there are a few outliers. This would indicate that we should focus on countries with low percentages for both indicators to maximise our chances of higher median income. Let's plot the 2 employment indicators by country to see which country is more likely to have higher incomes.

\`\`\`python

\#create a data frame

emp = pd.read_sql_query("SELECT e.country, unemp_rate, med_income_underemp, median_income FROM unemployment AS e INNER JOIN underemployment AS u ON e.country = u.country INNER JOIN median_income AS m ON e.country = m.country WHERE median_income > (SELECT AVG(median_income) FROM median_income) ORDER BY unemp_rate", engine)

\#filter DF to top 10 countries by unemp_rate

emp = emp.head(10)

\#plot stacked bar chart

fig, ax = plt.subplots(figsize=(20,10))

ax.bar(emp.country, emp\["unemp_rate"\], label = "Unemployment")

ax.bar(emp.country, emp\["med_income_underemp"\],bottom=emp\["unemp_rate"\],label="Underemployment")

ax.set_xticklabels(emp.country, rotation=90)

ax.set_ylabel("% unemployment & underemployment")

ax.set_title("Unemployment & underemployment rates in EU countries with higher than average median income")

ax.legend()

plt.show()

\`\`\`

!\[png\](output_83_0.png)

Insights:

\- Iceland leads in terms of both low unemployment and underemployment rates amongst counrties with above average median income.

\- While Germany, Norway, Malta, the UK and Switzerland have similar unemployment rates (4-5%), the UK is much worse placed once the underemployment rate is taken into account.

\- Surprising that an affluent countries like Denmark and Luxemburg have the highest unemployment rates (and Denmark with a fairly high underemployment rate) amongst this group of countries.

\## d) Subplots with categorical variables

We can now explore the life satisfaction scores given by each country to see which country's residents have ranked the highest for "high life satisfaction" and which country scored the lowest.

\`\`\`python

\#query data and convert to dataframe

life_satis = pd.read_sql_query("SELECT * FROM life_satisfaction", engine)

\#sort countries by highest % of population who ranked life satisfaction as 'high'

life_satis.sort_values(by=\['prct_life_satis_high'\], ascending=False, inplace=True, ignore_index=True)

\#select top 10 countries and name as a new variable: "data"

data=life_satis.head(10)

\#display data DF

data

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>prct_life_satis_high</th>
    
      <th>prct_life_satis_med</th>
    
      <th>prct_life_satis_low</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Denmark</td>
    
      <td>42.7</td>
    
      <td>46.6</td>
    
      <td>10.6</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Finland</td>
    
      <td>38.6</td>
    
      <td>55.5</td>
    
      <td>6.0</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Switzerland</td>
    
      <td>38.5</td>
    
      <td>53.5</td>
    
      <td>8.0</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Iceland</td>
    
      <td>38.1</td>
    
      <td>52.4</td>
    
      <td>9.5</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Austria</td>
    
      <td>37.9</td>
    
      <td>49.3</td>
    
      <td>12.9</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Norway</td>
    
      <td>35.6</td>
    
      <td>54.1</td>
    
      <td>10.3</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Sweden</td>
    
      <td>34.4</td>
    
      <td>56.6</td>
    
      <td>9.0</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Ireland</td>
    
      <td>30.6</td>
    
      <td>52.7</td>
    
      <td>16.7</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Poland</td>
    
      <td>29.4</td>
    
      <td>50.7</td>
    
      <td>19.9</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>United Kingdom</td>
    
      <td>27.8</td>
    
      <td>53.2</td>
    
      <td>19.1</td>
    
    </tr>

</tbody>

</table>

</div>

\`\`\`python

fig,ax=plt.subplots(3,1, figsize=(20,15))

ax\[0\].bar(data\['country'\], data\['prct_life_satis_high'\], color = "c")

ax\[1\].bar(data\['country'\], data\['prct_life_satis_med'\], color="m")

ax\[2\].bar(data\['country'\], data\['prct_life_satis_low'\], color="r")

ax\[0\].set_xticklabels(data.country)

ax\[1\].set_xticklabels(data.country)

ax\[2\].set_xticklabels(data.country)

ax\[0\].set_ylabel("% population with HIGH life satisfaction")

ax\[1\].set_ylabel("% population with MED life satisfaction")

ax\[2\].set_ylabel("% population with LOW life satisfaction")

plt.show()

\`\`\`

!\[png\](output_86_0.png)

Insights:

\- Out of the top 10 countries with high life satisfaction in the EU, Denmark comes highest clsoely followed by Finland and Switzerland.

\- Nordic countries seem to do very well on this indicator - Denmark, Finland, Iceland, Norway and Sweden have the hihest scores for both "high" and "med" life satisfaction.

\- In Poland and the UK almost 20% of the population rate life satisfaction as low - contrast that with Finland, where less than 7% of the population rate their life satisfaction as 'low'.

\## e) Linear regression - Regplots & lmplots

Finally, let's check if there's a possible correlation between high job satisfaction, weekly hours worked and life expectancy in the EU, and which countries rank highest for job satisfaction. We'll start by querying our database for the relevant indivators, joining tables and viewing the dataframe once imported into pandas.

\`\`\`python

\#query data and convert into a dataframe

work = pd.read_sql_query("SELECT j.country, prct_job_satis_high, avg_hrs_worked, life_expect FROM job_satisfaction AS j INNER JOIN work_hours AS w ON j.country=w.country INNER JOIN life_expectancy as l ON j.country=l.country", engine)

\#display joined data

work

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>prct_job_satis_high</th>
    
      <th>avg_hrs_worked</th>
    
      <th>life_expect</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Austria</td>
    
      <td>42.2</td>
    
      <td>36.5</td>
    
      <td>81.8</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Belgium</td>
    
      <td>23.0</td>
    
      <td>37.0</td>
    
      <td>81.5</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Bulgaria</td>
    
      <td>16.1</td>
    
      <td>40.8</td>
    
      <td>74.9</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Switzerland</td>
    
      <td>36.6</td>
    
      <td>34.7</td>
    
      <td>83.7</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Cyprus</td>
    
      <td>28.2</td>
    
      <td>39.2</td>
    
      <td>82.7</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Czechia</td>
    
      <td>29.6</td>
    
      <td>40.3</td>
    
      <td>79.1</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Germany</td>
    
      <td>25.0</td>
    
      <td>35.1</td>
    
      <td>81.0</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Denmark</td>
    
      <td>44.4</td>
    
      <td>32.9</td>
    
      <td>80.9</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Estonia</td>
    
      <td>26.6</td>
    
      <td>38.4</td>
    
      <td>78.0</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>Greece</td>
    
      <td>14.0</td>
    
      <td>42.3</td>
    
      <td>81.5</td>
    
    </tr>
    
    <tr>
    
      <th>10</th>
    
      <td>Spain</td>
    
      <td>19.4</td>
    
      <td>37.7</td>
    
      <td>83.5</td>
    
    </tr>
    
    <tr>
    
      <th>11</th>
    
      <td>Finland</td>
    
      <td>40.8</td>
    
      <td>36.7</td>
    
      <td>81.5</td>
    
    </tr>
    
    <tr>
    
      <th>12</th>
    
      <td>France</td>
    
      <td>20.0</td>
    
      <td>37.3</td>
    
      <td>82.7</td>
    
    </tr>
    
    <tr>
    
      <th>13</th>
    
      <td>Croatia</td>
    
      <td>25.6</td>
    
      <td>39.4</td>
    
      <td>78.2</td>
    
    </tr>
    
    <tr>
    
      <th>14</th>
    
      <td>Hungary</td>
    
      <td>23.0</td>
    
      <td>39.7</td>
    
      <td>76.2</td>
    
    </tr>
    
    <tr>
    
      <th>15</th>
    
      <td>Ireland</td>
    
      <td>28.3</td>
    
      <td>35.9</td>
    
      <td>81.8</td>
    
    </tr>
    
    <tr>
    
      <th>16</th>
    
      <td>Iceland</td>
    
      <td>42.3</td>
    
      <td>39.3</td>
    
      <td>82.2</td>
    
    </tr>
    
    <tr>
    
      <th>17</th>
    
      <td>Italy</td>
    
      <td>20.2</td>
    
      <td>37.0</td>
    
      <td>83.4</td>
    
    </tr>
    
    <tr>
    
      <th>18</th>
    
      <td>Lithuania</td>
    
      <td>29.6</td>
    
      <td>38.5</td>
    
      <td>74.9</td>
    
    </tr>
    
    <tr>
    
      <th>19</th>
    
      <td>Luxembourg</td>
    
      <td>30.4</td>
    
      <td>37.5</td>
    
      <td>82.7</td>
    
    </tr>
    
    <tr>
    
      <th>20</th>
    
      <td>Latvia</td>
    
      <td>25.8</td>
    
      <td>38.7</td>
    
      <td>74.9</td>
    
    </tr>
    
    <tr>
    
      <th>21</th>
    
      <td>Malta</td>
    
      <td>27.2</td>
    
      <td>38.7</td>
    
      <td>82.6</td>
    
    </tr>
    
    <tr>
    
      <th>22</th>
    
      <td>Netherlands</td>
    
      <td>22.9</td>
    
      <td>30.3</td>
    
      <td>81.7</td>
    
    </tr>
    
    <tr>
    
      <th>23</th>
    
      <td>Norway</td>
    
      <td>39.1</td>
    
      <td>33.8</td>
    
      <td>82.5</td>
    
    </tr>
    
    <tr>
    
      <th>24</th>
    
      <td>Poland</td>
    
      <td>32.0</td>
    
      <td>40.7</td>
    
      <td>78.0</td>
    
    </tr>
    
    <tr>
    
      <th>25</th>
    
      <td>Portugal</td>
    
      <td>24.5</td>
    
      <td>39.4</td>
    
      <td>81.3</td>
    
    </tr>
    
    <tr>
    
      <th>26</th>
    
      <td>Romania</td>
    
      <td>20.4</td>
    
      <td>39.9</td>
    
      <td>75.3</td>
    
    </tr>
    
    <tr>
    
      <th>27</th>
    
      <td>Sweden</td>
    
      <td>34.5</td>
    
      <td>36.4</td>
    
      <td>82.4</td>
    
    </tr>
    
    <tr>
    
      <th>28</th>
    
      <td>Slovenia</td>
    
      <td>29.1</td>
    
      <td>39.3</td>
    
      <td>81.2</td>
    
    </tr>
    
    <tr>
    
      <th>29</th>
    
      <td>Slovakia</td>
    
      <td>29.2</td>
    
      <td>40.1</td>
    
      <td>77.3</td>
    
    </tr>
    
    <tr>
    
      <th>30</th>
    
      <td>Turkey</td>
    
      <td>18.3</td>
    
      <td>46.8</td>
    
      <td>78.1</td>
    
    </tr>
    
    <tr>
    
      <th>31</th>
    
      <td>United Kingdom</td>
    
      <td>28.0</td>
    
      <td>36.6</td>
    
      <td>81.2</td>
    
    </tr>

</tbody>

</table>

</div>

\## Regplot

It can be very helpful to use statistical models to estimate a simple relationship between two noisy sets of observations. Looking at the above dataframe, it's difficult to draw any meaningful insights without visualisation. The common framework of linear regression can be used to visualise data in seaborn and see if there are any interesting linear relationships in our data.

Two main functions in seaborn are used to visualize a linear relationship as determined through regression. These functions - regplot() and lmplot() - are closely related, and share much of their core functionality.

Simply put, both functions draw a scatterplot of two variables, x and y, and then fit the regression model y \~ x and plot the resulting regression line and a 95% confidence interval for that regression.

The main difference between the 2 functions is that regplot() accepts the x and y variables in a variety of formats including simple numpy arrays, pandas Series objects, or as references to variables in a pandas DataFrame object passed to data. In contrast, lmplot() has data as a required parameter and the x and y variables must be specified as strings. This data format is called “long-form” or “tidy” data.

Other than this input flexibility, regplot() has a subset of lmplot()’s features.

\`\`\`python

plt.figure(figsize=(20,10))

sns.regplot(data=work, x="avg_hrs_worked", y="life_expect")

plt.show()

\`\`\`

!\[png\](output_90_0.png)

\`\`\`python

plt.figure(figsize=(20,10))

sns.regplot(data=work, x="avg_hrs_worked", y="prct_job_satis_high", color='red')

plt.show()

\`\`\`

!\[png\](output_91_0.png)

The plots above show how we can explore the relationship between a pair of variables using regplot(). there does seem to be a negative correlation between average weekly working hours and life expectancy as well as job satisfaction. The central tendency in both situations seems to be that longer hours reduce life expectancy as well as job satisfaction in Europe.

But it would be even more interesting to explore if life expectancy's relationship to hours worked, for example, changes as a function of a third variable, such as the weather. This is when lmplot is extremely handy.

While regplot() only shows a single relationship, lmplot() combines regplot() with FacetGrid to provide an easy interface to show a linear regression on “faceted” plots - this allow you to explore interactions with up to three additional categorical variables.

The best way to separate out a relationship is to plot both levels on the same axes and to use color to distinguish them. Let's start by querying the database for the three variables first.

\`\`\`python

\#query the database and create a DF

df_w = pd.read_sql_query("SELECT w.country, w.avg_hrs_worked, l.life_expect, avg_temp FROM work_hours AS w INNER JOIN life_expectancy as l ON w.country=l.country INNER JOIN weather ON w.country=weather.country", engine)

\#display DF

print(df_w)

\`\`\`

               country  avg_hrs_worked  life_expect  avg_temp
    
    0          Austria            36.5         81.8      44.6
    
    1          Belgium            37.0         81.5      48.8
    
    2         Bulgaria            40.8         74.9      49.4
    
    3          Croatia            39.4         78.2      55.1
    
    4           Cyprus            39.2         82.7      66.1
    
    5          Czechia            40.3         79.1      44.5
    
    6          Denmark            32.9         80.9      46.0
    
    7          Estonia            38.4         78.0      41.9
    
    8          Finland            36.7         81.5      36.6
    
    9           France            37.3         82.7      52.3
    
    10         Germany            35.1         81.0      46.6
    
    11          Greece            42.3         81.5      62.5
    
    12         Hungary            39.7         76.2      50.3
    
    13         Iceland            39.3         82.2      38.5
    
    14         Ireland            35.9         81.8      49.4
    
    15           Italy            37.0         83.4      56.3
    
    16          Latvia            38.7         74.9      42.9
    
    17       Lithuania            38.5         74.9      43.4
    
    18      Luxembourg            37.5         82.7      46.7
    
    19           Malta            38.7         82.6      65.7
    
    20     Netherlands            30.3         81.7      49.2
    
    21          Norway            33.8         82.5      39.8
    
    22          Poland            40.7         78.0      44.6
    
    23        Portugal            39.4         81.3      60.4
    
    24         Romania            39.9         75.3      47.6
    
    25        Slovakia            40.1         77.3      43.2
    
    26        Slovenia            39.3         81.2      46.4
    
    27           Spain            37.7         83.5      60.0
    
    28          Sweden            36.4         82.4      40.3
    
    29     Switzerland            34.7         83.7      42.5
    
    30          Turkey            46.8         78.1      54.2
    
    31  United Kingdom            36.6         81.2      48.8

Looks like the avg_temp column needs to be converted to celsius so we'll use a simple function to do so before we plot.

\`\`\`python

\#define function

def fahr_to_celsius(temp_fahr):

    """Convert Fahrenheit to Celsius
    
    
    
    Return Celsius conversion of input"""
    
    temp_celsius = (temp_fahr - 32) * 5 / 9
    
    return temp_celsius

\#create new column and apply the function to existing column

df_w\["avg_temp_c"\] = fahr_to_celsius(df_w\["avg_temp"\])

\#display DF

df_w

\#drop old col

df_w.drop(\['avg_temp'\], axis=1, inplace=True)

df_w

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>avg_hrs_worked</th>
    
      <th>life_expect</th>
    
      <th>avg_temp_c</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Austria</td>
    
      <td>36.5</td>
    
      <td>81.8</td>
    
      <td>7.000000</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Belgium</td>
    
      <td>37.0</td>
    
      <td>81.5</td>
    
      <td>9.333333</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Bulgaria</td>
    
      <td>40.8</td>
    
      <td>74.9</td>
    
      <td>9.666667</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Croatia</td>
    
      <td>39.4</td>
    
      <td>78.2</td>
    
      <td>12.833333</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Cyprus</td>
    
      <td>39.2</td>
    
      <td>82.7</td>
    
      <td>18.944444</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Czechia</td>
    
      <td>40.3</td>
    
      <td>79.1</td>
    
      <td>6.944444</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Denmark</td>
    
      <td>32.9</td>
    
      <td>80.9</td>
    
      <td>7.777778</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Estonia</td>
    
      <td>38.4</td>
    
      <td>78.0</td>
    
      <td>5.500000</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Finland</td>
    
      <td>36.7</td>
    
      <td>81.5</td>
    
      <td>2.555556</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>France</td>
    
      <td>37.3</td>
    
      <td>82.7</td>
    
      <td>11.277778</td>
    
    </tr>
    
    <tr>
    
      <th>10</th>
    
      <td>Germany</td>
    
      <td>35.1</td>
    
      <td>81.0</td>
    
      <td>8.111111</td>
    
    </tr>
    
    <tr>
    
      <th>11</th>
    
      <td>Greece</td>
    
      <td>42.3</td>
    
      <td>81.5</td>
    
      <td>16.944444</td>
    
    </tr>
    
    <tr>
    
      <th>12</th>
    
      <td>Hungary</td>
    
      <td>39.7</td>
    
      <td>76.2</td>
    
      <td>10.166667</td>
    
    </tr>
    
    <tr>
    
      <th>13</th>
    
      <td>Iceland</td>
    
      <td>39.3</td>
    
      <td>82.2</td>
    
      <td>3.611111</td>
    
    </tr>
    
    <tr>
    
      <th>14</th>
    
      <td>Ireland</td>
    
      <td>35.9</td>
    
      <td>81.8</td>
    
      <td>9.666667</td>
    
    </tr>
    
    <tr>
    
      <th>15</th>
    
      <td>Italy</td>
    
      <td>37.0</td>
    
      <td>83.4</td>
    
      <td>13.500000</td>
    
    </tr>
    
    <tr>
    
      <th>16</th>
    
      <td>Latvia</td>
    
      <td>38.7</td>
    
      <td>74.9</td>
    
      <td>6.055556</td>
    
    </tr>
    
    <tr>
    
      <th>17</th>
    
      <td>Lithuania</td>
    
      <td>38.5</td>
    
      <td>74.9</td>
    
      <td>6.333333</td>
    
    </tr>
    
    <tr>
    
      <th>18</th>
    
      <td>Luxembourg</td>
    
      <td>37.5</td>
    
      <td>82.7</td>
    
      <td>8.166667</td>
    
    </tr>
    
    <tr>
    
      <th>19</th>
    
      <td>Malta</td>
    
      <td>38.7</td>
    
      <td>82.6</td>
    
      <td>18.722222</td>
    
    </tr>
    
    <tr>
    
      <th>20</th>
    
      <td>Netherlands</td>
    
      <td>30.3</td>
    
      <td>81.7</td>
    
      <td>9.555556</td>
    
    </tr>
    
    <tr>
    
      <th>21</th>
    
      <td>Norway</td>
    
      <td>33.8</td>
    
      <td>82.5</td>
    
      <td>4.333333</td>
    
    </tr>
    
    <tr>
    
      <th>22</th>
    
      <td>Poland</td>
    
      <td>40.7</td>
    
      <td>78.0</td>
    
      <td>7.000000</td>
    
    </tr>
    
    <tr>
    
      <th>23</th>
    
      <td>Portugal</td>
    
      <td>39.4</td>
    
      <td>81.3</td>
    
      <td>15.777778</td>
    
    </tr>
    
    <tr>
    
      <th>24</th>
    
      <td>Romania</td>
    
      <td>39.9</td>
    
      <td>75.3</td>
    
      <td>8.666667</td>
    
    </tr>
    
    <tr>
    
      <th>25</th>
    
      <td>Slovakia</td>
    
      <td>40.1</td>
    
      <td>77.3</td>
    
      <td>6.222222</td>
    
    </tr>
    
    <tr>
    
      <th>26</th>
    
      <td>Slovenia</td>
    
      <td>39.3</td>
    
      <td>81.2</td>
    
      <td>8.000000</td>
    
    </tr>
    
    <tr>
    
      <th>27</th>
    
      <td>Spain</td>
    
      <td>37.7</td>
    
      <td>83.5</td>
    
      <td>15.555556</td>
    
    </tr>
    
    <tr>
    
      <th>28</th>
    
      <td>Sweden</td>
    
      <td>36.4</td>
    
      <td>82.4</td>
    
      <td>4.611111</td>
    
    </tr>
    
    <tr>
    
      <th>29</th>
    
      <td>Switzerland</td>
    
      <td>34.7</td>
    
      <td>83.7</td>
    
      <td>5.833333</td>
    
    </tr>
    
    <tr>
    
      <th>30</th>
    
      <td>Turkey</td>
    
      <td>46.8</td>
    
      <td>78.1</td>
    
      <td>12.333333</td>
    
    </tr>
    
    <tr>
    
      <th>31</th>
    
      <td>United Kingdom</td>
    
      <td>36.6</td>
    
      <td>81.2</td>
    
      <td>9.333333</td>
    
    </tr>

</tbody>

</table>

</div>

\`\`\`python

\#apply lambda function to create new column for weather type

df_w\['weather'\] = df_w\['avg_temp_c'\].apply(lambda x: 'cold' if x <= 10 else 'warm')

df_w

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>avg_hrs_worked</th>
    
      <th>life_expect</th>
    
      <th>avg_temp_c</th>
    
      <th>weather</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Austria</td>
    
      <td>36.5</td>
    
      <td>81.8</td>
    
      <td>7.000000</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Belgium</td>
    
      <td>37.0</td>
    
      <td>81.5</td>
    
      <td>9.333333</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Bulgaria</td>
    
      <td>40.8</td>
    
      <td>74.9</td>
    
      <td>9.666667</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Croatia</td>
    
      <td>39.4</td>
    
      <td>78.2</td>
    
      <td>12.833333</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Cyprus</td>
    
      <td>39.2</td>
    
      <td>82.7</td>
    
      <td>18.944444</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Czechia</td>
    
      <td>40.3</td>
    
      <td>79.1</td>
    
      <td>6.944444</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Denmark</td>
    
      <td>32.9</td>
    
      <td>80.9</td>
    
      <td>7.777778</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Estonia</td>
    
      <td>38.4</td>
    
      <td>78.0</td>
    
      <td>5.500000</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Finland</td>
    
      <td>36.7</td>
    
      <td>81.5</td>
    
      <td>2.555556</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>France</td>
    
      <td>37.3</td>
    
      <td>82.7</td>
    
      <td>11.277778</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>10</th>
    
      <td>Germany</td>
    
      <td>35.1</td>
    
      <td>81.0</td>
    
      <td>8.111111</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>11</th>
    
      <td>Greece</td>
    
      <td>42.3</td>
    
      <td>81.5</td>
    
      <td>16.944444</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>12</th>
    
      <td>Hungary</td>
    
      <td>39.7</td>
    
      <td>76.2</td>
    
      <td>10.166667</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>13</th>
    
      <td>Iceland</td>
    
      <td>39.3</td>
    
      <td>82.2</td>
    
      <td>3.611111</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>14</th>
    
      <td>Ireland</td>
    
      <td>35.9</td>
    
      <td>81.8</td>
    
      <td>9.666667</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>15</th>
    
      <td>Italy</td>
    
      <td>37.0</td>
    
      <td>83.4</td>
    
      <td>13.500000</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>16</th>
    
      <td>Latvia</td>
    
      <td>38.7</td>
    
      <td>74.9</td>
    
      <td>6.055556</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>17</th>
    
      <td>Lithuania</td>
    
      <td>38.5</td>
    
      <td>74.9</td>
    
      <td>6.333333</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>18</th>
    
      <td>Luxembourg</td>
    
      <td>37.5</td>
    
      <td>82.7</td>
    
      <td>8.166667</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>19</th>
    
      <td>Malta</td>
    
      <td>38.7</td>
    
      <td>82.6</td>
    
      <td>18.722222</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>20</th>
    
      <td>Netherlands</td>
    
      <td>30.3</td>
    
      <td>81.7</td>
    
      <td>9.555556</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>21</th>
    
      <td>Norway</td>
    
      <td>33.8</td>
    
      <td>82.5</td>
    
      <td>4.333333</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>22</th>
    
      <td>Poland</td>
    
      <td>40.7</td>
    
      <td>78.0</td>
    
      <td>7.000000</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>23</th>
    
      <td>Portugal</td>
    
      <td>39.4</td>
    
      <td>81.3</td>
    
      <td>15.777778</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>24</th>
    
      <td>Romania</td>
    
      <td>39.9</td>
    
      <td>75.3</td>
    
      <td>8.666667</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>25</th>
    
      <td>Slovakia</td>
    
      <td>40.1</td>
    
      <td>77.3</td>
    
      <td>6.222222</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>26</th>
    
      <td>Slovenia</td>
    
      <td>39.3</td>
    
      <td>81.2</td>
    
      <td>8.000000</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>27</th>
    
      <td>Spain</td>
    
      <td>37.7</td>
    
      <td>83.5</td>
    
      <td>15.555556</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>28</th>
    
      <td>Sweden</td>
    
      <td>36.4</td>
    
      <td>82.4</td>
    
      <td>4.611111</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>29</th>
    
      <td>Switzerland</td>
    
      <td>34.7</td>
    
      <td>83.7</td>
    
      <td>5.833333</td>
    
      <td>cold</td>
    
    </tr>
    
    <tr>
    
      <th>30</th>
    
      <td>Turkey</td>
    
      <td>46.8</td>
    
      <td>78.1</td>
    
      <td>12.333333</td>
    
      <td>warm</td>
    
    </tr>
    
    <tr>
    
      <th>31</th>
    
      <td>United Kingdom</td>
    
      <td>36.6</td>
    
      <td>81.2</td>
    
      <td>9.333333</td>
    
      <td>cold</td>
    
    </tr>

</tbody>

</table>

</div>

\## lmplot

Let's see if the relationship between life expectancy and hours worked changes as a function of the weather in a country.

\`\`\`python

\#set fig style

sns.set_style('darkgrid')

\#plot the two variables with the 'weather' variable as 'hue'

sns.lmplot(x="avg_hrs_worked", y="life_expect", hue="weather", data=df_w, height=8);

\`\`\`

    <Figure size 1440x1440 with 0 Axes>

!\[png\](output_98_1.png)

It looks like there's still a negative correlation between life expectancy and hours worked in both hot and cold countries but, interestingly, life expectancy in hot countries seems to be generally higher than in cold countries.

It would be even more interesting to explore this relationship as a function of a 4th variable - pollution. Does life expectancy decrease the more polluted a country's air is as well as the colder it is and the more hours people work?

To add another variable, you can draw multiple “facets” which each level of the variable appearing in the rows or columns of the grid.

First, let's add the pollution data variable to the dataframe:

\`\`\`python

\#query database and create DF

df_w = pd.read_sql_query("SELECT w.country, w.avg_hrs_worked, l.life_expect, avg_temp, prct_rpt_pollution FROM work_hours AS w INNER JOIN life_expectancy as l ON w.country=l.country INNER JOIN weather ON w.country=weather.country INNER JOIN pollution ON w.country=pollution.country", engine)

def fahr_to_celsius(temp_fahr):

    """Convert Fahrenheit to Celsius
    
    
    
    Return Celsius conversion of input"""
    
    temp_celsius = (temp_fahr - 32) * 5 / 9
    
    return temp_celsius

\#recreate new column and apply the function to existing column

df_w\["avg_temp_c"\] = fahr_to_celsius(df_w\["avg_temp"\])

\#recreate new weather col again as per above

df_w\['weather'\] = df_w\['avg_temp_c'\].apply(lambda x: 'cold' if x <= 10 else 'warm')

\`\`\`

\`\`\`python

\#create new col and categorise pollution levels

df_w\['pollution'\] = df_w\['prct_rpt_pollution'\].apply(lambda x: 'low' if x <= 10 else 'high')

\`\`\`

\`\`\`python

\#plot the 4 variables in 2 columns

sns.set_style('darkgrid')

sns.lmplot(x="avg_hrs_worked", y="life_expect", hue="weather", col="pollution", data=df_w, height=8, aspect=.8);

\`\`\`

!\[png\](output_102_0.png)

\## f) Pairplot

Using the pairplot() function with kind="reg" combines regplot() and PairGrid to show the linear relationship between variables in a dataset. Note that this function is different from lmplot().

In the figure below, the two axes don’t show the same relationship conditioned on two levels of a third variable. Instead, PairGrid() is used to show multiple relationships between different pairings of the variables in a dataset.

We'll compare the relationship between life expectancy and pollution levels in one axis and the relationship between life expectancy and weather in the other axis.

\`\`\`python

\#let's display the dataframe again

df_w

\`\`\`

<div>

<style scoped>

    .dataframe tbody tr th:only-of-type {
    
        vertical-align: middle;
    
    }
    
    .dataframe tbody tr th {
    
        vertical-align: top;
    
    }
    
    .dataframe thead th {
    
        text-align: right;
    
    }

</style>

<table border="1" class="dataframe">

<thead>

    <tr style="text-align: right;">
    
      <th></th>
    
      <th>country</th>
    
      <th>avg_hrs_worked</th>
    
      <th>life_expect</th>
    
      <th>avg_temp</th>
    
      <th>prct_rpt_pollution</th>
    
      <th>avg_temp_c</th>
    
      <th>weather</th>
    
      <th>pollution</th>
    
    </tr>

</thead>

<tbody>

    <tr>
    
      <th>0</th>
    
      <td>Belgium</td>
    
      <td>37.0</td>
    
      <td>81.5</td>
    
      <td>48.8</td>
    
      <td>13.2</td>
    
      <td>9.333333</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>1</th>
    
      <td>Bulgaria</td>
    
      <td>40.8</td>
    
      <td>74.9</td>
    
      <td>49.4</td>
    
      <td>15.1</td>
    
      <td>9.666667</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>2</th>
    
      <td>Czechia</td>
    
      <td>40.3</td>
    
      <td>79.1</td>
    
      <td>44.5</td>
    
      <td>13.5</td>
    
      <td>6.944444</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>3</th>
    
      <td>Denmark</td>
    
      <td>32.9</td>
    
      <td>80.9</td>
    
      <td>46.0</td>
    
      <td>6.8</td>
    
      <td>7.777778</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>4</th>
    
      <td>Germany</td>
    
      <td>35.1</td>
    
      <td>81.0</td>
    
      <td>46.6</td>
    
      <td>23.2</td>
    
      <td>8.111111</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>5</th>
    
      <td>Estonia</td>
    
      <td>38.4</td>
    
      <td>78.0</td>
    
      <td>41.9</td>
    
      <td>9.9</td>
    
      <td>5.500000</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>6</th>
    
      <td>Ireland</td>
    
      <td>35.9</td>
    
      <td>81.8</td>
    
      <td>49.4</td>
    
      <td>4.6</td>
    
      <td>9.666667</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>7</th>
    
      <td>Greece</td>
    
      <td>42.3</td>
    
      <td>81.5</td>
    
      <td>62.5</td>
    
      <td>19.6</td>
    
      <td>16.944444</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>8</th>
    
      <td>Spain</td>
    
      <td>37.7</td>
    
      <td>83.5</td>
    
      <td>60.0</td>
    
      <td>10.1</td>
    
      <td>15.555556</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>9</th>
    
      <td>France</td>
    
      <td>37.3</td>
    
      <td>82.7</td>
    
      <td>52.3</td>
    
      <td>14.1</td>
    
      <td>11.277778</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>10</th>
    
      <td>Croatia</td>
    
      <td>39.4</td>
    
      <td>78.2</td>
    
      <td>55.1</td>
    
      <td>7.0</td>
    
      <td>12.833333</td>
    
      <td>warm</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>11</th>
    
      <td>Italy</td>
    
      <td>37.0</td>
    
      <td>83.4</td>
    
      <td>56.3</td>
    
      <td>15.1</td>
    
      <td>13.500000</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>12</th>
    
      <td>Cyprus</td>
    
      <td>39.2</td>
    
      <td>82.7</td>
    
      <td>66.1</td>
    
      <td>9.2</td>
    
      <td>18.944444</td>
    
      <td>warm</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>13</th>
    
      <td>Latvia</td>
    
      <td>38.7</td>
    
      <td>74.9</td>
    
      <td>42.9</td>
    
      <td>17.2</td>
    
      <td>6.055556</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>14</th>
    
      <td>Lithuania</td>
    
      <td>38.5</td>
    
      <td>74.9</td>
    
      <td>43.4</td>
    
      <td>15.6</td>
    
      <td>6.333333</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>15</th>
    
      <td>Luxembourg</td>
    
      <td>37.5</td>
    
      <td>82.7</td>
    
      <td>46.7</td>
    
      <td>16.1</td>
    
      <td>8.166667</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>16</th>
    
      <td>Hungary</td>
    
      <td>39.7</td>
    
      <td>76.2</td>
    
      <td>50.3</td>
    
      <td>12.8</td>
    
      <td>10.166667</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>17</th>
    
      <td>Malta</td>
    
      <td>38.7</td>
    
      <td>82.6</td>
    
      <td>65.7</td>
    
      <td>30.2</td>
    
      <td>18.722222</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>18</th>
    
      <td>Netherlands</td>
    
      <td>30.3</td>
    
      <td>81.7</td>
    
      <td>49.2</td>
    
      <td>13.2</td>
    
      <td>9.555556</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>19</th>
    
      <td>Austria</td>
    
      <td>36.5</td>
    
      <td>81.8</td>
    
      <td>44.6</td>
    
      <td>10.7</td>
    
      <td>7.000000</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>20</th>
    
      <td>Poland</td>
    
      <td>40.7</td>
    
      <td>78.0</td>
    
      <td>44.6</td>
    
      <td>11.4</td>
    
      <td>7.000000</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>21</th>
    
      <td>Portugal</td>
    
      <td>39.4</td>
    
      <td>81.3</td>
    
      <td>60.4</td>
    
      <td>13.1</td>
    
      <td>15.777778</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>22</th>
    
      <td>Romania</td>
    
      <td>39.9</td>
    
      <td>75.3</td>
    
      <td>47.6</td>
    
      <td>14.5</td>
    
      <td>8.666667</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>23</th>
    
      <td>Slovenia</td>
    
      <td>39.3</td>
    
      <td>81.2</td>
    
      <td>46.4</td>
    
      <td>15.9</td>
    
      <td>8.000000</td>
    
      <td>cold</td>
    
      <td>high</td>
    
    </tr>
    
    <tr>
    
      <th>24</th>
    
      <td>Slovakia</td>
    
      <td>40.1</td>
    
      <td>77.3</td>
    
      <td>43.2</td>
    
      <td>9.3</td>
    
      <td>6.222222</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>25</th>
    
      <td>Finland</td>
    
      <td>36.7</td>
    
      <td>81.5</td>
    
      <td>36.6</td>
    
      <td>7.2</td>
    
      <td>2.555556</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>26</th>
    
      <td>Sweden</td>
    
      <td>36.4</td>
    
      <td>82.4</td>
    
      <td>40.3</td>
    
      <td>6.3</td>
    
      <td>4.611111</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>27</th>
    
      <td>United Kingdom</td>
    
      <td>36.6</td>
    
      <td>81.2</td>
    
      <td>48.8</td>
    
      <td>9.0</td>
    
      <td>9.333333</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>28</th>
    
      <td>Iceland</td>
    
      <td>39.3</td>
    
      <td>82.2</td>
    
      <td>38.5</td>
    
      <td>7.9</td>
    
      <td>3.611111</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>29</th>
    
      <td>Norway</td>
    
      <td>33.8</td>
    
      <td>82.5</td>
    
      <td>39.8</td>
    
      <td>6.8</td>
    
      <td>4.333333</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>30</th>
    
      <td>Switzerland</td>
    
      <td>34.7</td>
    
      <td>83.7</td>
    
      <td>42.5</td>
    
      <td>8.9</td>
    
      <td>5.833333</td>
    
      <td>cold</td>
    
      <td>low</td>
    
    </tr>
    
    <tr>
    
      <th>31</th>
    
      <td>Turkey</td>
    
      <td>46.8</td>
    
      <td>78.1</td>
    
      <td>54.2</td>
    
      <td>24.5</td>
    
      <td>12.333333</td>
    
      <td>warm</td>
    
      <td>high</td>
    
    </tr>

</tbody>

</table>

</div>

\`\`\`python

sns.set_style("whitegrid")

sns.pairplot(df_w, x_vars=\["prct_rpt_pollution", "avg_temp_c"\], y_vars=\["life_expect"\],

             height=8, aspect=.8, kind="reg");

\`\`\`

!\[png\](output_105_0.png)

Interesting (but not surprising) to see that the central tendency is for life expectancy to decrease as pollution levels increase and for life expectancy to increase as average temperatures increase in a country.

\# 8. Tableau visualisations

In conclusion, it seems that the EU countries with the best quality of life would be those where there's:

\- high GDP

\- low unemployment and underemployment

\- high life satisfaction

\- lower hours worked per week

\- low pollution

\- better weather

The question is which countries deliver on all these variables?

Here's a comparison of key economic variables by EU country. It looks like Iceland is doing best in terms of unemployment and underemployment rates, but its GDP is fairly low.

!\[eu_economic_viz_tableau.png\](attachment:eu_economic_viz_tableau.png)

Next, we can compare the countries based on citizens response to surveys examining the trust level in the police, political system and legal system.

!\[eu_trus_tableau.png\](attachment:eu_trus_tableau.png)

This shows us that while no country scores high on all 3 variables, most of the nordic countries and Switzerland seem to score higher than the rest.

If we wanted to see what average weekly work hours look like across the EU we can simply plot a map viz in Tableau. This clearly shows that the Netherlands and nordic countries are on the lower end of weekly hours worked.

!\[eu_hours.png\](attachment:eu_hours.png)

It's interesting to visualise the high job satisfaction scores across the EU. Clearly, the nordic countries, Austria and Switzerland are well ahead of the rest.

!\[eu_job_satis.png\](attachment:eu_job_satis.png)

And finally, let's compare life expectancy across the EU.

!\[eu_life_expect.png\](attachment:eu_life_expect.png)

Finally, we could create a dashboard in Tableau that combines the above worksheets so that all the data is available at a glance:

!\[eu_dashboard.png\](attachment:eu_dashboard.png)