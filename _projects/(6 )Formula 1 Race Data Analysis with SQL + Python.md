---
name: Formula 1 Race Data Analysis with SQL + Python 
tools: [SQL, Python, Pandas, Squarify, Seaborn, Matplotlib,Altair]
image: https://wallpapercave.com/wp/wp8757587.png
description: SQL data extraction and tranformation project with Python visualization.
---

![jpg](https://pbs.twimg.com/media/D4Q5Gu9WAAEDDZ8.jpg)

I've always been a big fan of Formula 1 especially for its technological advancements shown by each team as time progresses. For starters, here is a brief introduction of Formula 1.

**Championships** - There are two different titles in F1, the World Driver's Championship (WDC), which goes to the driver with the most individual points at the end of the season, and the World Constructor's Championship (WCC), which goes to the team with the most points between their two drivers. Every team fields 2 cars in every race. There are generally ~18 races or so, but the amount changes frequently as races are added or dropped.

**Scoring** - Scoring is based purely off of where you finish the race. It used to be that only the top 6 placing drivers scored, then it was changed to 8, and more recently it was changed to 10. 1st place is worth 25 points, then 18, 15, 12, 10, 8, 6, 4, 2, and 1 respectively.

This project will be using SQL queries and data visualiztion with Python for data analysis. we will be using SQL for extracting and trandforming the data before doing visualization with python. 

# 1. Questions

This analysis will try to answer the following questions:

**Drivers**
1. Which driver had the most F1 World Driver's Champion title?
2. Which driver had obtained the most pole position?
3. Which driver had the most points throughout their career?
4. How many F1 drivers that actually won at least one race?
5. What are the nationality of most F1 drivers?

**Constructors**
1. Who are the all time winning teams since 1950? 
2. Which teams are the earliest to participate in F1? 
3. What are the F1 constructors nationality? 
4. Who are the winning teams since the Hybrid era? 
5. How are the performance of the 2021 teams before? 

**F1 circuits and races**
1. Where are most of the F1 tracks located? 
2. What are the the fastest lap time for each Grand Prix? 
3. How many F1 races are there for each season?

# 2. Measurement Priorities

For Drivers:

* Drivers with the maximum total points for each year.
* Count of drivers where located at grid position 1.
* Sum of points by each drivers for all year.
* Count of drivers where wins are more than 1.
* Sum of drivers nationality count.

For Constructors:

* Total race wins count for each teams.
* First race date by each team.
* Count of teams nationality.
* Total race wins for teams that participate in 2014 and onwards.
* Sum of points for each year for teams participating in the 2021 F1 season.

For F1 circuits and races:

* Geolocation of the F1 circuits.
* Minimum lap time for each Grand Prix
* Count of race for each year.

# 3. Data Collection

* Source
    * The data is obtained from the Kaggle.

* Storage
    * Data is loaded into local database thorugh Microsoft SQL Database.

##### [Github link for project](https://github.com/ameerhaziq20/Malaysia-s-Death-Cause-Analysis-2019)
---
**Notebook Contents:**
 * [Connect to database](#connect_to_database)
 * [Drivers](#drivers)
     1. [Which driver had the most F1 World Driver's Champion title?](#world_champion_title)
     2. [Which driver had obtained the most pole position?](#most_pole)
     3. [Which driver had the most points throughout their career?](#driver_most_points)
     4. [How many F1 drivers that actually won at least one race?](#driver_won_once)
     5. [What are the nationality of most F1 drivers?](#driver_nationality)
     
     
 * [Constructors](#constructors)
     1. [Who are the all time winning teams since 1950?](#all_time_win_team)
     2. [Which teams are the earliest to participate in F1?](#earliest_teams)
     3. [What are the F1 constructors nationality?](#constructors_nationality)
     4. [Who are the winning teams since the Hybrid era?](#winning_hybrid)
     5. [How are the performance of the 2021 teams before?](#2021_team_perf)
     

 * [F1 circuits and races](#circuits_race)
     1. [Where are most of the F1 tracks located?](#tracks_location)
     2. [What are the the fastest lap time for each Grand Prix?](#fastest_lap_time_gp)
     3. [How many F1 races are there for each season?](#season_race_count)  

<a id='connect_to_database'></a>
# Connect to database


```python
# import libraries
import sqlalchemy
import pyodbc
import pandas as pd
import numpy as np

pyodbc.drivers()
```




    ['SQL Server',
     'Microsoft Access Driver (*.mdb, *.accdb)',
     'Microsoft Excel Driver (*.xls, *.xlsx, *.xlsm, *.xlsb)',
     'Microsoft Access Text Driver (*.txt, *.csv)',
     'SQL Server Native Client 11.0',
     'SQL Server Native Client RDA 11.0',
     'ODBC Driver 17 for SQL Server']




```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql
    


```python
%sql mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
```

# Extract, transform, and visualization


```python
#overall styling
import matplotlib.pyplot as plt
import seaborn as sns
import altair as alt

sns.set_style("darkgrid")
palette = ['#004146','#018076','#03BFB5','#949398']
seq_palette_m = 'viridis'
seq_palette_h =  sns.dark_palette("#e76f51")
b_figsize = (14,8)
```

<a id='drivers'></a>
## Drivers

<a id='world_champion_title'></a>
### 1. Which driver had the most F1 World Driver's Champion title?


```sql
%%sql world_title <<

WITH cte as (
      SELECT CONCAT(forename, ' ', surname) AS driver_name,
             year, SUM(points) as race_points,
             RANK() OVER (PARTITION BY year ORDER BY SUM(points) DESC) as seqnum
      FROM results r JOIN 
           constructors c
           ON r.constructorId = c.constructorId JOIN
           drivers d
           ON r.driverId = d.driverId JOIN
           races rc
           ON r.raceId = rc.raceId
      GROUP BY CONCAT(forename, ' ', surname), year
     )
SELECT *
FROM cte
WHERE seqnum = 1 AND year<=2020;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable world_title
    


```python
df_world_title = world_title.DataFrame()
```


```python
df_world_title.tail()
```




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
      <th>driver_name</th>
      <th>year</th>
      <th>race_points</th>
      <th>seqnum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>66</th>
      <td>Nico Rosberg</td>
      <td>2016</td>
      <td>385.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Lewis Hamilton</td>
      <td>2017</td>
      <td>363.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Lewis Hamilton</td>
      <td>2018</td>
      <td>408.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Lewis Hamilton</td>
      <td>2019</td>
      <td>413.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Lewis Hamilton</td>
      <td>2020</td>
      <td>347.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.countplot(data=df_world_title,x='driver_name',
              order=df_world_title['driver_name'].value_counts().index,
            palette=seq_palette_m)
plt.title("World Driver's Championship title count")
plt.xlabel("Driver name")
plt.ylabel("Count of titles")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_12_0.png)
    


**Description:** Since the 2021 season is stil on going, we will only consider the data up untill 2020. Here we can see that Lewis Hamilton and Michael Schumacher won the most World Driver Championship title. So far, only 32 driver's had won the title.

<a id='most_pole'></a>
### 2. Which driver had obtained the most pole position?


```sql
%%sql pole_position <<

WITH cte AS(

SELECT grid,
CONCAT(forename,' ',surname ) AS driver_name,
COUNT(driverRef) OVER(PARTITION BY driverRef) AS pole_count
FROM
    results AS r
    INNER JOIN races AS rc
    ON r.raceId = rc.raceId
    
    INNER JOIN constructors AS c
    ON r.constructorId = c.constructorId
    
    INNER JOIN drivers AS d
    ON r.driverId = d.driverId
        
WHERE grid = 1)

SELECT TOP 10 driver_name,MAX(pole_count) AS pole

FROM cte
GROUP BY driver_name
ORDER BY pole DESC
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable pole_position
    


```python
df_pole_position = pole_position.DataFrame()
```


```python
df_pole_position.head()
```




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
      <th>driver_name</th>
      <th>pole</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lewis Hamilton</td>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Michael Schumacher</td>
      <td>68</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ayrton Senna</td>
      <td>65</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sebastian Vettel</td>
      <td>57</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jim Clark</td>
      <td>34</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.barplot(data=df_pole_position,
            x='driver_name',
            y='pole',
            palette=seq_palette_m)
plt.title("Count of pole position obtained by drivers")
plt.xlabel("Driver name")
plt.ylabel("Count of pole positions")
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_18_0.png)
    


**Description:** From the chart we can see that Lewis Hamilton had obtained 100 pole position. Considering that Lewis Hamilton entered F1 later than most of the drivers in this chart, he still manage to secure a lot of pole position in a short window of time.

<a id='driver_most_points'></a>
### 3. Which driver had the most points throughout their career?


```sql
%%sql f1_drivers_points <<

SELECT TOP 10 driver_name, SUM(driver_points) AS total_points
FROM
    (SELECT

        (CONVERT(DECIMAL, points)) AS driver_points,
        concat(forename, ' ', surname) AS driver_name

    FROM 
        results AS r

        INNER JOIN constructors AS c
        ON r.constructorId = c.constructorId

        INNER JOIN drivers AS d
        ON r.driverId = d.driverId

        INNER JOIN races AS rc
        ON r.raceId = rc.raceId

        WHERE points >= '1') AS subquery
    
GROUP BY driver_name
ORDER BY total_points DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable f1_drivers_points
    


```python
df_f1_drivers_points = f1_drivers_points.DataFrame()
```


```python
df_f1_drivers_points.head()
```




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
      <th>driver_name</th>
      <th>total_points</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lewis Hamilton</td>
      <td>3928</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sebastian Vettel</td>
      <td>3048</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Fernando Alonso</td>
      <td>1919</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kimi Räikkönen</td>
      <td>1864</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Valtteri Bottas</td>
      <td>1604</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.barplot(data=df_f1_drivers_points, x='driver_name', y='total_points',palette='viridis')
plt.title("Total points of F1 Drivers")
plt.xlabel("Driver name")
plt.ylabel("Total points")
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_24_0.png)
    


**Description:** In terms of total points, we can see that Lewis Hamilton had the most points in F1 history. Up to this point we can see that Lewis Hamilton had a great F1 career.

One interesting point that we can see here is that the drivers in this chart are mostly the drivers that are competing in the later seasons of F1. Could it possibly that the drivers are able to obtain more points during the later season in comparison to the earlier F1 season? We'll discuss that further in the "F1 circuits and races" section.

<a id='driver_won_once'></a>
### 4. How many F1 drivers that actually won at least one race?


```sql
%%sql f1_drivers_win_count <<

Select driver_name, COUNT(driver_name) AS frequency
FROM
    (SELECT
        concat(forename, ' ', surname) AS driver_name

    FROM 
        results AS r

        INNER JOIN constructors AS c
        ON r.constructorId = c.constructorId

        INNER JOIN drivers AS d
        ON r.driverId = d.driverId

        INNER JOIN races AS rc
        ON r.raceId = rc.raceId

        WHERE positionText = '1') AS subquery

GROUP BY driver_name
ORDER BY frequency DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable f1_drivers_win_count
    


```python
df_f1_drivers_win_count  = f1_drivers_win_count.DataFrame()
```


```python
df_f1_drivers_win_count.tail()
```




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
      <th>driver_name</th>
      <th>frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>105</th>
      <td>Vittorio Brambilla</td>
      <td>1</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Sam Hanks</td>
      <td>1</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Richie Ginther</td>
      <td>1</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Robert Kubica</td>
      <td>1</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Rodger Ward</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=(10,20))
sns.barplot(data=df_f1_drivers_win_count,
            x='frequency',
            y='driver_name',
            palette=seq_palette_m)

plt.title('Driver wins since 1950')
plt.ylabel('Driver names')
plt.xlabel('Count of race wins')
plt.show()
```


    
![png](output_30_0.png)
    


**Description:** From the data, we can see that there are 110 drivers that at least obtain 1 race win (1st position) in each of those races. From the frequency of wins, Lewis Hamilton is having nearly 100 wins (96 to be exact) which is the highest total wins since 1950.

<a id='driver_nationality'></a>
### 5. What are the nationality of most F1 drivers?


```sql
%%sql f1_drivers_nationality <<


SELECT
    nationality, COUNT(nationality) AS frequency

FROM 
    drivers
    
GROUP BY nationality
ORDER BY frequency DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable f1_drivers_nationality
    


```python
df_f1_drivers_nationality =f1_drivers_nationality.DataFrame()
```


```python
df_f1_drivers_nationality.head()
```




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
      <th>nationality</th>
      <th>frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>British</td>
      <td>165</td>
    </tr>
    <tr>
      <th>1</th>
      <td>American</td>
      <td>157</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Italian</td>
      <td>99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>French</td>
      <td>73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>German</td>
      <td>50</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.barplot(data=df_f1_drivers_nationality, x='nationality', y='frequency',palette='viridis')
plt.title("Count of driver's nationality")
plt.xlabel("Nationality")
plt.ylabel("Count of drivers")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_36_0.png)
    


**Description:** Most of the Formula 1 drivers are British, which is quite as expected since most of the teams are based in UK. American drivers also come quite close in terms of total drivers but in terms of total teams. This shows that there is a possibility that the American drivers driver in teams that aren't American teams.

<a id='constructors'></a>
## Constructors

<a id='all_time_win_team'></a>
### 1. Who are the all time winning teams since 1950?


```sql
%%sql win_all_time <<

SELECT name,SUM(wins) AS total_wins
FROM 
    constructor_standings AS cs
    
    INNER JOIN constructors AS c
    ON cs.constructorId = c.constructorId
    
    INNER JOIN races AS r
    ON cs.raceId = r.raceId
    
    WHERE wins>=1
    GROUP BY name
    ORDER BY total_wins DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable win_all_time
    


```python
df_win_all_time = win_all_time.DataFrame()
```


```python
df_win_all_time.head()
```




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
      <th>name</th>
      <th>total_wins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ferrari</td>
      <td>1856</td>
    </tr>
    <tr>
      <th>1</th>
      <td>McLaren</td>
      <td>1595</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mercedes</td>
      <td>1108</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Williams</td>
      <td>934</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Red Bull</td>
      <td>580</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=(10,10))
sns.barplot(data=df_win_all_time,
            x='total_wins',
            y='name',
            palette=seq_palette_m)
plt.title('Number of wins by constructors since 1950')

plt.xlabel("Count of race wins")
plt.ylabel("Constructor names")
plt.show()
```


    
![png](output_43_0.png)
    


<a id='earliest_team'></a>
### 2. Which teams are the earliest to participate in F1?


```sql
%%sql team_orgin_date <<

SELECT name, MAX(DATEDIFF(year, date,CURRENT_TIMESTAMP)) AS team_age_years, MIN(date) AS first_race

FROM 
    constructor_standings AS cs
    
    INNER JOIN constructors AS c
    ON cs.constructorId = c.constructorId
    
    INNER JOIN races AS r
    ON cs.raceId = r.raceId
    
    GROUP BY name
    ORDER BY team_age_years DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable team_orgin_date
    


```python
df_team_orgin_date = team_orgin_date.DataFrame()
```


```python
df_team_orgin_date.head(5)
```




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
      <th>name</th>
      <th>team_age_years</th>
      <th>first_race</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Maserati</td>
      <td>63</td>
      <td>1958-01-19 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ferrari</td>
      <td>63</td>
      <td>1958-01-19 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Team Lotus</td>
      <td>63</td>
      <td>1958-05-18 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BRM</td>
      <td>63</td>
      <td>1958-05-18 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Connaught</td>
      <td>63</td>
      <td>1958-05-18 00:00:00.0000000</td>
    </tr>
  </tbody>
</table>
</div>



**Description:** From the table, we can see that among the eraliest team to participate since the inaugural season is Maserati, Ferrari, and Cooper. The list also includes Team Lotus and BRM. Just out of curiosity, let's see the teams that are recently created. 


```python
df_team_orgin_date.tail(5)
```




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
      <th>name</th>
      <th>team_age_years</th>
      <th>first_race</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>154</th>
      <td>Manor Marussia</td>
      <td>6</td>
      <td>2015-03-29 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Haas F1 Team</td>
      <td>5</td>
      <td>2016-03-20 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Racing Point</td>
      <td>2</td>
      <td>2019-03-17 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>157</th>
      <td>AlphaTauri</td>
      <td>1</td>
      <td>2020-07-05 00:00:00.0000000</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Alpine F1 Team</td>
      <td>0</td>
      <td>2021-03-28 00:00:00.0000000</td>
    </tr>
  </tbody>
</table>
</div>



**Description:** Alpine team seems to be the most recently created constructors team followed by AlphaTauri and Racing Point. 3 years before that, Haas F1 Team starts their first F1 debut with Manor Marussia on the year before it. Apparently, Manor Marussia is short-lived and no longer participating in F1. Next, let's visualize all of the team age in one plot.


```python
# import pandas for data wrangling

def circular_plot(dataframe,value,item,title):
    # Build a dataset

    # Reorder the dataframe
    df = dataframe
    
    # sort (I've sorted the data before so there's no need to sort again)
    # df = dataframe.sort_values(by=[value])

    # initialize the figure
    plt.figure(figsize=(20,10))
    ax = plt.subplot(111, polar=True)
    ax.text(x=17,y=0,s=title,backgroundcolor="#2FD0CB", color='white',ha='center',size=20)
    plt.axis('off')

    # Constants = parameters controling the plot layout:
    upperLimit = 100
    lowerLimit = 30
    labelPadding = 4

    # Compute max and min in the dataset
    max = df[value].max()

    # Let's compute heights: they are a conversion of each item value in those new coordinates
    # In our example, 0 in the dataset will be converted to the lowerLimit (10)
    # The maximum will be converted to the upperLimit (100)
    slope = (max - lowerLimit) / max
    heights = slope * df[value] + lowerLimit

    # Compute the width of each bar. In total we have 2*Pi = 360°
    width = 2*np.pi / len(df.index)

    # Compute the angle each bar is centered on:
    indexes = list(range(1, len(df.index)+1))
    angles = [element * width for element in indexes]
    angles

    # Draw bars
    bars = ax.bar(
        x=angles, 
        height=heights, 
        width=width, 
        bottom=lowerLimit,
        linewidth=2, 
        edgecolor="white",
        color="#61a4b2",
    )
    

    # Add labels
    for bar, angle, height, label in zip(bars,angles, heights, df[item]):

        # Labels are rotated. Rotation must be specified in degrees :(
        rotation = np.rad2deg(angle)

        # Flip some labels upside down
        alignment = ""
        if angle >= np.pi/2 and angle < 3*np.pi/2:
            alignment = "right"
            rotation = rotation + 180
        else: 
            alignment = "left"

        # Finally add the labels
        ax.text(
            x=angle, 
            y=lowerLimit + bar.get_height() + labelPadding, 
            s=label, 
            ha=alignment, 
            va='center', 
            rotation=rotation, 
            rotation_mode="anchor") 
        

title = 'F1 team age'
circular_plot(df_team_orgin_date,'team_age_years','name',title)
```


    
![png](output_51_0.png)
    


<a id='constructors_nationality'></a>
### 3. What are the F1 constructors nationality?


```sql
%%sql teams_nation <<

SELECT nationality, COUNT(nationality) AS count_teams

FROM 
    constructors AS c
    
    GROUP BY nationality
    ORDER BY count_teams DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable teams_nation
    


```python
df_teams_nation = teams_nation.DataFrame()
```


```python
df_teams_nation.head()
```




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
      <th>nationality</th>
      <th>count_teams</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>British</td>
      <td>86</td>
    </tr>
    <tr>
      <th>1</th>
      <td>American</td>
      <td>39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Italian</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>French</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>German</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.barplot(data=df_teams_nation, x='nationality',
            y='count_teams',
            palette=seq_palette_m)
plt.title('Nationality of F1 Teams')
plt.xlabel("Nationality")
plt.ylabel("Count of constructors")
plt.xticks(rotation=50)
plt.show()
```


    
![png](output_56_0.png)
    


**Description:** From the chart, we can see most of the team's nationality is British which twice more than the count of American teams. [From this aricle by BBC](https://www.bbc.com/sport/formula1/23048643), it is mostly due to its history and heritage. The UK had lots of unused airfields for racing and testing. Similar to Silicon Valley, having a “motorsport valley” attracts a lot of engineers who live close to each other which leads to a high supply of quality workforce.

<a id='winning_hybrid'></a>
### 4. Who are the winning teams since the Hybrid era?


```sql
%%sql wins_hybrid <<

SELECT name,SUM(wins) AS total_wins
FROM 
    constructor_standings AS cs
    
    INNER JOIN constructors AS c
    ON cs.constructorId = c.constructorId
    
    INNER JOIN races AS r
    ON cs.raceId = r.raceId
    
    WHERE r.year >= '2014' AND wins>=1
    GROUP BY name
    ORDER BY total_wins DESC;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable wins_hybrid
    


```python
df_wins_hybrid = wins_hybrid.DataFrame()
```


```python
df_wins_hybrid
```




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
      <th>name</th>
      <th>total_wins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercedes</td>
      <td>1054</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ferrari</td>
      <td>205</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Red Bull</td>
      <td>166</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AlphaTauri</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Racing Point</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=b_figsize)
sns.barplot(data=df_wins_hybrid,
            x='name',
            y='total_wins',
            palette=seq_palette_m)
plt.title('Number of wins by team in the Hybrid era')
plt.xlabel("Constructor name")
plt.ylabel("Count of wins")
plt.show()
```


    
![png](output_62_0.png)
    


**Description:** From the analysis, we can see that the Mercedes team had totaly dominated the F1 championship during the hybrid era which is from 2014 and onwards. Interestingly, as we can see from the plot, most of the races are won by these 5 teams since 2014.

<a id='2021_team_perf'></a>
### 5. How are the performance of the 2021 teams before?


```sql
%%sql performance_2021_c <<

SELECT name AS constructor, YEAR(CONVERT(datetime, date)) AS year, SUM(CAST(points AS float)) total_points

FROM

    constructor_results AS cr

    INNER JOIN constructors AS c
    ON cr.constructorId = c.constructorId

    INNER JOIN races AS r
    ON cr.raceId = r.raceId
    
    WHERE name IN

        (SELECT DISTINCT c.name

        FROM 
            constructor_results AS cr

            INNER JOIN constructors AS c
            ON cr.constructorId = c.constructorId

            INNER JOIN races AS r
            ON cr.raceId = r.raceId

           WHERE date >= Convert(datetime, '2020-12-31' ))
        
GROUP BY YEAR(CONVERT(datetime, date)),name
ORDER BY year;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable performance_2021_c
    


```python
df_performance_2021_c =performance_2021_c.DataFrame()
```


```python
df_performance_2021_c.head()
```




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
      <th>constructor</th>
      <th>year</th>
      <th>total_points</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ferrari</td>
      <td>1956</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ferrari</td>
      <td>1958</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Aston Martin</td>
      <td>1959</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ferrari</td>
      <td>1959</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aston Martin</td>
      <td>1960</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=(15,8))

sns.scatterplot(
    data=df_performance_2021_c,
    x="year",
    y='total_points',
    hue="constructor",
    size='total_points',
    sizes=(10,500),
    alpha=0.9,
    palette="hls"
)
plt.title("Performance of the 2021 teams throughout time")
plt.xlabel("Year")
plt.ylabel("Total race points")
plt.show()
```


    
![png](output_68_0.png)
    


**Description:** From the plot, we can see that Ferarri are among the oldest team to compete in the 2021 followed by Aston Martin and Alfa Romeo. It seems like the last two aforementioned constructors haven't score any significant points past 1970. This may be due to the teams hasn't participated for a while and making their return at the 2020 season.

We can also see that Red Bull had been winning a lot of points upon the year 2010 as well as the Mercedes team earliest obtained points. Even though they starts participating races in the later seasons, it seems like the team dominates the season beyond 2014 which is the start of the hybrid era.

What's even interesting is that Ferrari had a sudden drop of points nearing the 2020 season. [From this article](https://www.autosport.com/f1/news/binotto-ferrari-performance-so-far-in-2021-is-a-relief/6494159/), it seems like the Ferrari team's car for that season is suffering from performance and design issues.

<a id='circuits_race'></a>
## F1 circuits and races

<a id='tracks_location'></a>
### 1. Where are most of the F1 tracks located?


```sql
%%sql circuit_map <<

SELECT 
    name,
    location,
    country,
    lat,
    lng,
    CONVERT(INT,alt) as alt
FROM
    circuits
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable circuit_map
    


```python
df_circuit_map = circuit_map.DataFrame()
```


```python
df_circuit_map.dtypes
```




    name         object
    location     object
    country      object
    lat         float64
    lng         float64
    alt           int64
    dtype: object




```python
import altair as alt
from vega_datasets import data

states = alt.topo_feature(data.world_110m.url, 'countries')
circuits = df_circuit_map

background = alt.Chart(states, title='F1 Circuits location').mark_geoshape(
    fill='lightgray',
    stroke='white'
).project('equalEarth').properties(
    width=500,
    height=300
)

points = alt.Chart(circuits).mark_circle().encode(
    longitude='lng:Q',
    latitude='lat:Q',
    size=alt.value(100),
    tooltip=('name','country','location'),
    color=alt.Color('alt', scale=alt.Scale(scheme=seq_palette_m))
)

background + points
```





<div id="altair-viz-498d53e6eb87414ebec8167486ae509e"></div>
<script type="text/javascript">
  (function(spec, embedOpt){
    let outputDiv = document.currentScript.previousElementSibling;
    if (outputDiv.id !== "altair-viz-498d53e6eb87414ebec8167486ae509e") {
      outputDiv = document.getElementById("altair-viz-498d53e6eb87414ebec8167486ae509e");
    }
    const paths = {
      "vega": "https://cdn.jsdelivr.net/npm//vega@5?noext",
      "vega-lib": "https://cdn.jsdelivr.net/npm//vega-lib?noext",
      "vega-lite": "https://cdn.jsdelivr.net/npm//vega-lite@4.8.1?noext",
      "vega-embed": "https://cdn.jsdelivr.net/npm//vega-embed@6?noext",
    };

    function loadScript(lib) {
      return new Promise(function(resolve, reject) {
        var s = document.createElement('script');
        s.src = paths[lib];
        s.async = true;
        s.onload = () => resolve(paths[lib]);
        s.onerror = () => reject(`Error loading script: ${paths[lib]}`);
        document.getElementsByTagName("head")[0].appendChild(s);
      });
    }

    function showError(err) {
      outputDiv.innerHTML = `<div class="error" style="color:red;">${err}</div>`;
      throw err;
    }

    function displayChart(vegaEmbed) {
      vegaEmbed(outputDiv, spec, embedOpt)
        .catch(err => showError(`Javascript Error: ${err.message}<br>This usually means there's a typo in your chart specification. See the javascript console for the full traceback.`));
    }

    if(typeof define === "function" && define.amd) {
      requirejs.config({paths});
      require(["vega-embed"], displayChart, err => showError(`Error loading script: ${err.message}`));
    } else if (typeof vegaEmbed === "function") {
      displayChart(vegaEmbed);
    } else {
      loadScript("vega")
        .then(() => loadScript("vega-lite"))
        .then(() => loadScript("vega-embed"))
        .catch(showError)
        .then(() => displayChart(vegaEmbed));
    }
  })({"config": {"view": {"continuousWidth": 400, "continuousHeight": 300}}, "layer": [{"data": {"url": "https://cdn.jsdelivr.net/npm/vega-datasets@v1.29.0/data/world-110m.json", "format": {"feature": "countries", "type": "topojson"}}, "mark": {"type": "geoshape", "fill": "lightgray", "stroke": "white"}, "height": 300, "projection": {"type": "equalEarth"}, "title": "F1 Circuits location", "width": 500}, {"data": {"name": "data-fc15649da3660a7f0eeac96edbde2acc"}, "mark": "circle", "encoding": {"color": {"type": "quantitative", "field": "alt", "scale": {"scheme": "viridis"}}, "latitude": {"field": "lat", "type": "quantitative"}, "longitude": {"field": "lng", "type": "quantitative"}, "size": {"value": 100}, "tooltip": [{"type": "nominal", "field": "name"}, {"type": "nominal", "field": "country"}, {"type": "nominal", "field": "location"}]}}], "$schema": "https://vega.github.io/schema/vega-lite/v4.8.1.json", "datasets": {"data-fc15649da3660a7f0eeac96edbde2acc": [{"name": "Albert Park Grand Prix Circuit", "location": "Melbourne", "country": "Australia", "lat": -37.8497, "lng": 144.968, "alt": 10}, {"name": "Sepang International Circuit", "location": "Kuala Lumpur", "country": "Malaysia", "lat": 2.76083, "lng": 101.738, "alt": 18}, {"name": "Bahrain International Circuit", "location": "Sakhir", "country": "Bahrain", "lat": 26.0325, "lng": 50.5106, "alt": 7}, {"name": "Circuit de Barcelona-Catalunya", "location": "Montmel\u00f3", "country": "Spain", "lat": 41.57, "lng": 2.26111, "alt": 109}, {"name": "Istanbul Park", "location": "Istanbul", "country": "Turkey", "lat": 40.9517, "lng": 29.405, "alt": 130}, {"name": "Circuit de Monaco", "location": "Monte-Carlo", "country": "Monaco", "lat": 43.7347, "lng": 7.42056, "alt": 7}, {"name": "Circuit Gilles Villeneuve", "location": "Montreal", "country": "Canada", "lat": 45.5, "lng": -73.5228, "alt": 13}, {"name": "Circuit de Nevers Magny-Cours", "location": "Magny Cours", "country": "France", "lat": 46.8642, "lng": 3.16361, "alt": 228}, {"name": "Silverstone Circuit", "location": "Silverstone", "country": "UK", "lat": 52.0786, "lng": -1.01694, "alt": 153}, {"name": "Hockenheimring", "location": "Hockenheim", "country": "Germany", "lat": 49.3278, "lng": 8.56583, "alt": 103}, {"name": "Hungaroring", "location": "Budapest", "country": "Hungary", "lat": 47.5789, "lng": 19.2486, "alt": 264}, {"name": "Valencia Street Circuit", "location": "Valencia", "country": "Spain", "lat": 39.4589, "lng": -0.331667, "alt": 4}, {"name": "Circuit de Spa-Francorchamps", "location": "Spa", "country": "Belgium", "lat": 50.4372, "lng": 5.97139, "alt": 401}, {"name": "Autodromo Nazionale di Monza", "location": "Monza", "country": "Italy", "lat": 45.6156, "lng": 9.28111, "alt": 162}, {"name": "Marina Bay Street Circuit", "location": "Marina Bay", "country": "Singapore", "lat": 1.2914, "lng": 103.864, "alt": 18}, {"name": "Fuji Speedway", "location": "Oyama", "country": "Japan", "lat": 35.3717, "lng": 138.927, "alt": 583}, {"name": "Shanghai International Circuit", "location": "Shanghai", "country": "China", "lat": 31.3389, "lng": 121.22, "alt": 5}, {"name": "Aut\u00f3dromo Jos\u00e9 Carlos Pace", "location": "S\u00e3o Paulo", "country": "Brazil", "lat": -23.7036, "lng": -46.6997, "alt": 785}, {"name": "Indianapolis Motor Speedway", "location": "Indianapolis", "country": "USA", "lat": 39.795, "lng": -86.2347, "alt": 223}, {"name": "N\u00fcrburgring", "location": "N\u00fcrburg", "country": "Germany", "lat": 50.3356, "lng": 6.9475, "alt": 578}, {"name": "Autodromo Enzo e Dino Ferrari", "location": "Imola", "country": "Italy", "lat": 44.3439, "lng": 11.7167, "alt": 37}, {"name": "Suzuka Circuit", "location": "Suzuka", "country": "Japan", "lat": 34.8431, "lng": 136.541, "alt": 45}, {"name": "A1-Ring", "location": "Spielburg", "country": "Austria", "lat": 47.2197, "lng": 14.7647, "alt": 678}, {"name": "Yas Marina Circuit", "location": "Abu Dhabi", "country": "UAE", "lat": 24.4672, "lng": 54.6031, "alt": 3}, {"name": "Aut\u00f3dromo Juan y Oscar G\u00e1lvez", "location": "Buenos Aires", "country": "Argentina", "lat": -34.6943, "lng": -58.4593, "alt": 8}, {"name": "Circuito de Jerez", "location": "Jerez de la Frontera", "country": "Spain", "lat": 36.7083, "lng": -6.03417, "alt": 37}, {"name": "Aut\u00f3dromo do Estoril", "location": "Estoril", "country": "Portugal", "lat": 38.7506, "lng": -9.39417, "alt": 130}, {"name": "Okayama International Circuit", "location": "Okayama", "country": "Japan", "lat": 34.915, "lng": 134.221, "alt": 266}, {"name": "Adelaide Street Circuit", "location": "Adelaide", "country": "Australia", "lat": -34.9272, "lng": 138.617, "alt": 58}, {"name": "Kyalami", "location": "Midrand", "country": "South Africa", "lat": -25.9894, "lng": 28.0767, "alt": 1460}, {"name": "Donington Park", "location": "Castle Donington", "country": "UK", "lat": 52.8306, "lng": -1.37528, "alt": 88}, {"name": "Aut\u00f3dromo Hermanos Rodr\u00edguez", "location": "Mexico City", "country": "Mexico", "lat": 19.4042, "lng": -99.0907, "alt": 2227}, {"name": "Phoenix street circuit", "location": "Phoenix", "country": "USA", "lat": 33.4479, "lng": -112.075, "alt": 345}, {"name": "Circuit Paul Ricard", "location": "Le Castellet", "country": "France", "lat": 43.2506, "lng": 5.79167, "alt": 432}, {"name": "Korean International Circuit", "location": "Yeongam County", "country": "Korea", "lat": 34.7333, "lng": 126.417, "alt": 0}, {"name": "Aut\u00f3dromo Internacional Nelson Piquet", "location": "Rio de Janeiro", "country": "Brazil", "lat": -22.9756, "lng": -43.395, "alt": 1126}, {"name": "Detroit Street Circuit", "location": "Detroit", "country": "USA", "lat": 42.3298, "lng": -83.0401, "alt": 177}, {"name": "Brands Hatch", "location": "Kent", "country": "UK", "lat": 51.3569, "lng": 0.263056, "alt": 145}, {"name": "Circuit Park Zandvoort", "location": "Zandvoort", "country": "Netherlands", "lat": 52.3888, "lng": 4.54092, "alt": 6}, {"name": "Zolder", "location": "Heusden-Zolder", "country": "Belgium", "lat": 50.9894, "lng": 5.25694, "alt": 36}, {"name": "Dijon-Prenois", "location": "Dijon", "country": "France", "lat": 47.3625, "lng": 4.89913, "alt": 484}, {"name": "Fair Park", "location": "Dallas", "country": "USA", "lat": 32.7774, "lng": -96.7587, "alt": 139}, {"name": "Long Beach", "location": "California", "country": "USA", "lat": 33.7651, "lng": -118.189, "alt": 12}, {"name": "Las Vegas Street Circuit", "location": "Nevada", "country": "USA", "lat": 36.1162, "lng": -115.174, "alt": 639}, {"name": "Jarama", "location": "Madrid", "country": "Spain", "lat": 40.6171, "lng": -3.58558, "alt": 609}, {"name": "Watkins Glen", "location": "New York State", "country": "USA", "lat": 42.3369, "lng": -76.9272, "alt": 485}, {"name": "Scandinavian Raceway", "location": "Anderstorp", "country": "Sweden", "lat": 57.2653, "lng": 13.6042, "alt": 153}, {"name": "Mosport International Raceway", "location": "Ontario", "country": "Canada", "lat": 44.0481, "lng": -78.6756, "alt": 332}, {"name": "Montju\u00efc", "location": "Barcelona", "country": "Spain", "lat": 41.3664, "lng": 2.15167, "alt": 79}, {"name": "Nivelles-Baulers", "location": "Brussels", "country": "Belgium", "lat": 50.6211, "lng": 4.32694, "alt": 139}, {"name": "Charade Circuit", "location": "Clermont-Ferrand", "country": "France", "lat": 45.7472, "lng": 3.03889, "alt": 790}, {"name": "Circuit Mont-Tremblant", "location": "Quebec", "country": "Canada", "lat": 46.1877, "lng": -74.6099, "alt": 214}, {"name": "Rouen-Les-Essarts", "location": "Rouen", "country": "France", "lat": 49.3306, "lng": 1.00458, "alt": 81}, {"name": "Le Mans", "location": "Le Mans", "country": "France", "lat": 47.95, "lng": 0.224231, "alt": 67}, {"name": "Reims-Gueux", "location": "Reims", "country": "France", "lat": 49.2542, "lng": 3.93083, "alt": 88}, {"name": "Prince George Circuit", "location": "Eastern Cape Province", "country": "South Africa", "lat": -33.0486, "lng": 27.8736, "alt": 15}, {"name": "Zeltweg", "location": "Styria", "country": "Austria", "lat": 47.2039, "lng": 14.7478, "alt": 676}, {"name": "Aintree", "location": "Liverpool", "country": "UK", "lat": 53.4769, "lng": -2.94056, "alt": 20}, {"name": "Circuito da Boavista", "location": "Oporto", "country": "Portugal", "lat": 41.1705, "lng": -8.67325, "alt": 28}, {"name": "Riverside International Raceway", "location": "California", "country": "USA", "lat": 33.937, "lng": -117.273, "alt": 470}, {"name": "AVUS", "location": "Berlin", "country": "Germany", "lat": 52.4806, "lng": 13.2514, "alt": 53}, {"name": "Monsanto Park Circuit", "location": "Lisbon", "country": "Portugal", "lat": 38.7197, "lng": -9.20306, "alt": 158}, {"name": "Sebring International Raceway", "location": "Florida", "country": "USA", "lat": 27.4547, "lng": -81.3483, "alt": 18}, {"name": "Ain Diab", "location": "Casablanca", "country": "Morocco", "lat": 33.5786, "lng": -7.6875, "alt": 19}, {"name": "Pescara Circuit", "location": "Pescara", "country": "Italy", "lat": 42.475, "lng": 14.1508, "alt": 129}, {"name": "Circuit Bremgarten", "location": "Bern", "country": "Switzerland", "lat": 46.9589, "lng": 7.40194, "alt": 551}, {"name": "Circuit de Pedralbes", "location": "Barcelona", "country": "Spain", "lat": 41.3903, "lng": 2.11667, "alt": 85}, {"name": "Buddh International Circuit", "location": "Uttar Pradesh", "country": "India", "lat": 28.3487, "lng": 77.5331, "alt": 194}, {"name": "Circuit of the Americas", "location": "Austin", "country": "USA", "lat": 30.1328, "lng": -97.6411, "alt": 161}, {"name": "Red Bull Ring", "location": "Spielburg", "country": "Austria", "lat": 47.2197, "lng": 14.7647, "alt": 678}, {"name": "Sochi Autodrom", "location": "Sochi", "country": "Russia", "lat": 43.4057, "lng": 39.9578, "alt": 2}, {"name": "Port Imperial Street Circuit", "location": "New Jersey", "country": "USA", "lat": 40.7769, "lng": -74.0111, "alt": 4}, {"name": "Baku City Circuit", "location": "Baku", "country": "Azerbaijan", "lat": 40.3725, "lng": 49.8533, "alt": -7}, {"name": "Hanoi Street Circuit", "location": "Hanoi", "country": "Vietnam", "lat": 21.0166, "lng": 105.766, "alt": 9}, {"name": "Aut\u00f3dromo Internacional do Algarve", "location": "Portim\u00e3o", "country": "Portugal", "lat": 37.227, "lng": -8.6267, "alt": 108}, {"name": "Autodromo Internazionale del Mugello", "location": "Mugello", "country": "Italy", "lat": 43.9975, "lng": 11.3719, "alt": 255}, {"name": "Jeddah Street Circuit", "location": "Jeddah", "country": "Saudi Arabia", "lat": 21.5433, "lng": 39.1728, "alt": 15}]}}, {"mode": "vega-lite"});
</script>



**Description:** As we can see from the map, we can see that most of the F1 race tracks are mostly in the Europe and North America. In Asia, there are more track on the East in camparison to the South East. Most of the tracks are at sea level. The track that is very further from sea level which is the Autódromo Hermanos Rodríguez (Mexico city). Track altitudes are taken into consideration by teams since at lower pressures of high altitudes, air molecules spread out more so the air becomes less dense. This will greatly affect the aerodynamics of the car.

<a id='fastest_lap_time_gp'></a>
### 2. What are the the fastest lap time for each Grand Prix?


```sql
%%sql fastest_lap <<

WITH cte as (
      SELECT
    race_name,
    CONCAT(forename, ' ', surname) AS driver_name,
    MIN(l.milliseconds) as time_milli,
    RANK() OVER (PARTITION BY race_name ORDER BY MIN(l.milliseconds)) as seqnum
    FROM 
    
        lap_times AS l
    
        INNER JOIN races AS r
        ON l.raceId = r.raceId

        INNER JOIN drivers AS d
        ON l.driverId = d.driverId
    
    GROUP BY race_name, CONCAT(forename, ' ', surname)

     )
SELECT

race_name,
driver_name,
COUNT(driver_name) OVER(PARTITION BY driver_name) AS driver_freq,
time_milli

FROM cte
WHERE seqnum = 1
ORDER BY race_name;
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable fastest_lap
    


```python
df_fastest_lap = fastest_lap.DataFrame()
```


```python
df_fastest_lap['time_delta'] = pd.to_timedelta(df_fastest_lap['time_milli'], unit='ms')
```


```python
df_fastest_lap.style.background_gradient(cmap='PuBu')
```




<style  type="text/css" >
#T_9c748_row0_col2,#T_9c748_row1_col2,#T_9c748_row7_col3,#T_9c748_row7_col4,#T_9c748_row10_col2,#T_9c748_row13_col2,#T_9c748_row25_col2,#T_9c748_row26_col2,#T_9c748_row27_col2,#T_9c748_row34_col2{
            background-color:  #023858;
            color:  #f1f1f1;
        }#T_9c748_row0_col3,#T_9c748_row0_col4{
            background-color:  #2685bb;
            color:  #000000;
        }#T_9c748_row1_col3,#T_9c748_row1_col4{
            background-color:  #04588a;
            color:  #f1f1f1;
        }#T_9c748_row2_col2,#T_9c748_row5_col2,#T_9c748_row15_col2,#T_9c748_row20_col2,#T_9c748_row22_col2,#T_9c748_row28_col2,#T_9c748_row28_col3,#T_9c748_row28_col4,#T_9c748_row30_col2,#T_9c748_row31_col2,#T_9c748_row32_col2{
            background-color:  #fff7fb;
            color:  #000000;
        }#T_9c748_row2_col3,#T_9c748_row2_col4{
            background-color:  #2a88bc;
            color:  #000000;
        }#T_9c748_row3_col2,#T_9c748_row6_col2,#T_9c748_row9_col2,#T_9c748_row11_col2,#T_9c748_row17_col2,#T_9c748_row29_col2{
            background-color:  #1379b5;
            color:  #f1f1f1;
        }#T_9c748_row3_col3,#T_9c748_row3_col4{
            background-color:  #4e9ac6;
            color:  #000000;
        }#T_9c748_row4_col2,#T_9c748_row7_col2,#T_9c748_row8_col2,#T_9c748_row12_col2,#T_9c748_row14_col2,#T_9c748_row16_col2,#T_9c748_row19_col2,#T_9c748_row24_col2,#T_9c748_row33_col2,#T_9c748_row35_col2{
            background-color:  #e8e4f0;
            color:  #000000;
        }#T_9c748_row4_col3,#T_9c748_row4_col4{
            background-color:  #d8d7e9;
            color:  #000000;
        }#T_9c748_row5_col3,#T_9c748_row5_col4{
            background-color:  #034369;
            color:  #f1f1f1;
        }#T_9c748_row6_col3,#T_9c748_row6_col4{
            background-color:  #187cb6;
            color:  #000000;
        }#T_9c748_row8_col3,#T_9c748_row8_col4,#T_9c748_row35_col3,#T_9c748_row35_col4{
            background-color:  #bfc9e1;
            color:  #000000;
        }#T_9c748_row9_col3,#T_9c748_row9_col4,#T_9c748_row22_col3,#T_9c748_row22_col4,#T_9c748_row24_col3,#T_9c748_row24_col4,#T_9c748_row26_col3,#T_9c748_row26_col4,#T_9c748_row34_col3,#T_9c748_row34_col4{
            background-color:  #80aed2;
            color:  #000000;
        }#T_9c748_row10_col3,#T_9c748_row10_col4{
            background-color:  #d3d4e7;
            color:  #000000;
        }#T_9c748_row11_col3,#T_9c748_row11_col4{
            background-color:  #0872b1;
            color:  #f1f1f1;
        }#T_9c748_row12_col3,#T_9c748_row12_col4{
            background-color:  #2987bc;
            color:  #000000;
        }#T_9c748_row13_col3,#T_9c748_row13_col4{
            background-color:  #9ab8d8;
            color:  #000000;
        }#T_9c748_row14_col3,#T_9c748_row14_col4,#T_9c748_row20_col3,#T_9c748_row20_col4{
            background-color:  #83afd3;
            color:  #000000;
        }#T_9c748_row15_col3,#T_9c748_row15_col4{
            background-color:  #9ebad9;
            color:  #000000;
        }#T_9c748_row16_col3,#T_9c748_row16_col4{
            background-color:  #a8bedc;
            color:  #000000;
        }#T_9c748_row17_col3,#T_9c748_row17_col4{
            background-color:  #94b6d7;
            color:  #000000;
        }#T_9c748_row18_col2,#T_9c748_row21_col2,#T_9c748_row23_col2{
            background-color:  #c4cbe3;
            color:  #000000;
        }#T_9c748_row18_col3,#T_9c748_row18_col4{
            background-color:  #2f8bbe;
            color:  #000000;
        }#T_9c748_row19_col3,#T_9c748_row19_col4{
            background-color:  #6ba5cd;
            color:  #000000;
        }#T_9c748_row21_col3,#T_9c748_row21_col4{
            background-color:  #045687;
            color:  #f1f1f1;
        }#T_9c748_row23_col3,#T_9c748_row23_col4{
            background-color:  #056ba7;
            color:  #f1f1f1;
        }#T_9c748_row25_col3,#T_9c748_row25_col4{
            background-color:  #adc1dd;
            color:  #000000;
        }#T_9c748_row27_col3,#T_9c748_row27_col4{
            background-color:  #05659f;
            color:  #f1f1f1;
        }#T_9c748_row29_col3,#T_9c748_row29_col4{
            background-color:  #73a9cf;
            color:  #000000;
        }#T_9c748_row30_col3,#T_9c748_row30_col4{
            background-color:  #034973;
            color:  #f1f1f1;
        }#T_9c748_row31_col3,#T_9c748_row31_col4{
            background-color:  #99b8d8;
            color:  #000000;
        }#T_9c748_row32_col3,#T_9c748_row32_col4{
            background-color:  #dad9ea;
            color:  #000000;
        }#T_9c748_row33_col3,#T_9c748_row33_col4{
            background-color:  #4697c4;
            color:  #000000;
        }</style><table id="T_9c748_" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >race_name</th>        <th class="col_heading level0 col1" >driver_name</th>        <th class="col_heading level0 col2" >driver_freq</th>        <th class="col_heading level0 col3" >time_milli</th>        <th class="col_heading level0 col4" >time_delta</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_9c748_level0_row0" class="row_heading level0 row0" >0</th>
                        <td id="T_9c748_row0_col0" class="data row0 col0" >70th Anniversary Grand Prix</td>
                        <td id="T_9c748_row0_col1" class="data row0 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row0_col2" class="data row0 col2" >8</td>
                        <td id="T_9c748_row0_col3" class="data row0 col3" >88451</td>
                        <td id="T_9c748_row0_col4" class="data row0 col4" >0 days 00:01:28.451000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_9c748_row1_col0" class="data row1 col0" >Abu Dhabi Grand Prix</td>
                        <td id="T_9c748_row1_col1" class="data row1 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row1_col2" class="data row1 col2" >8</td>
                        <td id="T_9c748_row1_col3" class="data row1 col3" >99283</td>
                        <td id="T_9c748_row1_col4" class="data row1 col4" >0 days 00:01:39.283000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_9c748_row2_col0" class="data row2 col0" >Argentine Grand Prix</td>
                        <td id="T_9c748_row2_col1" class="data row2 col1" >Gerhard Berger</td>
                        <td id="T_9c748_row2_col2" class="data row2 col2" >1</td>
                        <td id="T_9c748_row2_col3" class="data row2 col3" >87981</td>
                        <td id="T_9c748_row2_col4" class="data row2 col4" >0 days 00:01:27.981000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row3" class="row_heading level0 row3" >3</th>
                        <td id="T_9c748_row3_col0" class="data row3 col0" >Australian Grand Prix</td>
                        <td id="T_9c748_row3_col1" class="data row3 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row3_col2" class="data row3 col2" >6</td>
                        <td id="T_9c748_row3_col3" class="data row3 col3" >84125</td>
                        <td id="T_9c748_row3_col4" class="data row3 col4" >0 days 00:01:24.125000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row4" class="row_heading level0 row4" >4</th>
                        <td id="T_9c748_row4_col0" class="data row4 col0" >Austrian Grand Prix</td>
                        <td id="T_9c748_row4_col1" class="data row4 col1" >Max Verstappen</td>
                        <td id="T_9c748_row4_col2" class="data row4 col2" >2</td>
                        <td id="T_9c748_row4_col3" class="data row4 col3" >66200</td>
                        <td id="T_9c748_row4_col4" class="data row4 col4" >0 days 00:01:06.200000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row5" class="row_heading level0 row5" >5</th>
                        <td id="T_9c748_row5_col0" class="data row5 col0" >Azerbaijan Grand Prix</td>
                        <td id="T_9c748_row5_col1" class="data row5 col1" >Charles Leclerc</td>
                        <td id="T_9c748_row5_col2" class="data row5 col2" >1</td>
                        <td id="T_9c748_row5_col3" class="data row5 col3" >103009</td>
                        <td id="T_9c748_row5_col4" class="data row5 col4" >0 days 00:01:43.009000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row6" class="row_heading level0 row6" >6</th>
                        <td id="T_9c748_row6_col0" class="data row6 col0" >Bahrain Grand Prix</td>
                        <td id="T_9c748_row6_col1" class="data row6 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row6_col2" class="data row6 col2" >6</td>
                        <td id="T_9c748_row6_col3" class="data row6 col3" >90252</td>
                        <td id="T_9c748_row6_col4" class="data row6 col4" >0 days 00:01:30.252000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row7" class="row_heading level0 row7" >7</th>
                        <td id="T_9c748_row7_col0" class="data row7 col0" >Belgian Grand Prix</td>
                        <td id="T_9c748_row7_col1" class="data row7 col1" >Kimi Räikkönen</td>
                        <td id="T_9c748_row7_col2" class="data row7 col2" >2</td>
                        <td id="T_9c748_row7_col3" class="data row7 col3" >105108</td>
                        <td id="T_9c748_row7_col4" class="data row7 col4" >0 days 00:01:45.108000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row8" class="row_heading level0 row8" >8</th>
                        <td id="T_9c748_row8_col0" class="data row8 col0" >Brazilian Grand Prix</td>
                        <td id="T_9c748_row8_col1" class="data row8 col1" >Valtteri Bottas</td>
                        <td id="T_9c748_row8_col2" class="data row8 col2" >2</td>
                        <td id="T_9c748_row8_col3" class="data row8 col3" >70540</td>
                        <td id="T_9c748_row8_col4" class="data row8 col4" >0 days 00:01:10.540000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row9" class="row_heading level0 row9" >9</th>
                        <td id="T_9c748_row9_col0" class="data row9 col0" >British Grand Prix</td>
                        <td id="T_9c748_row9_col1" class="data row9 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row9_col2" class="data row9 col2" >6</td>
                        <td id="T_9c748_row9_col3" class="data row9 col3" >78739</td>
                        <td id="T_9c748_row9_col4" class="data row9 col4" >0 days 00:01:18.739000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row10" class="row_heading level0 row10" >10</th>
                        <td id="T_9c748_row10_col0" class="data row10 col0" >Canadian Grand Prix</td>
                        <td id="T_9c748_row10_col1" class="data row10 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row10_col2" class="data row10 col2" >8</td>
                        <td id="T_9c748_row10_col3" class="data row10 col3" >67058</td>
                        <td id="T_9c748_row10_col4" class="data row10 col4" >0 days 00:01:07.058000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row11" class="row_heading level0 row11" >11</th>
                        <td id="T_9c748_row11_col0" class="data row11 col0" >Chinese Grand Prix</td>
                        <td id="T_9c748_row11_col1" class="data row11 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row11_col2" class="data row11 col2" >6</td>
                        <td id="T_9c748_row11_col3" class="data row11 col3" >92238</td>
                        <td id="T_9c748_row11_col4" class="data row11 col4" >0 days 00:01:32.238000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row12" class="row_heading level0 row12" >12</th>
                        <td id="T_9c748_row12_col0" class="data row12 col0" >Eifel Grand Prix</td>
                        <td id="T_9c748_row12_col1" class="data row12 col1" >Max Verstappen</td>
                        <td id="T_9c748_row12_col2" class="data row12 col2" >2</td>
                        <td id="T_9c748_row12_col3" class="data row12 col3" >88139</td>
                        <td id="T_9c748_row12_col4" class="data row12 col4" >0 days 00:01:28.139000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row13" class="row_heading level0 row13" >13</th>
                        <td id="T_9c748_row13_col0" class="data row13 col0" >Emilia Romagna Grand Prix</td>
                        <td id="T_9c748_row13_col1" class="data row13 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row13_col2" class="data row13 col2" >8</td>
                        <td id="T_9c748_row13_col3" class="data row13 col3" >75484</td>
                        <td id="T_9c748_row13_col4" class="data row13 col4" >0 days 00:01:15.484000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row14" class="row_heading level0 row14" >14</th>
                        <td id="T_9c748_row14_col0" class="data row14 col0" >European Grand Prix</td>
                        <td id="T_9c748_row14_col1" class="data row14 col1" >Juan Pablo Montoya</td>
                        <td id="T_9c748_row14_col2" class="data row14 col2" >2</td>
                        <td id="T_9c748_row14_col3" class="data row14 col3" >78354</td>
                        <td id="T_9c748_row14_col4" class="data row14 col4" >0 days 00:01:18.354000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row15" class="row_heading level0 row15" >15</th>
                        <td id="T_9c748_row15_col0" class="data row15 col0" >French Grand Prix</td>
                        <td id="T_9c748_row15_col1" class="data row15 col1" >David Coulthard</td>
                        <td id="T_9c748_row15_col2" class="data row15 col2" >1</td>
                        <td id="T_9c748_row15_col3" class="data row15 col3" >75045</td>
                        <td id="T_9c748_row15_col4" class="data row15 col4" >0 days 00:01:15.045000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row16" class="row_heading level0 row16" >16</th>
                        <td id="T_9c748_row16_col0" class="data row16 col0" >German Grand Prix</td>
                        <td id="T_9c748_row16_col1" class="data row16 col1" >Kimi Räikkönen</td>
                        <td id="T_9c748_row16_col2" class="data row16 col2" >2</td>
                        <td id="T_9c748_row16_col3" class="data row16 col3" >73780</td>
                        <td id="T_9c748_row16_col4" class="data row16 col4" >0 days 00:01:13.780000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row17" class="row_heading level0 row17" >17</th>
                        <td id="T_9c748_row17_col0" class="data row17 col0" >Hungarian Grand Prix</td>
                        <td id="T_9c748_row17_col1" class="data row17 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row17_col2" class="data row17 col2" >6</td>
                        <td id="T_9c748_row17_col3" class="data row17 col3" >76207</td>
                        <td id="T_9c748_row17_col4" class="data row17 col4" >0 days 00:01:16.207000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row18" class="row_heading level0 row18" >18</th>
                        <td id="T_9c748_row18_col0" class="data row18 col0" >Indian Grand Prix</td>
                        <td id="T_9c748_row18_col1" class="data row18 col1" >Sebastian Vettel</td>
                        <td id="T_9c748_row18_col2" class="data row18 col2" >3</td>
                        <td id="T_9c748_row18_col3" class="data row18 col3" >87249</td>
                        <td id="T_9c748_row18_col4" class="data row18 col4" >0 days 00:01:27.249000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row19" class="row_heading level0 row19" >19</th>
                        <td id="T_9c748_row19_col0" class="data row19 col0" >Italian Grand Prix</td>
                        <td id="T_9c748_row19_col1" class="data row19 col1" >Rubens Barrichello</td>
                        <td id="T_9c748_row19_col2" class="data row19 col2" >2</td>
                        <td id="T_9c748_row19_col3" class="data row19 col3" >81046</td>
                        <td id="T_9c748_row19_col4" class="data row19 col4" >0 days 00:01:21.046000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row20" class="row_heading level0 row20" >20</th>
                        <td id="T_9c748_row20_col0" class="data row20 col0" >Japanese Grand Prix</td>
                        <td id="T_9c748_row20_col1" class="data row20 col1" >Felipe Massa</td>
                        <td id="T_9c748_row20_col2" class="data row20 col2" >1</td>
                        <td id="T_9c748_row20_col3" class="data row20 col3" >78426</td>
                        <td id="T_9c748_row20_col4" class="data row20 col4" >0 days 00:01:18.426000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row21" class="row_heading level0 row21" >21</th>
                        <td id="T_9c748_row21_col0" class="data row21 col0" >Korean Grand Prix</td>
                        <td id="T_9c748_row21_col1" class="data row21 col1" >Sebastian Vettel</td>
                        <td id="T_9c748_row21_col2" class="data row21 col2" >3</td>
                        <td id="T_9c748_row21_col3" class="data row21 col3" >99605</td>
                        <td id="T_9c748_row21_col4" class="data row21 col4" >0 days 00:01:39.605000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row22" class="row_heading level0 row22" >22</th>
                        <td id="T_9c748_row22_col0" class="data row22 col0" >Luxembourg Grand Prix</td>
                        <td id="T_9c748_row22_col1" class="data row22 col1" >Heinz-Harald Frentzen</td>
                        <td id="T_9c748_row22_col2" class="data row22 col2" >1</td>
                        <td id="T_9c748_row22_col3" class="data row22 col3" >78805</td>
                        <td id="T_9c748_row22_col4" class="data row22 col4" >0 days 00:01:18.805000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row23" class="row_heading level0 row23" >23</th>
                        <td id="T_9c748_row23_col0" class="data row23 col0" >Malaysian Grand Prix</td>
                        <td id="T_9c748_row23_col1" class="data row23 col1" >Sebastian Vettel</td>
                        <td id="T_9c748_row23_col2" class="data row23 col2" >3</td>
                        <td id="T_9c748_row23_col3" class="data row23 col3" >94080</td>
                        <td id="T_9c748_row23_col4" class="data row23 col4" >0 days 00:01:34.080000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row24" class="row_heading level0 row24" >24</th>
                        <td id="T_9c748_row24_col0" class="data row24 col0" >Mexican Grand Prix</td>
                        <td id="T_9c748_row24_col1" class="data row24 col1" >Valtteri Bottas</td>
                        <td id="T_9c748_row24_col2" class="data row24 col2" >2</td>
                        <td id="T_9c748_row24_col3" class="data row24 col3" >78741</td>
                        <td id="T_9c748_row24_col4" class="data row24 col4" >0 days 00:01:18.741000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row25" class="row_heading level0 row25" >25</th>
                        <td id="T_9c748_row25_col0" class="data row25 col0" >Monaco Grand Prix</td>
                        <td id="T_9c748_row25_col1" class="data row25 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row25_col2" class="data row25 col2" >8</td>
                        <td id="T_9c748_row25_col3" class="data row25 col3" >72909</td>
                        <td id="T_9c748_row25_col4" class="data row25 col4" >0 days 00:01:12.909000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row26" class="row_heading level0 row26" >26</th>
                        <td id="T_9c748_row26_col0" class="data row26 col0" >Portuguese Grand Prix</td>
                        <td id="T_9c748_row26_col1" class="data row26 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row26_col2" class="data row26 col2" >8</td>
                        <td id="T_9c748_row26_col3" class="data row26 col3" >78750</td>
                        <td id="T_9c748_row26_col4" class="data row26 col4" >0 days 00:01:18.750000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row27" class="row_heading level0 row27" >27</th>
                        <td id="T_9c748_row27_col0" class="data row27 col0" >Russian Grand Prix</td>
                        <td id="T_9c748_row27_col1" class="data row27 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row27_col2" class="data row27 col2" >8</td>
                        <td id="T_9c748_row27_col3" class="data row27 col3" >95761</td>
                        <td id="T_9c748_row27_col4" class="data row27 col4" >0 days 00:01:35.761000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row28" class="row_heading level0 row28" >28</th>
                        <td id="T_9c748_row28_col0" class="data row28 col0" >Sakhir Grand Prix</td>
                        <td id="T_9c748_row28_col1" class="data row28 col1" >George Russell</td>
                        <td id="T_9c748_row28_col2" class="data row28 col2" >1</td>
                        <td id="T_9c748_row28_col3" class="data row28 col3" >55404</td>
                        <td id="T_9c748_row28_col4" class="data row28 col4" >0 days 00:00:55.404000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row29" class="row_heading level0 row29" >29</th>
                        <td id="T_9c748_row29_col0" class="data row29 col0" >San Marino Grand Prix</td>
                        <td id="T_9c748_row29_col1" class="data row29 col1" >Michael Schumacher</td>
                        <td id="T_9c748_row29_col2" class="data row29 col2" >6</td>
                        <td id="T_9c748_row29_col3" class="data row29 col3" >80411</td>
                        <td id="T_9c748_row29_col4" class="data row29 col4" >0 days 00:01:20.411000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row30" class="row_heading level0 row30" >30</th>
                        <td id="T_9c748_row30_col0" class="data row30 col0" >Singapore Grand Prix</td>
                        <td id="T_9c748_row30_col1" class="data row30 col1" >Kevin Magnussen</td>
                        <td id="T_9c748_row30_col2" class="data row30 col2" >1</td>
                        <td id="T_9c748_row30_col3" class="data row30 col3" >101905</td>
                        <td id="T_9c748_row30_col4" class="data row30 col4" >0 days 00:01:41.905000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row31" class="row_heading level0 row31" >31</th>
                        <td id="T_9c748_row31_col0" class="data row31 col0" >Spanish Grand Prix</td>
                        <td id="T_9c748_row31_col1" class="data row31 col1" >Giancarlo Fisichella</td>
                        <td id="T_9c748_row31_col2" class="data row31 col2" >1</td>
                        <td id="T_9c748_row31_col3" class="data row31 col3" >75641</td>
                        <td id="T_9c748_row31_col4" class="data row31 col4" >0 days 00:01:15.641000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row32" class="row_heading level0 row32" >32</th>
                        <td id="T_9c748_row32_col0" class="data row32 col0" >Styrian Grand Prix</td>
                        <td id="T_9c748_row32_col1" class="data row32 col1" >Carlos Sainz</td>
                        <td id="T_9c748_row32_col2" class="data row32 col2" >1</td>
                        <td id="T_9c748_row32_col3" class="data row32 col3" >65619</td>
                        <td id="T_9c748_row32_col4" class="data row32 col4" >0 days 00:01:05.619000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row33" class="row_heading level0 row33" >33</th>
                        <td id="T_9c748_row33_col0" class="data row33 col0" >Turkish Grand Prix</td>
                        <td id="T_9c748_row33_col1" class="data row33 col1" >Juan Pablo Montoya</td>
                        <td id="T_9c748_row33_col2" class="data row33 col2" >2</td>
                        <td id="T_9c748_row33_col3" class="data row33 col3" >84770</td>
                        <td id="T_9c748_row33_col4" class="data row33 col4" >0 days 00:01:24.770000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row34" class="row_heading level0 row34" >34</th>
                        <td id="T_9c748_row34_col0" class="data row34 col0" >Tuscan Grand Prix</td>
                        <td id="T_9c748_row34_col1" class="data row34 col1" >Lewis Hamilton</td>
                        <td id="T_9c748_row34_col2" class="data row34 col2" >8</td>
                        <td id="T_9c748_row34_col3" class="data row34 col3" >78833</td>
                        <td id="T_9c748_row34_col4" class="data row34 col4" >0 days 00:01:18.833000</td>
            </tr>
            <tr>
                        <th id="T_9c748_level0_row35" class="row_heading level0 row35" >35</th>
                        <td id="T_9c748_row35_col0" class="data row35 col0" >United States Grand Prix</td>
                        <td id="T_9c748_row35_col1" class="data row35 col1" >Rubens Barrichello</td>
                        <td id="T_9c748_row35_col2" class="data row35 col2" >2</td>
                        <td id="T_9c748_row35_col3" class="data row35 col3" >70399</td>
                        <td id="T_9c748_row35_col4" class="data row35 col4" >0 days 00:01:10.399000</td>
            </tr>
    </tbody></table>



**Description:** There are a total of 35 F1 tracks throughout history. We can also see that most of the fastest lap time is done by Lewis Hamilton where the fastest lap ever done by him is is the Canadian Grand Prix with a time of 1 minute and 07.058000 seconds.

One more thing that intrigue me is that despite being a driver to one of the slower car for the season, we can see that George Russell did the fastest lap in Sakhir GP. From [Wikipedia](https://en.wikipedia.org/wiki/2020_Sakhir_Grand_Prix), we can see that at that time George Russell is temporarily driving for Mercedes in Lewis Hamilton's car as a replacement driver due to Lewis having tested positive for Covid-19.

<a id='season_race_count'></a>
### 3. How many F1 races are there for each season?


```sql
%%sql org_race <<

SELECT year,COUNT(race_name) AS count
FROM
    races AS rc
    
    INNER JOIN circuits AS c
    ON rc.circuitId = c.circuitId

GROUP BY year
ORDER BY year
```

     * mssql+pyodbc://@DESKTOP-8RI327A\SQLEXPRESS/F1?driver=SQL+Server
    Done.
    Returning data to local variable org_race
    


```python
df_org_race = org_race.DataFrame()
```


```python
df_org_race['year'] = df_org_race['year'].astype(str)
```


```python
df_org_race.head()
```




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
      <th>year</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1950</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1951</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1952</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1953</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1954</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize = b_figsize)
plt.title('Number F1 races organized for each season')
sns.lineplot(data=df_org_race, x='year', y='count',color=palette[1])
plt.xlabel('Season')
plt.ylabel('Count of races')
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_88_0.png)
    


**Description:** Here we can see that the 2021 season had the most races which is 23. Initally in 1950, there were less than 10 races held. One key feature is that there is a consistent amount of races between 1984 and 1994 which is 16 races. It seems that the 16-race calendar was favoured by the FIA during that period. 

From the previous analysis of the driver points, we can see that the drivers that obtained the most overall points are the drivers that are from the later season of F1 (from 2010 and onwards). This somehow makes sense since during the later seasons, there are more races per season in comparison to the earlier seasons. This means that the drivers are able to obtain more points since there are more races.
