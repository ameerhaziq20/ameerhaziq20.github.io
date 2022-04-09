---
name: Death cause Analysis Malaysia
tools: [Python, Pandas, Squarify, Seaborn, Matplotlib]
image: https://i.pinimg.com/736x/e9/c2/96/e9c296fc18499a211e82fa48f4b76439.jpg
description: Data wrangling practice project using Python and Pandas.
---

![jpg](https://thumbs.dreamstime.com/b/hospital-room-interior-intensive-therapy-patient-ward-medical-tools-row-nursing-care-bed-empty-no-people-modern-clinic-139834092.jpg)

This project is done to practice data wrangling (or data munging). The truth is that data cannot always be used as it is and needs preparation in a way so that it can be used. Some cleaning and wrangling needs to be done before we can do analysis on them. In this project, we will make use of the the Death Statistics 2019 data obtained from [Department of Statistics Malaysia](https://newss.statistics.gov.my/newss-portalx/ep/epFreeDownloadContentSearch.seam?contentId=148840&actionMethod=ep%2FepFreeDownloadContentSearch.xhtml%3AcontentAction.doDisplayContent&cid=162492). Also included is a simple visualization to give some insights about the data. All of these process are made using Python.

# 1. Questions

This analysis will try to answer the following questions:
* Which gender live longer, men or women?
* What is the leasing cause of death in Malaysia?
* Which state has the highest death and death rate?

# 2. Measurement Priorities

* Count of deaths of each gender by each age group. 
* The death cause with the most count of casualty.
* The count of death cases in each state in Malaysia.

# 3. Data Collection

* Source
    * The data is obtained from the Department of Statistics Malaysia Website.

* Storage
    * The cleaned data will be stored in a CSV file.

##### [Github link for project](https://github.com/ameerhaziq20/Malaysia-s-Death-Cause-Analysis-2019)
---
Data is obtained from:

[JADUAL PENERBITAN PERANGKAAN SEBAB KEMATIAN, MALAYSIA, 2020](https://newss.statistics.gov.my/newss-portalx/ep/epFreeDownloadContentSearch.seam?contentId=148840&actionMethod=ep%2FepFreeDownloadContentSearch.xhtml%3AcontentAction.doDisplayContent&cid=162492)

Statistics on Causes of Death, Malaysia, 2020 presents statistics on major causes of death by classification for 2019. The Department of Statistics Malaysia (DOSM) uses the International Statistical Classification of Diseases and Related Health Problems, 10th Revision (ICD-10) published by the World Health Organization (2010 ) as reference material for coding the cause of death.

# Import Data


```python
import pandas as pd
xls = pd.ExcelFile('dataset\death_cause_malaysia_2020.xlsx')
```

# Data Cleaning 

## Table 1.1: Deaths (number and rate) by state and sex, Malaysia, 2019

### Preview table


```python
df_gen = pd.read_excel(xls,sheet_name="1.1")
```


```python
df_gen
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
      <th>Jadual 1.1: Kematian (bilangan dan kadar) mengikut negeri dan jantina, Malaysia, 2019</th>
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Table 1.1: Deaths (number and rate) by state a...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Kematian</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Kadar kematian kasar</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Deaths</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Crude death rate</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Jumlah</td>
      <td>Lelaki</td>
      <td>Perempuan</td>
      <td>NaN</td>
      <td>Jumlah</td>
      <td>Lelaki</td>
      <td>Perempuan</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>Total</td>
      <td>Male</td>
      <td>Female</td>
      <td>NaN</td>
      <td>Total</td>
      <td>Male</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>MALAYSIA</td>
      <td>NaN</td>
      <td>173746</td>
      <td>99681</td>
      <td>74065</td>
      <td>NaN</td>
      <td>5.342242</td>
      <td>5.945819</td>
      <td>4.700105</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>Johor</td>
      <td>21776</td>
      <td>12743</td>
      <td>9033</td>
      <td>NaN</td>
      <td>5.789692</td>
      <td>6.453459</td>
      <td>5.056066</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>Kedah</td>
      <td>14292</td>
      <td>8027</td>
      <td>6265</td>
      <td>NaN</td>
      <td>6.574828</td>
      <td>7.287441</td>
      <td>5.842794</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>Kelantan</td>
      <td>11389</td>
      <td>6436</td>
      <td>4953</td>
      <td>NaN</td>
      <td>6.045617</td>
      <td>6.759489</td>
      <td>5.316083</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NaN</td>
      <td>Melaka</td>
      <td>5542</td>
      <td>3084</td>
      <td>2458</td>
      <td>NaN</td>
      <td>5.96941</td>
      <td>6.619759</td>
      <td>5.314342</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>NaN</td>
      <td>Negeri Sembilan</td>
      <td>7157</td>
      <td>4097</td>
      <td>3060</td>
      <td>NaN</td>
      <td>6.355022</td>
      <td>7.056639</td>
      <td>5.608422</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>NaN</td>
      <td>Pahang</td>
      <td>9165</td>
      <td>5381</td>
      <td>3784</td>
      <td>NaN</td>
      <td>5.483302</td>
      <td>6.067542</td>
      <td>4.822913</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>NaN</td>
      <td>Perak</td>
      <td>18712</td>
      <td>10554</td>
      <td>8158</td>
      <td>NaN</td>
      <td>7.458543</td>
      <td>8.322346</td>
      <td>6.575591</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>NaN</td>
      <td>Perlis</td>
      <td>1994</td>
      <td>1124</td>
      <td>870</td>
      <td>NaN</td>
      <td>7.84891</td>
      <td>8.886218</td>
      <td>6.82032</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>NaN</td>
      <td>Pulau Pinang</td>
      <td>10879</td>
      <td>6101</td>
      <td>4778</td>
      <td>NaN</td>
      <td>6.150355</td>
      <td>6.862011</td>
      <td>5.431132</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NaN</td>
      <td>Sabah</td>
      <td>14493</td>
      <td>8439</td>
      <td>6054</td>
      <td>NaN</td>
      <td>3.711919</td>
      <td>4.142434</td>
      <td>3.242217</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>Sarawak</td>
      <td>13990</td>
      <td>7888</td>
      <td>6102</td>
      <td>NaN</td>
      <td>4.985706</td>
      <td>5.401526</td>
      <td>4.534463</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>NaN</td>
      <td>Selangor</td>
      <td>27700</td>
      <td>16142</td>
      <td>11558</td>
      <td>NaN</td>
      <td>4.257525</td>
      <td>4.782697</td>
      <td>3.69142</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>NaN</td>
      <td>Terengganu</td>
      <td>7334</td>
      <td>4201</td>
      <td>3133</td>
      <td>NaN</td>
      <td>5.893177</td>
      <td>6.634565</td>
      <td>5.125219</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NaN</td>
      <td>W.P. Kuala Lumpur</td>
      <td>8819</td>
      <td>5168</td>
      <td>3651</td>
      <td>NaN</td>
      <td>4.94744</td>
      <td>5.646853</td>
      <td>4.209432</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>NaN</td>
      <td>W.P. Labuan</td>
      <td>296</td>
      <td>182</td>
      <td>114</td>
      <td>NaN</td>
      <td>2.981166</td>
      <td>3.637308</td>
      <td>2.31458</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>NaN</td>
      <td>W.P. Putrajaya</td>
      <td>208</td>
      <td>114</td>
      <td>94</td>
      <td>NaN</td>
      <td>2.006715</td>
      <td>2.347853</td>
      <td>1.706082</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Kadar adalah bagi setiap 1,000 penduduk</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>The rates are per 1,000 population</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Remove unwanted rows and columns


```python
df_gen = df_gen.iloc[8: , :]
df_gen = df_gen.iloc[:-4]
df_gen = df_gen.iloc[:,1:]
df_gen = df_gen.iloc[:,:-2]
```


```python
df_gen.drop('Unnamed: 5',axis=1,inplace=True)
```


```python
df_gen
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
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>Johor</td>
      <td>21776</td>
      <td>12743</td>
      <td>9033</td>
      <td>5.789692</td>
      <td>6.453459</td>
      <td>5.056066</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kedah</td>
      <td>14292</td>
      <td>8027</td>
      <td>6265</td>
      <td>6.574828</td>
      <td>7.287441</td>
      <td>5.842794</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Kelantan</td>
      <td>11389</td>
      <td>6436</td>
      <td>4953</td>
      <td>6.045617</td>
      <td>6.759489</td>
      <td>5.316083</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Melaka</td>
      <td>5542</td>
      <td>3084</td>
      <td>2458</td>
      <td>5.96941</td>
      <td>6.619759</td>
      <td>5.314342</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Negeri Sembilan</td>
      <td>7157</td>
      <td>4097</td>
      <td>3060</td>
      <td>6.355022</td>
      <td>7.056639</td>
      <td>5.608422</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Pahang</td>
      <td>9165</td>
      <td>5381</td>
      <td>3784</td>
      <td>5.483302</td>
      <td>6.067542</td>
      <td>4.822913</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Perak</td>
      <td>18712</td>
      <td>10554</td>
      <td>8158</td>
      <td>7.458543</td>
      <td>8.322346</td>
      <td>6.575591</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Perlis</td>
      <td>1994</td>
      <td>1124</td>
      <td>870</td>
      <td>7.84891</td>
      <td>8.886218</td>
      <td>6.82032</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Pulau Pinang</td>
      <td>10879</td>
      <td>6101</td>
      <td>4778</td>
      <td>6.150355</td>
      <td>6.862011</td>
      <td>5.431132</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Sabah</td>
      <td>14493</td>
      <td>8439</td>
      <td>6054</td>
      <td>3.711919</td>
      <td>4.142434</td>
      <td>3.242217</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Sarawak</td>
      <td>13990</td>
      <td>7888</td>
      <td>6102</td>
      <td>4.985706</td>
      <td>5.401526</td>
      <td>4.534463</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Selangor</td>
      <td>27700</td>
      <td>16142</td>
      <td>11558</td>
      <td>4.257525</td>
      <td>4.782697</td>
      <td>3.69142</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Terengganu</td>
      <td>7334</td>
      <td>4201</td>
      <td>3133</td>
      <td>5.893177</td>
      <td>6.634565</td>
      <td>5.125219</td>
    </tr>
    <tr>
      <th>21</th>
      <td>W.P. Kuala Lumpur</td>
      <td>8819</td>
      <td>5168</td>
      <td>3651</td>
      <td>4.94744</td>
      <td>5.646853</td>
      <td>4.209432</td>
    </tr>
    <tr>
      <th>22</th>
      <td>W.P. Labuan</td>
      <td>296</td>
      <td>182</td>
      <td>114</td>
      <td>2.981166</td>
      <td>3.637308</td>
      <td>2.31458</td>
    </tr>
    <tr>
      <th>23</th>
      <td>W.P. Putrajaya</td>
      <td>208</td>
      <td>114</td>
      <td>94</td>
      <td>2.006715</td>
      <td>2.347853</td>
      <td>1.706082</td>
    </tr>
  </tbody>
</table>
</div>



### Rename Columns


```python
dict = {'Unnamed: 1': 'state_death',
        'Unnamed: 2': 'total_death',
        'Unnamed: 3': 'male_death',
        'Unnamed: 4': 'female_death',
        'Unnamed: 6': 'total_death_rate',
        'Unnamed: 7': 'male_death_rate',
        'Unnamed: 8': 'female_death_rate',

       }

df_gen.rename(columns=dict,
          inplace=True)
```

### Reset Index


```python
df_gen.reset_index(drop=True,inplace=True)
```


```python
df_gen
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
      <th>state_death</th>
      <th>total_death</th>
      <th>male_death</th>
      <th>female_death</th>
      <th>total_death_rate</th>
      <th>male_death_rate</th>
      <th>female_death_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Johor</td>
      <td>21776</td>
      <td>12743</td>
      <td>9033</td>
      <td>5.789692</td>
      <td>6.453459</td>
      <td>5.056066</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kedah</td>
      <td>14292</td>
      <td>8027</td>
      <td>6265</td>
      <td>6.574828</td>
      <td>7.287441</td>
      <td>5.842794</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelantan</td>
      <td>11389</td>
      <td>6436</td>
      <td>4953</td>
      <td>6.045617</td>
      <td>6.759489</td>
      <td>5.316083</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Melaka</td>
      <td>5542</td>
      <td>3084</td>
      <td>2458</td>
      <td>5.96941</td>
      <td>6.619759</td>
      <td>5.314342</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Negeri Sembilan</td>
      <td>7157</td>
      <td>4097</td>
      <td>3060</td>
      <td>6.355022</td>
      <td>7.056639</td>
      <td>5.608422</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Pahang</td>
      <td>9165</td>
      <td>5381</td>
      <td>3784</td>
      <td>5.483302</td>
      <td>6.067542</td>
      <td>4.822913</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Perak</td>
      <td>18712</td>
      <td>10554</td>
      <td>8158</td>
      <td>7.458543</td>
      <td>8.322346</td>
      <td>6.575591</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Perlis</td>
      <td>1994</td>
      <td>1124</td>
      <td>870</td>
      <td>7.84891</td>
      <td>8.886218</td>
      <td>6.82032</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Pulau Pinang</td>
      <td>10879</td>
      <td>6101</td>
      <td>4778</td>
      <td>6.150355</td>
      <td>6.862011</td>
      <td>5.431132</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sabah</td>
      <td>14493</td>
      <td>8439</td>
      <td>6054</td>
      <td>3.711919</td>
      <td>4.142434</td>
      <td>3.242217</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Sarawak</td>
      <td>13990</td>
      <td>7888</td>
      <td>6102</td>
      <td>4.985706</td>
      <td>5.401526</td>
      <td>4.534463</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Selangor</td>
      <td>27700</td>
      <td>16142</td>
      <td>11558</td>
      <td>4.257525</td>
      <td>4.782697</td>
      <td>3.69142</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Terengganu</td>
      <td>7334</td>
      <td>4201</td>
      <td>3133</td>
      <td>5.893177</td>
      <td>6.634565</td>
      <td>5.125219</td>
    </tr>
    <tr>
      <th>13</th>
      <td>W.P. Kuala Lumpur</td>
      <td>8819</td>
      <td>5168</td>
      <td>3651</td>
      <td>4.94744</td>
      <td>5.646853</td>
      <td>4.209432</td>
    </tr>
    <tr>
      <th>14</th>
      <td>W.P. Labuan</td>
      <td>296</td>
      <td>182</td>
      <td>114</td>
      <td>2.981166</td>
      <td>3.637308</td>
      <td>2.31458</td>
    </tr>
    <tr>
      <th>15</th>
      <td>W.P. Putrajaya</td>
      <td>208</td>
      <td>114</td>
      <td>94</td>
      <td>2.006715</td>
      <td>2.347853</td>
      <td>1.706082</td>
    </tr>
  </tbody>
</table>
</div>



### Convert to appropriate data types


```python
cols = df_gen.columns.drop('state_death')
df_gen[cols] = df_gen[cols].apply(pd.to_numeric, errors='coerce')
```


```python
df_gen.dtypes
```




    state_death           object
    total_death            int64
    male_death             int64
    female_death           int64
    total_death_rate     float64
    male_death_rate      float64
    female_death_rate    float64
    dtype: object



### Export cleaned table


```python
df_gen
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
      <th>state_death</th>
      <th>total_death</th>
      <th>male_death</th>
      <th>female_death</th>
      <th>total_death_rate</th>
      <th>male_death_rate</th>
      <th>female_death_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Johor</td>
      <td>21776</td>
      <td>12743</td>
      <td>9033</td>
      <td>5.789692</td>
      <td>6.453459</td>
      <td>5.056066</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kedah</td>
      <td>14292</td>
      <td>8027</td>
      <td>6265</td>
      <td>6.574828</td>
      <td>7.287441</td>
      <td>5.842794</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelantan</td>
      <td>11389</td>
      <td>6436</td>
      <td>4953</td>
      <td>6.045617</td>
      <td>6.759489</td>
      <td>5.316083</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Melaka</td>
      <td>5542</td>
      <td>3084</td>
      <td>2458</td>
      <td>5.969410</td>
      <td>6.619759</td>
      <td>5.314342</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Negeri Sembilan</td>
      <td>7157</td>
      <td>4097</td>
      <td>3060</td>
      <td>6.355022</td>
      <td>7.056639</td>
      <td>5.608422</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Pahang</td>
      <td>9165</td>
      <td>5381</td>
      <td>3784</td>
      <td>5.483302</td>
      <td>6.067542</td>
      <td>4.822913</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Perak</td>
      <td>18712</td>
      <td>10554</td>
      <td>8158</td>
      <td>7.458543</td>
      <td>8.322346</td>
      <td>6.575591</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Perlis</td>
      <td>1994</td>
      <td>1124</td>
      <td>870</td>
      <td>7.848910</td>
      <td>8.886218</td>
      <td>6.820320</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Pulau Pinang</td>
      <td>10879</td>
      <td>6101</td>
      <td>4778</td>
      <td>6.150355</td>
      <td>6.862011</td>
      <td>5.431132</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sabah</td>
      <td>14493</td>
      <td>8439</td>
      <td>6054</td>
      <td>3.711919</td>
      <td>4.142434</td>
      <td>3.242217</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Sarawak</td>
      <td>13990</td>
      <td>7888</td>
      <td>6102</td>
      <td>4.985706</td>
      <td>5.401526</td>
      <td>4.534463</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Selangor</td>
      <td>27700</td>
      <td>16142</td>
      <td>11558</td>
      <td>4.257525</td>
      <td>4.782697</td>
      <td>3.691420</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Terengganu</td>
      <td>7334</td>
      <td>4201</td>
      <td>3133</td>
      <td>5.893177</td>
      <td>6.634565</td>
      <td>5.125219</td>
    </tr>
    <tr>
      <th>13</th>
      <td>W.P. Kuala Lumpur</td>
      <td>8819</td>
      <td>5168</td>
      <td>3651</td>
      <td>4.947440</td>
      <td>5.646853</td>
      <td>4.209432</td>
    </tr>
    <tr>
      <th>14</th>
      <td>W.P. Labuan</td>
      <td>296</td>
      <td>182</td>
      <td>114</td>
      <td>2.981166</td>
      <td>3.637308</td>
      <td>2.314580</td>
    </tr>
    <tr>
      <th>15</th>
      <td>W.P. Putrajaya</td>
      <td>208</td>
      <td>114</td>
      <td>94</td>
      <td>2.006715</td>
      <td>2.347853</td>
      <td>1.706082</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_gen.to_csv('cleaned_data/death_rate_by_state_and_sex_2019.csv',index=False)
```

## Table 1.3: Deaths by age group and sex, Malaysia, 2019

### Preview table


```python
df = pd.read_excel(xls,sheet_name="1.3")
```


```python
df
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
      <th>Jadual 1.3: Kematian mengikut kumpulan umur dan jantina, Malaysia, 2019</th>
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
      <th>Unnamed: 11</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Table 1.3: Deaths by age group and sex, Malays...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kumpulan umur</td>
      <td>Jumlah</td>
      <td>Lelaki</td>
      <td>Perempuan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Age group</td>
      <td>Total</td>
      <td>Male</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jumlah/Total</td>
      <td>173746</td>
      <td>99681</td>
      <td>74065</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Kurang 1 tahun/Under 1 year</td>
      <td>3117</td>
      <td>1711</td>
      <td>1406</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kurang 1 hari/Under 1 day</td>
      <td>652</td>
      <td>351</td>
      <td>301</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1 hari hingga &lt; 1 minggu/1 day to &lt; 1 week</td>
      <td>842</td>
      <td>467</td>
      <td>375</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1 minggu hingga &lt; 4 minggu/1 week to &lt; 4 weeks</td>
      <td>486</td>
      <td>267</td>
      <td>219</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>4 minggu hingga &lt; 3 bulan/4 weeks to &lt; 3 months</td>
      <td>459</td>
      <td>248</td>
      <td>211</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3 bulan hingga &lt; 6 bulan/3 months to &lt; 6 months</td>
      <td>349</td>
      <td>195</td>
      <td>154</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>6 bulan hingga &lt; 9 bulan/6 months to &lt; 9 months</td>
      <td>203</td>
      <td>120</td>
      <td>83</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>9 bulan hingga &lt; 12 bulan/9 months to &lt; 12 mon...</td>
      <td>126</td>
      <td>63</td>
      <td>63</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1 tahun/1 year</td>
      <td>167</td>
      <td>87</td>
      <td>80</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2 tahun/2 years</td>
      <td>233</td>
      <td>135</td>
      <td>98</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>3 tahun/3 years</td>
      <td>133</td>
      <td>68</td>
      <td>65</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4 tahun/4 years</td>
      <td>115</td>
      <td>71</td>
      <td>44</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0-4</td>
      <td>3765</td>
      <td>2072</td>
      <td>1693</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>5-9</td>
      <td>596</td>
      <td>352</td>
      <td>244</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>10-14</td>
      <td>656</td>
      <td>384</td>
      <td>272</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>15-19</td>
      <td>1591</td>
      <td>1215</td>
      <td>376</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>20-24</td>
      <td>2136</td>
      <td>1615</td>
      <td>521</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>25-29</td>
      <td>2423</td>
      <td>1718</td>
      <td>705</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>30-34</td>
      <td>3017</td>
      <td>2081</td>
      <td>936</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>35-39</td>
      <td>4144</td>
      <td>2803</td>
      <td>1341</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>40-44</td>
      <td>5591</td>
      <td>3778</td>
      <td>1813</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>45-49</td>
      <td>7436</td>
      <td>4944</td>
      <td>2492</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>50-54</td>
      <td>10717</td>
      <td>6919</td>
      <td>3798</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>55-59</td>
      <td>14572</td>
      <td>9334</td>
      <td>5238</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>32</th>
      <td>60-64</td>
      <td>18046</td>
      <td>11275</td>
      <td>6771</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>33</th>
      <td>65-69</td>
      <td>19539</td>
      <td>11855</td>
      <td>7684</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>34</th>
      <td>70-74</td>
      <td>20351</td>
      <td>11901</td>
      <td>8450</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35</th>
      <td>75-79</td>
      <td>19672</td>
      <td>10737</td>
      <td>8935</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>36</th>
      <td>80-84</td>
      <td>19479</td>
      <td>9201</td>
      <td>10278</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>37</th>
      <td>85 dan lebih/85 and over</td>
      <td>20015</td>
      <td>7497</td>
      <td>12518</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Remove unwanted rows and columns


```python
df = df.iloc[8: , :]
```


```python
df.head()
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
      <th>Jadual 1.3: Kematian mengikut kumpulan umur dan jantina, Malaysia, 2019</th>
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
      <th>Unnamed: 11</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>Kurang 1 tahun/Under 1 year</td>
      <td>3117</td>
      <td>1711</td>
      <td>1406</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kurang 1 hari/Under 1 day</td>
      <td>652</td>
      <td>351</td>
      <td>301</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1 hari hingga &lt; 1 minggu/1 day to &lt; 1 week</td>
      <td>842</td>
      <td>467</td>
      <td>375</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1 minggu hingga &lt; 4 minggu/1 week to &lt; 4 weeks</td>
      <td>486</td>
      <td>267</td>
      <td>219</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>4 minggu hingga &lt; 3 bulan/4 weeks to &lt; 3 months</td>
      <td>459</td>
      <td>248</td>
      <td>211</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df.iloc[:,:-8]
```


```python
dict = {'Jadual 1.3: Kematian mengikut kumpulan umur dan jantina, Malaysia, 2019': 'age_group',
        'Unnamed: 1': 'total_death_group',
        'Unnamed: 2': 'male_death_age',
        'Unnamed: 3': 'female_death_age',
        }

df.rename(columns=dict,
          inplace=True)
```


```python
df.reset_index(drop=True, inplace=True)
```


```python
df.head()
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
      <th>age_group</th>
      <th>total_death_group</th>
      <th>male_death_age</th>
      <th>female_death_age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kurang 1 tahun/Under 1 year</td>
      <td>3117</td>
      <td>1711</td>
      <td>1406</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kurang 1 hari/Under 1 day</td>
      <td>652</td>
      <td>351</td>
      <td>301</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 hari hingga &lt; 1 minggu/1 day to &lt; 1 week</td>
      <td>842</td>
      <td>467</td>
      <td>375</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 minggu hingga &lt; 4 minggu/1 week to &lt; 4 weeks</td>
      <td>486</td>
      <td>267</td>
      <td>219</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4 minggu hingga &lt; 3 bulan/4 weeks to &lt; 3 months</td>
      <td>459</td>
      <td>248</td>
      <td>211</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['age_group']=df.age_group.str.replace(".*/", '')
```

    <ipython-input-219-bda0008f5b22>:1: FutureWarning: The default value of regex will change from True to False in a future version.
      df['age_group']=df.age_group.str.replace(".*/", '')
    


```python
df.head()
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
      <th>age_group</th>
      <th>total_death_group</th>
      <th>male_death_age</th>
      <th>female_death_age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Under 1 year</td>
      <td>3117</td>
      <td>1711</td>
      <td>1406</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Under 1 day</td>
      <td>652</td>
      <td>351</td>
      <td>301</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 day to &lt; 1 week</td>
      <td>842</td>
      <td>467</td>
      <td>375</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 week to &lt; 4 weeks</td>
      <td>486</td>
      <td>267</td>
      <td>219</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4 weeks to &lt; 3 months</td>
      <td>459</td>
      <td>248</td>
      <td>211</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.to_csv('cleaned_data/death_rate_by_age_group_and_sex_2019.csv',index=False)
```

## Table 1.6: Ten principal causes of death (medically certified and non-medically certified) by sex, Malaysia, 2019

### Preview table


```python
df = pd.read_excel(xls,sheet_name=r"1.6(1)")
```


```python
df
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
      <th>Jadual 1.6: Sepuluh sebab kematian utama (disahkan dan tidak disahkan secara perubatan)</th>
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
      <th>Unnamed: 7</th>
      <th>Unnamed: 8</th>
      <th>Unnamed: 9</th>
      <th>Unnamed: 10</th>
      <th>Unnamed: 11</th>
      <th>Unnamed: 12</th>
      <th>Unnamed: 13</th>
      <th>Unnamed: 14</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>mengikut jantina, Malaysia, 2019</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Table 1.6: Ten principal causes of death (medi...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jumlah/Total</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Disahkan secara perubatan</td>
      <td>Bil.</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>Medically certified</td>
      <td>No.</td>
      <td>%</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1.</td>
      <td>Ischaemic heart diseases</td>
      <td>16325</td>
      <td>14.954564</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2.</td>
      <td>Pneumonia</td>
      <td>13356</td>
      <td>12.234803</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3.</td>
      <td>Cerebrovascular diseases</td>
      <td>8691</td>
      <td>7.961416</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>4.</td>
      <td>Transport accidents</td>
      <td>4198</td>
      <td>3.84559</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>5.</td>
      <td>Malignant neoplasm of trachea, bronchus and lung</td>
      <td>2602</td>
      <td>2.38357</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>6.</td>
      <td>Chronic lower respiratory diseases</td>
      <td>2312</td>
      <td>2.117914</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>7.</td>
      <td>Diabetes mellitus</td>
      <td>2008</td>
      <td>1.839434</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>8.</td>
      <td>Malignant neoplasm of colon, rectum and anus</td>
      <td>1873</td>
      <td>1.715767</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>9.</td>
      <td>Malignant neoplasm of breast</td>
      <td>1869</td>
      <td>1.712103</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>10.</td>
      <td>Diseases of the liver</td>
      <td>1601</td>
      <td>1.466601</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NaN</td>
      <td>Keseluruhan sebab                           ...</td>
      <td>109164</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>NaN</td>
      <td>Tidak disahkan secara perubatan</td>
      <td>Bil.</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>NaN</td>
      <td>Non-medically certified</td>
      <td>No.</td>
      <td>%</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1.</td>
      <td>Sakit tua 65 tahun dan lebih                  ...</td>
      <td>30665</td>
      <td>47.482271</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2.</td>
      <td>Darah tinggi                                  ...</td>
      <td>2393</td>
      <td>3.705367</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>3.</td>
      <td>Kencing manis                                 ...</td>
      <td>1997</td>
      <td>3.092193</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>4.</td>
      <td>Penyakit serebrovaskular                      ...</td>
      <td>1708</td>
      <td>2.6447</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>5.</td>
      <td>Lelah                                         ...</td>
      <td>1076</td>
      <td>1.66455</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>27</th>
      <td>6.</td>
      <td>Penyakit jantung iskemia                      ...</td>
      <td>1023</td>
      <td>1.584033</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>7.</td>
      <td>Barah trakea, bronkus dan paru-paru           ...</td>
      <td>742</td>
      <td>1.148927</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>8.</td>
      <td>Barah kolon, rektum dan dubur                 ...</td>
      <td>715</td>
      <td>1.10712</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30</th>
      <td>9.</td>
      <td>Barah hati                                    ...</td>
      <td>533</td>
      <td>0.825307</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>10.</td>
      <td>Barah payu dara                               ...</td>
      <td>524</td>
      <td>0.811372</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NaN</td>
      <td>Keseluruhan sebab                           ...</td>
      <td>64582</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Remove unwanted rows and columns


```python
df = df.iloc[:,1:]
```


```python
df = df.iloc[:,:-11]
```


```python
df = df.iloc[7: , :]
```


```python
df
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
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>Ischaemic heart diseases</td>
      <td>16325</td>
      <td>14.954564</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Pneumonia</td>
      <td>13356</td>
      <td>12.234803</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Cerebrovascular diseases</td>
      <td>8691</td>
      <td>7.961416</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Transport accidents</td>
      <td>4198</td>
      <td>3.84559</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Malignant neoplasm of trachea, bronchus and lung</td>
      <td>2602</td>
      <td>2.38357</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Chronic lower respiratory diseases</td>
      <td>2312</td>
      <td>2.117914</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Diabetes mellitus</td>
      <td>2008</td>
      <td>1.839434</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Malignant neoplasm of colon, rectum and anus</td>
      <td>1873</td>
      <td>1.715767</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Malignant neoplasm of breast</td>
      <td>1869</td>
      <td>1.712103</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Diseases of the liver</td>
      <td>1601</td>
      <td>1.466601</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Keseluruhan sebab                           ...</td>
      <td>109164</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Tidak disahkan secara perubatan</td>
      <td>Bil.</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Non-medically certified</td>
      <td>No.</td>
      <td>%</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Sakit tua 65 tahun dan lebih                  ...</td>
      <td>30665</td>
      <td>47.482271</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Darah tinggi                                  ...</td>
      <td>2393</td>
      <td>3.705367</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Kencing manis                                 ...</td>
      <td>1997</td>
      <td>3.092193</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Penyakit serebrovaskular                      ...</td>
      <td>1708</td>
      <td>2.6447</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Lelah                                         ...</td>
      <td>1076</td>
      <td>1.66455</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Penyakit jantung iskemia                      ...</td>
      <td>1023</td>
      <td>1.584033</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Barah trakea, bronkus dan paru-paru           ...</td>
      <td>742</td>
      <td>1.148927</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Barah kolon, rektum dan dubur                 ...</td>
      <td>715</td>
      <td>1.10712</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Barah hati                                    ...</td>
      <td>533</td>
      <td>0.825307</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Barah payu dara                               ...</td>
      <td>524</td>
      <td>0.811372</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Keseluruhan sebab                           ...</td>
      <td>64582</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Rename Columns


```python
dict = {'Unnamed: 1': 'death_cause',
        'Unnamed: 2': 'death_count',
        'Unnamed: 3': 'percentage',
       }

df.rename(columns=dict,
          inplace=True)
```

### Split tables


```python
df_med = df.iloc[0:10 , :]
df_xmed = df.iloc[15:25 , :]
```


```python
df
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
      <th>death_cause</th>
      <th>death_count</th>
      <th>percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>Ischaemic heart diseases</td>
      <td>16325</td>
      <td>14.954564</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Pneumonia</td>
      <td>13356</td>
      <td>12.234803</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Cerebrovascular diseases</td>
      <td>8691</td>
      <td>7.961416</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Transport accidents</td>
      <td>4198</td>
      <td>3.84559</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Malignant neoplasm of trachea, bronchus and lung</td>
      <td>2602</td>
      <td>2.38357</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Chronic lower respiratory diseases</td>
      <td>2312</td>
      <td>2.117914</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Diabetes mellitus</td>
      <td>2008</td>
      <td>1.839434</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Malignant neoplasm of colon, rectum and anus</td>
      <td>1873</td>
      <td>1.715767</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Malignant neoplasm of breast</td>
      <td>1869</td>
      <td>1.712103</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Diseases of the liver</td>
      <td>1601</td>
      <td>1.466601</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Keseluruhan sebab                           ...</td>
      <td>109164</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Tidak disahkan secara perubatan</td>
      <td>Bil.</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Non-medically certified</td>
      <td>No.</td>
      <td>%</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Sakit tua 65 tahun dan lebih                  ...</td>
      <td>30665</td>
      <td>47.482271</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Darah tinggi                                  ...</td>
      <td>2393</td>
      <td>3.705367</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Kencing manis                                 ...</td>
      <td>1997</td>
      <td>3.092193</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Penyakit serebrovaskular                      ...</td>
      <td>1708</td>
      <td>2.6447</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Lelah                                         ...</td>
      <td>1076</td>
      <td>1.66455</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Penyakit jantung iskemia                      ...</td>
      <td>1023</td>
      <td>1.584033</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Barah trakea, bronkus dan paru-paru           ...</td>
      <td>742</td>
      <td>1.148927</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Barah kolon, rektum dan dubur                 ...</td>
      <td>715</td>
      <td>1.10712</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Barah hati                                    ...</td>
      <td>533</td>
      <td>0.825307</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Barah payu dara                               ...</td>
      <td>524</td>
      <td>0.811372</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Keseluruhan sebab                           ...</td>
      <td>64582</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Certification Label


```python
df_med['certification'] = 'medical'
df_xmed['certification'] = 'non-medical'
```

    <ipython-input-231-738d62170e63>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_med['certification'] = 'medical'
    <ipython-input-231-738d62170e63>:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_xmed['certification'] = 'non-medical'
    


```python
df_xmed['death_cause'] = df_xmed.death_cause.str.replace('  +', '/')
```

    <ipython-input-232-58ca563d90ad>:1: FutureWarning: The default value of regex will change from True to False in a future version.
      df_xmed['death_cause'] = df_xmed.death_cause.str.replace('  +', '/')
    <ipython-input-232-58ca563d90ad>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_xmed['death_cause'] = df_xmed.death_cause.str.replace('  +', '/')
    


```python
df_xmed['death_cause']=df_xmed.death_cause.str.replace(".*/", '')
```

    <ipython-input-233-d35e3569a3bb>:1: FutureWarning: The default value of regex will change from True to False in a future version.
      df_xmed['death_cause']=df_xmed.death_cause.str.replace(".*/", '')
    <ipython-input-233-d35e3569a3bb>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_xmed['death_cause']=df_xmed.death_cause.str.replace(".*/", '')
    

### Combine DataFrames


```python
df = df_med.append(df_xmed,ignore_index = True)
```

### Reset Index


```python
df.reset_index(drop=True,inplace=True)
```


```python
df
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
      <th>death_cause</th>
      <th>death_count</th>
      <th>percentage</th>
      <th>certification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ischaemic heart diseases</td>
      <td>16325</td>
      <td>14.954564</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pneumonia</td>
      <td>13356</td>
      <td>12.234803</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cerebrovascular diseases</td>
      <td>8691</td>
      <td>7.961416</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Transport accidents</td>
      <td>4198</td>
      <td>3.84559</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Malignant neoplasm of trachea, bronchus and lung</td>
      <td>2602</td>
      <td>2.38357</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Chronic lower respiratory diseases</td>
      <td>2312</td>
      <td>2.117914</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Diabetes mellitus</td>
      <td>2008</td>
      <td>1.839434</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Malignant neoplasm of colon, rectum and anus</td>
      <td>1873</td>
      <td>1.715767</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Malignant neoplasm of breast</td>
      <td>1869</td>
      <td>1.712103</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Diseases of the liver</td>
      <td>1601</td>
      <td>1.466601</td>
      <td>medical</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Old age 65 years and over</td>
      <td>30665</td>
      <td>47.482271</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hypertension</td>
      <td>2393</td>
      <td>3.705367</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Diabetes mellitus</td>
      <td>1997</td>
      <td>3.092193</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Cerebrovascular diseases</td>
      <td>1708</td>
      <td>2.6447</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Asthma</td>
      <td>1076</td>
      <td>1.66455</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ischaemic heart diseases</td>
      <td>1023</td>
      <td>1.584033</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Trachea, bronchus and lung cancer</td>
      <td>742</td>
      <td>1.148927</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Colon, rectum and anus cancer</td>
      <td>715</td>
      <td>1.10712</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Liver cancer</td>
      <td>533</td>
      <td>0.825307</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Breast cancer</td>
      <td>524</td>
      <td>0.811372</td>
      <td>non-medical</td>
    </tr>
  </tbody>
</table>
</div>



### Export Cleaned Table


```python
df.to_csv('cleaned_data/death_causes_2019.csv',index=False)
```

# Visualization


```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
```

## Death by gender in 2019


```python
df = pd.read_csv('cleaned_data/death_rate_by_age_group_and_sex_2019.csv')
```


```python
df.head()
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
      <th>age_group</th>
      <th>total_death_group</th>
      <th>male_death_age</th>
      <th>female_death_age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Under 1 year</td>
      <td>3117</td>
      <td>1711</td>
      <td>1406</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Under 1 day</td>
      <td>652</td>
      <td>351</td>
      <td>301</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1 day to &lt; 1 week</td>
      <td>842</td>
      <td>467</td>
      <td>375</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1 week to &lt; 4 weeks</td>
      <td>486</td>
      <td>267</td>
      <td>219</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4 weeks to &lt; 3 months</td>
      <td>459</td>
      <td>248</td>
      <td>211</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title("Death by age group in Malaysia")


sns.barplot(data=df, x='age_group',y='total_death_group')
plt.xticks(rotation=90)
plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_69_0.png)
    


**Description**: From the graph, we can see the distribution of total death count by age group in Malaysia. From the graph, we can see that people in Malaysia mostly died at the age 70-74 which is nearly 20k deaths.  


```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title("Male Death Age in Malaysia")


sns.barplot(data=df, x='age_group',y='male_death_age',palette='mako')
plt.xticks(rotation=90)
plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_71_0.png)
    


**Description**: From the graph, we can see the distribution of death count of males by age group in Malaysia. From the graph, we can see that men in Malaysia mostly died at the age 70-74 which is nearly 12k deaths.  


```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title("Female Death Age in Malaysia")


sns.barplot(data=df, x='age_group',y='female_death_age',palette='plasma')
plt.xticks(rotation=90)
plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_73_0.png)
    


**Description**: From the graph, we can see the distribution of death count of females by age group in Malaysia. From the graph, we can see that women in Malaysia mostly died at the age 85 and over which is more than 12k deaths.


```python
max_male_age = df.loc[df.male_death_age == df.male_death_age.max(), 'age_group'].values[0]
max_female_age = df.loc[df.female_death_age == df.female_death_age.max(), 'age_group'].values[0]

print('Most male in Malaysia died between '+max_male_age)
print('Most female in Malaysia died between '+max_female_age)
```

    Most male in Malaysia died between 70-74 
    Most female in Malaysia died between 85 and over
    


```python
df['male_death_sum']=df['male_death_age'].sum()
df['female_death_sum']=df['female_death_age'].sum()
```


```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title("Death age by gender in Malaysia")
sns.barplot('age_group', 'value', hue='variable', palette='mako',
             data=pd.melt(df[['male_death_age','female_death_age','age_group']], 'age_group'))
plt.xticks(rotation=90)
plt.show()
```

    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_77_1.png)
    


**Description**: From the graph, we can see the comparison of death count of females and males by age group in Malaysia. From the graph, we can see that more men died and live shorter in comparison women in Malaysia.

## Death by gender in 2019


```python
df = pd.read_csv('cleaned_data/death_causes_2019.csv')
```


```python
df.tail()
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
      <th>death_cause</th>
      <th>death_count</th>
      <th>percentage</th>
      <th>certification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>Ischaemic heart diseases</td>
      <td>1023</td>
      <td>1.584033</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Trachea, bronchus and lung cancer</td>
      <td>742</td>
      <td>1.148927</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Colon, rectum and anus cancer</td>
      <td>715</td>
      <td>1.107120</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Liver cancer</td>
      <td>533</td>
      <td>0.825307</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Breast cancer</td>
      <td>524</td>
      <td>0.811372</td>
      <td>non-medical</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title("Death causes by medical certification in Malaysia")
sns.barplot(data=df, x='death_count',y='death_cause',hue='certification',palette='mako')
plt.xticks(rotation=0)
plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_82_0.png)
    


**Description**: The bar chart shows the major cause of death in Malaysia, grouped into medical and non-medical certified cases. Here, we can see that most of the medically certified death is caused by isochaemic heart diseases. Old age 65 years and over is the most non-medically certified death in Malaysia.


```python
df_med = df[df['certification']=='medical']
df_nmed = df[df['certification']=='non-medical']
```


```python
df_nmed
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
      <th>death_cause</th>
      <th>death_count</th>
      <th>percentage</th>
      <th>certification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>Old age 65 years and over</td>
      <td>30665</td>
      <td>47.482271</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hypertension</td>
      <td>2393</td>
      <td>3.705367</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Diabetes mellitus</td>
      <td>1997</td>
      <td>3.092193</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Cerebrovascular diseases</td>
      <td>1708</td>
      <td>2.644700</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Asthma</td>
      <td>1076</td>
      <td>1.664550</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ischaemic heart diseases</td>
      <td>1023</td>
      <td>1.584033</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Trachea, bronchus and lung cancer</td>
      <td>742</td>
      <td>1.148927</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Colon, rectum and anus cancer</td>
      <td>715</td>
      <td>1.107120</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Liver cancer</td>
      <td>533</td>
      <td>0.825307</td>
      <td>non-medical</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Breast cancer</td>
      <td>524</td>
      <td>0.811372</td>
      <td>non-medical</td>
    </tr>
  </tbody>
</table>
</div>




```python
import squarify

df_med.sort_values('death_count', ascending=False, inplace=True)

fig, ax = plt.subplots(1, figsize = (26,8))
squarify.plot(sizes=df_med['death_count'], 
                label=df_med['death_cause'][:10], 
                alpha=.8)
plt.axis('off')
plt.show()
```

    <ipython-input-253-49e775037926>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_med.sort_values('death_count', ascending=False, inplace=True)
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_86_1.png)
    



```python
import squarify

df_nmed.sort_values('death_count', ascending=False, inplace=True)

fig, ax = plt.subplots(1, figsize = (26,8))
squarify.plot(sizes=df_nmed['death_count'], 
                label=df_nmed['death_cause'][:10], 
                alpha=.8)
plt.axis('off')
plt.show()
```

    <ipython-input-254-61ce348f60b8>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_nmed.sort_values('death_count', ascending=False, inplace=True)
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_87_1.png)
    


## Death by state in 2019


```python
df = pd.read_csv('cleaned_data/death_rate_by_state_and_sex_2019.csv')
```


```python
df.head()
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
      <th>state_death</th>
      <th>total_death</th>
      <th>male_death</th>
      <th>female_death</th>
      <th>total_death_rate</th>
      <th>male_death_rate</th>
      <th>female_death_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Johor</td>
      <td>21776</td>
      <td>12743</td>
      <td>9033</td>
      <td>5.789692</td>
      <td>6.453459</td>
      <td>5.056066</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kedah</td>
      <td>14292</td>
      <td>8027</td>
      <td>6265</td>
      <td>6.574828</td>
      <td>7.287441</td>
      <td>5.842794</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelantan</td>
      <td>11389</td>
      <td>6436</td>
      <td>4953</td>
      <td>6.045617</td>
      <td>6.759489</td>
      <td>5.316083</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Melaka</td>
      <td>5542</td>
      <td>3084</td>
      <td>2458</td>
      <td>5.969410</td>
      <td>6.619759</td>
      <td>5.314342</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Negeri Sembilan</td>
      <td>7157</td>
      <td>4097</td>
      <td>3060</td>
      <td>6.355022</td>
      <td>7.056639</td>
      <td>5.608422</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title(r"Death age by state and gender in Malaysia (2019)")
sns.barplot('state_death', 'value', hue='variable', palette='mako',
             data=pd.melt(df[['male_death','female_death','state_death']], 'state_death'))
plt.xticks(rotation=50)
plt.show()
```

    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_91_1.png)
    


**Description**: From the chart, we can se that Selangor recorded the most death cases in Malaysia.In contrast, W.P. Putrajaya recorded the least death case. Overall, we can see that all of the states recorded more death for men than women.


```python
fig = plt.figure(figsize=(10,5),facecolor='1')
plt.title(r"Death rate per 1000 people by state and gender in Malaysia (2019)")

sns.barplot('state_death', 'value', hue='variable', palette='mako',
             data=pd.melt(df[['male_death_rate','female_death_rate','total_death_rate','state_death']], 'state_death'))
plt.xticks(rotation=50)
plt.show()
```

    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Death%20cause%20Analysis%20Malaysia%202019/output_93_1.png)
    


**Description**: The bar chart shows the death rate per 1000 people by state and gender in Malaysia. Here, we can see that Perlis recorded the highest total death rate for both gender in Malaysia. In contrast, W.P. Putrajaya have the lowest death rate for both genders. For all of the states, we can see that men have a higher death rate than women.
