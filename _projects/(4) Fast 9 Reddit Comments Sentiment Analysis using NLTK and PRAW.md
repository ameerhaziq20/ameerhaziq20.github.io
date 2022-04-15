---
name: Fast 9 Reddit Comments Sentiment Analysis using NLTK and PRAW
tools: [Python, Pandas, nltk, PRAW, Matplotlib, Seaborn]
image: https://i.ytimg.com/vi/bMw-BlfIb30/maxresdefault.jpg
description: Let's see how Redditors respond to the Fast & Furious 9 movie.
---

![jpg](https://image11.m1905.cn/uploadfile/2021/0421/20210421100242955450.jpg)

Let's analyze the reviews for the Fast & Furious 9 movie. If you’re unfamiliar with the premise of the latest outing for Dominic Toretto (played by Vin Diesel) and his family, here’s a brief recap. Following the events of The Fate of the Furious, the crew must face off against Jakob (played by John Cena), who is Dominic’s younger brother and a deadly assassin. Here, we will scrape comments from a Reddit post discusing about their thoughts about the movie.

# 1. Questions

This analysis will try to answer the following questions:
* What are the Redditors comments on the Fast 9 movie?
* Are the comments mostly potray their opinions or stating a fact?
* What are the positive and negative comments mostly about?

# 2. Measurement Priorities

* The polarity of the Redditor's comments by analyzing the sentiment's polarity score. 
* The subjectivity of the Redditor's comments by analyzing the subjectivity score of the sentiments.
* Creating a collection of words for each polarity of sentiments using word clouds.

# 3. Data Collection

* Source
    * The comments collected will be from reddit.com using their API.
    * The python library PRAW will be used to gather the comments.

* Storage
    * The collected comments will be stored in a CSV file.
    * The cleaned comments will also be stored as both CSV file.


##### [Github link for project](https://github.com/ameerhaziq20/data-science-portfolio/tree/main/Fast%209%20Movie%20Sentiment%20Analysis%20From%20Reddit%20Using%20PRAW)
---

# Obtain Reddit Comments


```python
import praw
import pandas as pd
import datetime as dt 
```

## Create Instance of Reddit


```python
reddit = praw.Reddit(
    user_agent="Comment Extraction (by u/USERNAME)",
    client_id="*********",
    client_secret="*********",
    username="*********",
    password="*********",
)
```

## Link to Reddit Post


```python
url = "https://www.reddit.com/r/movies/comments/o7e258/official_discussion_f9_the_fast_saga_spoilers/"
submission = reddit.submission(url=url)
```

## Parsing the comments


```python
from praw.models import MoreComments
from datetime import datetime

# Create temporary list to hold the comments data.
list_comments=[]
list_time=[]

# Store in a dictionary.
dict_collect = {'comments':list_comments,
                'time': list_time,
               }

# Loop through each top level comment.
for top_level_comment in submission.comments:
    if isinstance(top_level_comment, MoreComments):
        continue
    # Append comment to list
    list_comments.append(top_level_comment.body)
    
    # Convert and append time to list
    time = (datetime.utcfromtimestamp(top_level_comment.created_utc).strftime('%Y-%m-%d %H:%M:%S'))
    list_time.append(time)
```

## Convert to DataFrame


```python
df = pd.DataFrame.from_dict(dict_collect)
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
      <th>comments</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>F10&amp;11 are gonna get the Infinity War/Endgame ...</td>
      <td>2021-06-26 10:42:18</td>
    </tr>
    <tr>
      <th>96</th>
      <td>I got many problems with the plot. Doms brothe...</td>
      <td>2021-07-04 20:50:01</td>
    </tr>
    <tr>
      <th>97</th>
      <td>They did my dude Sean the Tokyo Drifter dirty....</td>
      <td>2021-07-29 21:03:51</td>
    </tr>
    <tr>
      <th>98</th>
      <td>I have a nephew who is eight and loves cars bu...</td>
      <td>2021-07-16 06:26:07</td>
    </tr>
    <tr>
      <th>99</th>
      <td>That Toretto Nordic blood</td>
      <td>2021-06-26 04:02:12</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 2 columns</p>
</div>




```python
df.to_csv('dataset/fast_9_review.csv',index = False)
```

# EDA
Let's take a look at our gathered comments about Fast 9 from the posts.

## Word count


```python
eda = pd.read_csv('dataset/fast_9_review.csv')
```


```python
eda.head()
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
      <th>comments</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
    </tr>
  </tbody>
</table>
</div>




```python
eda['word_count'] = eda['comments'].apply(lambda x: len(str(x).split(" ")))
```


```python
eda.head()
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
      <th>comments</th>
      <th>time</th>
      <th>word_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>14</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Average Word count: '+str(int(eda['word_count'].mean()))+ ' words')
```

    Average Word count: 46 words
    

## Average word lenght


```python
def avg_word(comments):
  words = comments.split()
  return int((sum(len(word) for word in words) / len(words)))

# Calculate average words
eda['avg_word_len'] = eda['comments'].apply(lambda x: avg_word(x))
```


```python
eda.head()
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
      <th>comments</th>
      <th>time</th>
      <th>word_count</th>
      <th>avg_word_len</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>22</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>22</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>14</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>19</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>28</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Character count


```python
eda['char_count'] = eda['comments'].str.len()
```


```python
eda.head()
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
      <th>comments</th>
      <th>time</th>
      <th>word_count</th>
      <th>avg_word_len</th>
      <th>char_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>22</td>
      <td>4</td>
      <td>111</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>22</td>
      <td>4</td>
      <td>126</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>14</td>
      <td>4</td>
      <td>81</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>19</td>
      <td>3</td>
      <td>98</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>28</td>
      <td>3</td>
      <td>135</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Average character count: '+str(int(eda['char_count'].mean()))+ ' words')
```

    Average character count: 251 words
    

## Stopword count


```python
# Import stopwords
from nltk.corpus import stopwords
```


```python
stop_words = stopwords.words('english')
eda['stopword_count'] = eda['comments'].apply(lambda x: len([x for x in x.split() if x in stop_words]))
```


```python
eda.head()
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
      <th>comments</th>
      <th>time</th>
      <th>word_count</th>
      <th>avg_word_len</th>
      <th>char_count</th>
      <th>stopword_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>22</td>
      <td>4</td>
      <td>111</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>22</td>
      <td>4</td>
      <td>126</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>14</td>
      <td>4</td>
      <td>81</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>19</td>
      <td>3</td>
      <td>98</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>28</td>
      <td>3</td>
      <td>135</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>



## Summary statistics


```python
eda.describe()
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
      <th>word_count</th>
      <th>avg_word_len</th>
      <th>char_count</th>
      <th>stopword_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>46.480000</td>
      <td>4.040000</td>
      <td>251.420000</td>
      <td>18.430000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>64.721233</td>
      <td>1.033969</td>
      <td>355.543465</td>
      <td>26.918228</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>9.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>14.000000</td>
      <td>4.000000</td>
      <td>67.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>22.500000</td>
      <td>4.000000</td>
      <td>126.000000</td>
      <td>9.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>51.250000</td>
      <td>4.000000</td>
      <td>268.750000</td>
      <td>21.250000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>465.000000</td>
      <td>9.000000</td>
      <td>2604.000000</td>
      <td>196.000000</td>
    </tr>
  </tbody>
</table>
</div>



# Process Text


```python
data = pd.read_csv('dataset/fast_9_review.csv')
```


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
    </tr>
  </tbody>
</table>
</div>



## Convert to lowercase


```python
# Converting each comments to lowercase
data['lowercased'] = data['comments'].apply(lambda x: " ".join(x.lower() for x in x.split()))
```


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
      <th>lowercased</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>if you’re ever falling to your death try land ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>i think the most intimidating villain in the m...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>tez: we'll be alright as long as we obey the l...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>i wish dom would interact more with the rest o...</td>
    </tr>
  </tbody>
</table>
</div>



## Punctuations removal


```python
# remove punctuations from comments
data['nopunc'] = data['lowercased'].str.replace('[^\w\s]', '')
```

    <ipython-input-138-94766295ce28>:2: FutureWarning: The default value of regex will change from True to False in a future version.
      data['nopunc'] = data['lowercased'].str.replace('[^\w\s]', '')
    


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
      <th>lowercased</th>
      <th>nopunc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>if you’re ever falling to your death try land ...</td>
      <td>if youre ever falling to your death try land o...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>i think the most intimidating villain in the m...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>tez: we'll be alright as long as we obey the l...</td>
      <td>tez well be alright as long as we obey the law...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>i wish dom would interact more with the rest o...</td>
    </tr>
  </tbody>
</table>
</div>



## Remove stopwords


```python
# Import stopwords
from nltk.corpus import stopwords
stop_words = stopwords.words('english')

data['nopunc_nostop'] = data['nopunc'].apply(lambda x: " ".join(x for x in x.split() if x not in stop_words))
```


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
      <th>lowercased</th>
      <th>nopunc</th>
      <th>nopunc_nostop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>if you’re ever falling to your death try land ...</td>
      <td>if youre ever falling to your death try land o...</td>
      <td>youre ever falling death try land car break fa...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>think intimidating villain movie charlize ther...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>tez: we'll be alright as long as we obey the l...</td>
      <td>tez well be alright as long as we obey the law...</td>
      <td>tez well alright long obey laws physics entire...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# View the top 30 words used
freq= pd.Series(" ".join(data['nopunc_nostop']).split()).value_counts()[:30]
freq
```




    movie        44
    dom          38
    like         25
    one          19
    scene        18
    car          17
    space        15
    also         15
    fast         14
    family       13
    got          13
    movies       13
    dont         12
    end          12
    going        12
    han          12
    felt         11
    vin          11
    cena         11
    doms         11
    part         11
    back         11
    get          10
    time         10
    even         10
    shaw         10
    still        10
    franchise     9
    roman         9
    actually      9
    dtype: int64




```python
other_stopwords = ['actually', 'time', 'one', 'get', 'got', 'even', 'time']
```


```python
data['nopunc_nostop_nocommon'] = data['nopunc_nostop'].apply(lambda x: "".join(" ".join(x for x in x.split() if x not in other_stopwords)))
```


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
      <th>lowercased</th>
      <th>nopunc</th>
      <th>nopunc_nostop</th>
      <th>nopunc_nostop_nocommon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>if you’re ever falling to your death try land ...</td>
      <td>if youre ever falling to your death try land o...</td>
      <td>youre ever falling death try land car break fa...</td>
      <td>youre ever falling death try land car break fa...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>think intimidating villain movie charlize ther...</td>
      <td>think intimidating villain movie charlize ther...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>tez: we'll be alright as long as we obey the l...</td>
      <td>tez well be alright as long as we obey the law...</td>
      <td>tez well alright long obey laws physics entire...</td>
      <td>tez well alright long obey laws physics entire...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
    </tr>
  </tbody>
</table>
</div>



## Lemmatize the comments


```python
# Import textblob
from textblob import Word

# Lemmatize final review format
data['cleaned_comments'] = data['nopunc_nostop_nocommon'].apply(lambda x: " ".join([Word(word).lemmatize() for word in x.split()]))
```


```python
data.head()
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
      <th>comments</th>
      <th>time</th>
      <th>lowercased</th>
      <th>nopunc</th>
      <th>nopunc_nostop</th>
      <th>nopunc_nostop_nocommon</th>
      <th>cleaned_comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>If you’re ever falling to your death try land ...</td>
      <td>2021-06-25 02:24:48</td>
      <td>if you’re ever falling to your death try land ...</td>
      <td>if youre ever falling to your death try land o...</td>
      <td>youre ever falling death try land car break fa...</td>
      <td>youre ever falling death try land car break fa...</td>
      <td>youre ever falling death try land car break fa...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Hellen Mirren saying John Cena and Vin Diesel ...</td>
      <td>2021-06-25 06:23:22</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena and vin diesel ...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>hellen mirren saying john cena vin diesel simi...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I think the most intimidating villain in the m...</td>
      <td>2021-06-25 04:46:46</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>i think the most intimidating villain in the m...</td>
      <td>think intimidating villain movie charlize ther...</td>
      <td>think intimidating villain movie charlize ther...</td>
      <td>think intimidating villain movie charlize ther...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tez: we'll be alright as long as we obey the l...</td>
      <td>2021-06-25 02:46:44</td>
      <td>tez: we'll be alright as long as we obey the l...</td>
      <td>tez well be alright as long as we obey the law...</td>
      <td>tez well alright long obey laws physics entire...</td>
      <td>tez well alright long obey laws physics entire...</td>
      <td>tez well alright long obey law physic entire m...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I wish Dom would interact more with the rest o...</td>
      <td>2021-06-25 02:29:29</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>i wish dom would interact more with the rest o...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>wish dom would interact rest crew felt like ha...</td>
    </tr>
  </tbody>
</table>
</div>



## Export cleaned comments


```python
data[['cleaned_comments','time']].to_csv('dataset/fast_9_review_cleaned.csv',index = False)
```

# Visualization


```python
df = pd.read_csv('dataset/fast_9_review_cleaned.csv')
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
      <th>cleaned_comments</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>youre ever falling death try land car break fa...</td>
      <td>2021-06-25 02:24:48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>2021-06-25 06:23:22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>think intimidating villain movie charlize ther...</td>
      <td>2021-06-25 04:46:46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tez well alright long obey law physic entire m...</td>
      <td>2021-06-25 02:46:44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>2021-06-25 02:29:29</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>f1011 gonna infinity warendgame treatment gonn...</td>
      <td>2021-06-26 10:42:18</td>
    </tr>
    <tr>
      <th>96</th>
      <td>many problem plot doms brother big spyevil guy...</td>
      <td>2021-07-04 20:50:01</td>
    </tr>
    <tr>
      <th>97</th>
      <td>dude sean tokyo drifter dirty fuck behind plan...</td>
      <td>2021-07-29 21:03:51</td>
    </tr>
    <tr>
      <th>98</th>
      <td>nephew eight love car mum wont let watch ff mo...</td>
      <td>2021-07-16 06:26:07</td>
    </tr>
    <tr>
      <th>99</th>
      <td>toretto nordic blood</td>
      <td>2021-06-26 04:02:12</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 2 columns</p>
</div>



## Polarity and Subjectivity

### Calculate score


```python
# Calculate polarity
from textblob import TextBlob
df['polarity'] = df['cleaned_comments'].apply(lambda x: TextBlob(x).sentiment[0])

# Calculate subjectivity
df['subjectivity'] = df['cleaned_comments'].apply(lambda x: TextBlob(x).sentiment[1])
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
      <th>cleaned_comments</th>
      <th>time</th>
      <th>polarity</th>
      <th>subjectivity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>youre ever falling death try land car break fa...</td>
      <td>2021-06-25 02:24:48</td>
      <td>0.025000</td>
      <td>0.175000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>2021-06-25 06:23:22</td>
      <td>0.066667</td>
      <td>0.266667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>think intimidating villain movie charlize ther...</td>
      <td>2021-06-25 04:46:46</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tez well alright long obey law physic entire m...</td>
      <td>2021-06-25 02:46:44</td>
      <td>0.087500</td>
      <td>0.581250</td>
    </tr>
    <tr>
      <th>4</th>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>2021-06-25 02:29:29</td>
      <td>-0.291667</td>
      <td>0.541667</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Summary statictics of the scores
df.describe()
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
      <th>polarity</th>
      <th>subjectivity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>100.000000</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.057331</td>
      <td>0.439929</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.286197</td>
      <td>0.272889</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-0.900000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.300000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.046165</td>
      <td>0.435020</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.200000</td>
      <td>0.606399</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



### Label the scores


```python
# Add polarity label

def polar_label(polar):
  if (polar<-0.5):
    return "negative"
  elif (polar>=-0.5) and (polar<0):
    return "weak negative"
  elif (polar==0):
    return "neutral"
  elif (polar>0)and(polar<=0.5):
    return "weak positive"
  else:
    return "positive"

df['polar_label'] = df['polarity'].apply(lambda x: polar_label(x))
```


```python
# Add subjectivity label

def subj_label(subj):
  if subj<0.5:
    return "objective"
  elif subj==0.5:
    return "neutral"
  else:
    return "subjective"

df['subj_label'] = df['subjectivity'].apply(lambda x: subj_label(x))
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
      <th>cleaned_comments</th>
      <th>time</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>youre ever falling death try land car break fa...</td>
      <td>2021-06-25 02:24:48</td>
      <td>0.025000</td>
      <td>0.175000</td>
      <td>weak positive</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hellen mirren saying john cena vin diesel simi...</td>
      <td>2021-06-25 06:23:22</td>
      <td>0.066667</td>
      <td>0.266667</td>
      <td>weak positive</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>2</th>
      <td>think intimidating villain movie charlize ther...</td>
      <td>2021-06-25 04:46:46</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>neutral</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tez well alright long obey law physic entire m...</td>
      <td>2021-06-25 02:46:44</td>
      <td>0.087500</td>
      <td>0.581250</td>
      <td>weak positive</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>4</th>
      <td>wish dom would interact rest crew felt like ha...</td>
      <td>2021-06-25 02:29:29</td>
      <td>-0.291667</td>
      <td>0.541667</td>
      <td>weak negative</td>
      <td>subjective</td>
    </tr>
  </tbody>
</table>
</div>




```python
# count of polar label
df['polar_label'].value_counts()
```




    weak positive    52
    neutral          20
    weak negative    19
    positive          5
    negative          4
    Name: polar_label, dtype: int64



## Score distribution


```python
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
plt.figure(figsize=(10,5),facecolor='1')
plt.title('Polarity score distribution')

sns.kdeplot(
   data=df, x="polarity", 
   fill=True, palette="Set1",
   alpha=.5, linewidth=1)

plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_67_0.png)
    


**Description:** The polarity score distribution graph shows a unimodal bell shaped distribution. A left skew is shown from the distribution which tells us that the mean gets pulled towards the tail, and is less than the median. 


```python
plt.figure(figsize=(10,5),facecolor='1')
plt.title('Subjectivity score distribution')

sns.kdeplot(
   data=df, x="subjectivity", 
   fill=True, palette="Set1",
   alpha=.5, linewidth=1)

plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_69_0.png)
    


**Description:** The subjectivity score distribution graph shows a bimodal distribution. A slight right skew is shown from the distribution which tells us that the mean gets pulled towards the tail, and is greater than the median. 

## Label count


```python
plt.figure(figsize=(10,5),facecolor='1')
plt.title('Count of polarity score')

sns.countplot(
   data=df, x="polar_label",palette="Set1")

plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_72_0.png)
    


**Description:** The bar chart shows that count of polarity labels from the Fast 9 movie Reddit comments. From the chart we can see that most of the comments are 'weak positive'. This shows that Reddit comments on the post are reacting slightly positive towards the movie.


```python
plt.figure(figsize=(10,5),facecolor='1')
plt.title('Count of polarity score')

sns.countplot(
   data=df, x="subj_label",palette="Set1")

plt.show()
```


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_74_0.png)
    


**Description:** The bar chart shows that count of subjectivity labels from the Fast 9 movie Reddit comments. From the chart we can see that most of the comments are 'objective'. This means that most of the collected comments are mostly factual albeit by only a small margin.  

## Wordcloud

### Positive comments wordcloud


```python
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
```


```python
# Text of all words in column Tweets

df_neg = df[df['polar_label']=='negative']

text = " ".join(review for review in df_neg.cleaned_comments.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='Set1').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(10,5))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 328 words in the combination of all cells in column Tweets.
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_79_1.png)
    


### Negative comments wordcloud


```python
# Text of all words in column Tweets

df_neg = df[df['polar_label']=='positive']

text = " ".join(review for review in df_neg.cleaned_comments.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='plasma').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(10,5))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 274 words in the combination of all cells in column Tweets.
    


    
![png](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Fast%209%20Reddit%20Comments%20Sentiment%20Analysis%20using%20NLTK%20and%20PRAW/output_81_1.png)
    

