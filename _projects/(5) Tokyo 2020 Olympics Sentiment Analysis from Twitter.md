---
name: Tokyo 2020 Olympics Sentiment Analysis from Twitter using Tweepy
tools: [Python, Pandas, nltk, scikit-learn, Tweepy, Matplotlib, Seaborn]
image: https://cdn-japantimes.com/wp-content/uploads/2021/01/np_file_63428.jpeg
description: Find out the the worlds reaction on the 2020 Summer Olympics Tokyo.
---

![jpg](https://4friendsnihongo.com/wp-content/uploads/2019/10/Tokyo-2020-Olympics-Banner.jpg)

The extraordinary 2020 Summer Olympics will take place without fans or spectators and under a state of emergency due to the coronavirus pandemic, after a 12-month delay. But nevertheless, even without the any fans at the stadium, there are still words of support from all around the world at the palms of our hand. But are the Tweets truly supportive, or filled with hate comments? Let's find out.

# 1. Questions

This analysis will try to answer the following questions:
* What are the people's reactions on Twitter about the 2020 Summer Olympics Tokyo?
* Are the Tweets posted are about their opinions or stating a fact?
* What are the Tweets mostly talk about?

# 2. Measurement Priorities

* The polarity of the Tweets by analyzing the sentiment's polarity score. 
* The subjectivity of the Tweets by analyzing the subjectivity score of the sentiments.
* Creating a collection of words for each group of sentiments using word clouds.

# 3. Data Collection

* Source
    * The Tweets collected will be from Twitter.com using their API.
    * The python library Tweepy will be used to gather the Tweets.
    * The Tweets collected will be the posts tagged with #Tokyo2020.

* Storage
    * The collected Tweets will be stored in a CSV and TXT file.
    * The cleaned Tweets will also be stored as both CSV and TXT file.

##### [Github link for project](https://github.com/ameerhaziq20/Tokyo_2020_Olympics_Sentiment_Analysis_from_Twitter/)
---

# Importing main libraries


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

#Setting Up Tweepy to use Twitter API

![twitter j.png](data:image/png;base64,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)

## Twitter Authentication


```python
import tweepy as tw
```


```python
#authentication information.
consumer_key ='*********'
consumer_secret='*********'
access_token='*********'
access_token_secret='**********'
```


```python
#Authentication
auth=tw.OAuthHandler(consumer_key,consumer_secret)
auth.set_access_token(access_token,access_token_secret)
api=tw.API(auth,wait_on_rate_limit=True)
```

## Collect Tweets


```python
 #fetch tweets

 hashtag = '#Tokyo2020'
 query = tw.Cursor(api.search,q=hashtag, lang='en').items(2000)
 tweets = [{'Tweets':tweet.text,'Timestamp':tweet.created_at} for tweet in query]
 print(tweets)

```


```python
#create a copy of the raw data
tweet=tweets.copy()
```


```python
#store as DataFrame
data=pd.DataFrame.from_dict(tweet)
data.head()
```

## Export Collected Data


```python
data.to_csv(r'/original/tokyo2020all.csv', index = False)
data.to_csv(r'/original/tokyo2020all.txt', index = False)
```

# Load Exported data to Data Frame


```python
text_data='original/tokyo2020all.txt'
```


```python
data_txt = pd.read_csv(text_data)
```


```python
data_txt.shape
```




    (2000, 2)




```python
df=data_txt.copy()
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
      <th>Tweets</th>
      <th>Timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Limerick is incredibly proud of @sarahlavin_ 🏃...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Ra_THORe: Amazing performance! \n#Kamalpre...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @BadmintonTalk: INDONESIA #INA Men's Single...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
  </tbody>
</table>
</div>



# EDA

## Data Shape


```python
eda = df.copy()
```


```python
eda.shape
```




    (2000, 2)



## Data Types and Null count


```python
eda.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2000 entries, 0 to 1999
    Data columns (total 2 columns):
     #   Column     Non-Null Count  Dtype 
    ---  ------     --------------  ----- 
     0   Tweets     2000 non-null   object
     1   Timestamp  2000 non-null   object
    dtypes: object(2)
    memory usage: 31.4+ KB
    

## Scraped Tweets WordCloud


```python
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
```


```python
# Text of all words in column Tweets

text = " ".join(review for review in eda.Tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)
stopwords.update(['RT','#F9'])

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='inferno').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 270852 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_29_1.jpg)
    


## Language Detection


```python
from langdetect import detect

lang = detect(text)

print(lang)
```

    en
    

# Data Preprocessing


```python
df_cleaning=df.copy()
```


```python
df_cleaning.head()
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
      <th>Tweets</th>
      <th>Timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Limerick is incredibly proud of @sarahlavin_ 🏃...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Ra_THORe: Amazing performance! \n#Kamalpre...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @BadmintonTalk: INDONESIA #INA Men's Single...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
  </tbody>
</table>
</div>



## Removing Hashtags and Mentions


```python
import re
def cleaning_hash_mentions(data):
    return re.sub("((#[A-Za-z0-9]+)|(@[A-Za-z0-9]+))",' ',data)

df_cleaning['tweets_no_tags'] = df_cleaning['Tweets'].apply(lambda x: cleaning_hash_mentions(x))
```

## Removing URLs


```python
#remove URLs
import re
def cleaning_URLs(data):
    return re.sub('((www.[^s]+)|(https?://[^s]+))',' ',data)

df_cleaning['tweets_no_url'] = df_cleaning['tweets_no_tags'].apply(lambda x: cleaning_URLs(x))
```

## Removing Numbers


```python
#remove numbers
def cleaning_numbers(data):
    return re.sub('[0-9]+', '', data)
df_cleaning['tweets_no_num'] = df_cleaning['tweets_no_url'].apply(lambda x: cleaning_numbers(x))
```

## Set to Lower Case


```python
# Lower case all words
df_cleaning['tweets_lower'] = df_cleaning['tweets_no_num'].apply(lambda x: " ".join(x.lower() for x in x.split()))
```

## Removing Punctuations


```python
# Remove Punctuation
df_cleaning['tweets_nopunc'] = df_cleaning['tweets_lower'].str.replace('[^\w\s]', '')
```

    <ipython-input-19-92b1699700d6>:2: FutureWarning: The default value of regex will change from True to False in a future version.
      df_cleaning['tweets_nopunc'] = df_cleaning['tweets_lower'].str.replace('[^\w\s]', '')
    

## Removing Stopwords


```python
# Import stopwords
import nltk
from nltk.corpus import stopwords
nltk.download('stopwords')
stop_words = stopwords.words('english')

# Remove Stopwords
df_cleaning['tweets_nopunc_nostop'] = df_cleaning['tweets_nopunc'].apply(lambda x: " ".join(x for x in x.split() if x not in stop_words))
```

    [nltk_data] Downloading package stopwords to
    [nltk_data]     C:\Users\Haziq\AppData\Roaming\nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    


```python
# Return frequency of values
freq= pd.Series(" ".join(df_cleaning['tweets_nopunc_nostop']).split()).value_counts()[:30]
```


```python
freq
```




    rt             1821
    anthony         376
    ginting         362
    first           321
    womens          265
    win             242
    ms              231
    gold            223
    sinisuka        218
    finals          200
    final           199
    nesthy          196
    become          195
    indonesia       193
    indonesian      185
    petecio         185
    medal           184
    mens            181
    player          180
    dhabitah        139
    olympic         137
    singles         133
    reach           131
    game            130
    another         129
    rises           128
    nur             127
    springboard     117
    last            116
    vs              113
    dtype: int64




```python
other_stopwords = ['rt', 'tokyo2020', 'reach','ganbattemalaysia','badmintontalk','sokongmalaysia']
```


```python
df_cleaning['tweets_nopunc_nostop_nocommon'] = df_cleaning['tweets_nopunc_nostop'].apply(lambda x: "".join(" ".join(x for x in x.split() if x not in other_stopwords)))
```

## Lemmatization


```python
# Import textblob
from textblob import Word
nltk.download('wordnet')

# Lemmatize final review format
df_cleaning['cleaned_tweets'] = df_cleaning['tweets_nopunc_nostop_nocommon']\
.apply(lambda x: " ".join([Word(word).lemmatize() for word in x.split()]))
```

    [nltk_data] Downloading package wordnet to
    [nltk_data]     C:\Users\Haziq\AppData\Roaming\nltk_data...
    [nltk_data]   Package wordnet is already up-to-date!
    

## Output from Preprocessing


```python
df_cleaning.head()
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
      <th>Tweets</th>
      <th>Timestamp</th>
      <th>tweets_no_tags</th>
      <th>tweets_no_url</th>
      <th>tweets_no_num</th>
      <th>tweets_lower</th>
      <th>tweets_nopunc</th>
      <th>tweets_nopunc_nostop</th>
      <th>tweets_nopunc_nostop_nocommon</th>
      <th>cleaned_tweets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>rt : nur dhabitah is through to the final!</td>
      <td>rt  nur dhabitah is through to the final</td>
      <td>rt nur dhabitah final</td>
      <td>nur dhabitah final</td>
      <td>nur dhabitah final</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Limerick is incredibly proud of @sarahlavin_ 🏃...</td>
      <td>2021-07-31 07:26:36</td>
      <td>Limerick is incredibly proud of  _ 🏃‍♀️ She is...</td>
      <td>Limerick is incredibly proud of  _ 🏃‍♀️ She is...</td>
      <td>Limerick is incredibly proud of  _ 🏃‍♀️ She is...</td>
      <td>limerick is incredibly proud of _ 🏃‍♀️ she is ...</td>
      <td>limerick is incredibly proud of _  she is a ph...</td>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @stadiumastro: NUR DHABITAH IS THROUGH TO T...</td>
      <td>2021-07-31 07:26:36</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>RT  : NUR DHABITAH IS THROUGH TO THE FINAL!\n\...</td>
      <td>rt : nur dhabitah is through to the final!</td>
      <td>rt  nur dhabitah is through to the final</td>
      <td>rt nur dhabitah final</td>
      <td>nur dhabitah final</td>
      <td>nur dhabitah final</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Ra_THORe: Amazing performance! \n#Kamalpre...</td>
      <td>2021-07-31 07:26:36</td>
      <td>RT  _THORe: Amazing performance! \n  advances ...</td>
      <td>RT  _THORe: Amazing performance! \n  advances ...</td>
      <td>RT  _THORe: Amazing performance! \n  advances ...</td>
      <td>rt _thore: amazing performance! advances to wo...</td>
      <td>rt _thore amazing performance advances to wome...</td>
      <td>rt _thore amazing performance advances womens ...</td>
      <td>_thore amazing performance advances womens dis...</td>
      <td>_thore amazing performance advance woman discu...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @BadmintonTalk: INDONESIA #INA Men's Single...</td>
      <td>2021-07-31 07:26:36</td>
      <td>RT  : INDONESIA   Men's Singles Rises Again!\n...</td>
      <td>RT  : INDONESIA   Men's Singles Rises Again!\n...</td>
      <td>RT  : INDONESIA   Men's Singles Rises Again!\n...</td>
      <td>rt : indonesia men's singles rises again! anth...</td>
      <td>rt  indonesia mens singles rises again anthony...</td>
      <td>rt indonesia mens singles rises anthony sinisu...</td>
      <td>indonesia mens singles rises anthony sinisuka ...</td>
      <td>indonesia men single rise anthony sinisuka gin...</td>
    </tr>
  </tbody>
</table>
</div>



## Convert Tweet Time Object to Timestamp


```python
df_cleaning['Timestamp'] = pd.to_datetime(df_cleaning['Timestamp'])
```


```python
df_cleaning.dtypes
```




    Tweets                                   object
    Timestamp                        datetime64[ns]
    tweets_no_tags                           object
    tweets_no_url                            object
    tweets_no_num                            object
    tweets_lower                             object
    tweets_nopunc                            object
    tweets_nopunc_nostop                     object
    tweets_nopunc_nostop_nocommon            object
    cleaned_tweets                           object
    dtype: object




```python
df_cleaned= df_cleaning[["cleaned_tweets","Timestamp"]]
```


```python
df_cleaned.head()
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
      <th>cleaned_tweets</th>
      <th>Timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nur dhabitah final</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>1</th>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nur dhabitah final</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>_thore amazing performance advance woman discu...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>2021-07-31 07:26:36</td>
    </tr>
  </tbody>
</table>
</div>



## Export Cleaned Data


```python
df_cleaned.to_csv('cleaned/tokyo2020_cleaned.csv', index = False)
df_cleaned.to_csv('cleaned/tokyo2020_cleaned.txt', index = False)
```

# Sentiment Analysis


```python
df_sa=df_cleaned.copy()
```


```python
# Calculate polarity and subjectivity
from textblob import TextBlob
df_sa['polarity'] = df_sa['cleaned_tweets'].apply(lambda x: TextBlob(x).sentiment[0])
df_sa['subjectivity'] = df_sa['cleaned_tweets'].apply(lambda x: TextBlob(x).sentiment[1])
```


```python
df_sa.head()
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
      <th>cleaned_tweets</th>
      <th>Timestamp</th>
      <th>polarity</th>
      <th>subjectivity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nur dhabitah final</td>
      <td>2021-07-31 07:26:36</td>
      <td>0.000000</td>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
      <td>2021-07-31 07:26:36</td>
      <td>0.650000</td>
      <td>0.75000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nur dhabitah final</td>
      <td>2021-07-31 07:26:36</td>
      <td>0.000000</td>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>_thore amazing performance advance woman discu...</td>
      <td>2021-07-31 07:26:36</td>
      <td>0.300000</td>
      <td>0.95000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>2021-07-31 07:26:36</td>
      <td>0.089286</td>
      <td>0.27381</td>
    </tr>
  </tbody>
</table>
</div>



## Polarity Label


```python
df_label = df_sa.copy()
```


```python
def polar_label(polar):
  if polar<0:
    return "negative"
  elif polar==0:
    return "neutral"
  else:
    return "positive"

df_label['polar_label'] = df_label['polarity'].apply(lambda x: polar_label(x))
```


```python
def subj_label(subj):
  if subj<0.5:
    return "objective"
  elif subj==0.5:
    return "neutral"
  else:
    return "subjective"

df_label['subj_label'] = df_label['subjectivity'].apply(lambda x: subj_label(x))
```


```python
df_label[['cleaned_tweets','polarity', 'subjectivity','polar_label','subj_label']].head()
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
      <th>cleaned_tweets</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nur dhabitah final</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>neutral</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>1</th>
      <td>limerick incredibly proud _ phenomenal athlete...</td>
      <td>0.650000</td>
      <td>0.75000</td>
      <td>positive</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nur dhabitah final</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>neutral</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>3</th>
      <td>_thore amazing performance advance woman discu...</td>
      <td>0.300000</td>
      <td>0.95000</td>
      <td>positive</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>4</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>0.089286</td>
      <td>0.27381</td>
      <td>positive</td>
      <td>objective</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_label['polar_label'].value_counts()
```




    positive    989
    neutral     842
    negative    169
    Name: polar_label, dtype: int64




```python
df_label['subj_label'].value_counts()
```




    objective     1226
    subjective     719
    neutral         55
    Name: subj_label, dtype: int64




```python
df_label.isna().sum()
```




    cleaned_tweets    0
    Timestamp         0
    polarity          0
    subjectivity      0
    polar_label       0
    subj_label        0
    dtype: int64



## Polarity & Subjectivity Score


```python
df_label[['polarity','subjectivity']].describe()
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
      <td>2000.000000</td>
      <td>2000.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.134933</td>
      <td>0.413012</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.250944</td>
      <td>0.354464</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.250000</td>
      <td>0.650000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Visualization

### Moving Averages


```python
df_vis = df_label.copy()
```


```python
df_mov_avg = df_vis[['Timestamp', 'polarity','subjectivity']]
df_mov_avg = df_mov_avg.sort_values(by='Timestamp', ascending=True)
df_mov_avg['MA Polarity'] = df_mov_avg.polarity.rolling(10, min_periods=3).mean()
df_mov_avg['MA Subjectivity'] = df_mov_avg.subjectivity.rolling(10, min_periods=3).mean()
```


```python
df_mov_avg.tail()
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
      <th>Timestamp</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>MA Polarity</th>
      <th>MA Subjectivity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>2021-07-31 07:26:36</td>
      <td>0.089286</td>
      <td>0.27381</td>
      <td>0.116323</td>
      <td>0.401171</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-07-31 07:26:36</td>
      <td>0.300000</td>
      <td>0.95000</td>
      <td>0.146323</td>
      <td>0.496171</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-07-31 07:26:36</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>0.133823</td>
      <td>0.579504</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-07-31 07:26:36</td>
      <td>0.650000</td>
      <td>0.75000</td>
      <td>0.155489</td>
      <td>0.594504</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2021-07-31 07:26:36</td>
      <td>0.000000</td>
      <td>1.00000</td>
      <td>0.188823</td>
      <td>0.654504</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_mov_avg.dtypes
```




    Timestamp          datetime64[ns]
    polarity                  float64
    subjectivity              float64
    MA Polarity               float64
    MA Subjectivity           float64
    dtype: object




```python
fig, axes = plt.subplots(2, 1, figsize=(13, 10))

axes[0].plot(df_mov_avg['Timestamp'], df_mov_avg['MA Polarity'], color='darkorange')
axes[0].set_title("\n".join(["Polarity"]))
axes[1].plot(df_mov_avg['Timestamp'], df_mov_avg['MA Subjectivity'], color='darkorchid')
axes[1].set_title("\n".join(["Subjectivity"]))


fig.suptitle("\n".join(["MA of Sentiment Scores"]), y=0.98)
plt.show()
```
    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_82_0.jpg)
    


### Cleaned Tweets WordCloud


```python
# Text of all words in column Tweets

text = " ".join(review for review in df_cleaned.cleaned_tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='plasma').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 130215 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_84_1.jpg)
    


### Positive Tweets WordCloud


```python
df_pos = df_vis[df_vis['polar_label']=='positive']

# Text of all words in column Tweets

text = " ".join(review for review in df_pos.cleaned_tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='inferno').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 72234 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_86_1.jpg)


### Negative Tweets WordCloud


```python
df_neg = df_vis[df_vis['polar_label']=='negative']

# Text of all words in column Tweets

text = " ".join(review for review in df_neg.cleaned_tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='tab10').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 11599 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_88_1.jpg)
    


### Objective Tweets WordCloud


```python
df_obj = df_vis[df_vis['subj_label']=='objective']

# Text of all words in column Tweets

text = " ".join(review for review in df_obj.cleaned_tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='tab10').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 81725 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_90_1.jpg)
    


### Subjective Tweets WordCloud


```python
df_subj = df_vis[df_vis['subj_label']=='subjective']

# Text of all words in column Tweets

text = " ".join(review for review in df_subj.cleaned_tweets.astype(str))
print ("There are {} words in the combination of all cells in column Tweets.".format(len(text)))

# Create stopword list:
# remove words that we want to exclude

stopwords = set(STOPWORDS)

# Generate a word cloud image

wordcloud = WordCloud(stopwords=stopwords, background_color="white", width=800, height=400,colormap='tab10').generate(text)

# Display the generated image:
# the matplotlib way:

fig=plt.figure(figsize=(20,10))
plt.tight_layout(pad=0)
plt.axis("off")
plt.imshow(wordcloud, interpolation='bilinear')
plt.show()
```

    There are 44634 words in the combination of all cells in column Tweets.
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_92_1.jpg)
    


### Label Count and Correlation Heatmap


```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axs = plt.subplots(1,2)
plt.figure(figsize=(10,10))
fig.set_size_inches(30,10)

fig.suptitle('Cleaned Tweets EDA')
label_counts=df_vis['polar_label'].value_counts()
label_counts.plot(kind='pie',autopct='%1.1f%%',ax=axs[0])
axs[0].set_title("Count of labels")

corr = df_mov_avg.corr()
sns.heatmap(corr,ax=axs[1],annot=True,cmap="vlag")
axs[1].set_title("Correlation between variables")
axs[1].tick_params(labelrotation=45,axis='y')

plt.show()

```


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_94_0.jpg)
    



    <Figure size 720x720 with 0 Axes>


### Correlation Between Polarity and Subjectivity


```python
fig, axs = plt.subplots(2,2)
plt.figure(figsize=(10,10))
fig.set_size_inches(30,20)

fig.suptitle('Correlation Between Subjectivity and Polarity')

#polarity distribution
sns.kdeplot(
   data=df_vis, x="polarity", hue="polar_label",
   fill=True, palette="Set1",
   alpha=.5, linewidth=1,ax=axs[0,0]
)
axs[0,0].set_title("Distribution of Polarity Scores")

#correlation between polarity and subjectivity
corr_s_p = df_vis[['polarity','subjectivity']].corr()
sns.heatmap(corr_s_p,annot=True,cmap="vlag",ax=axs[0,1])
axs[0,1].set_title("Correlation between Polarity and Subjectivity")

#jointplot of subjectivity and polarity
sns.scatterplot(data=df_vis, x="polarity", y="subjectivity",ax=axs[1,0])
sns.kdeplot(data=df_vis, x="polarity", y="subjectivity", levels=7,
            linewidths=1,ax=axs[1,0],palette='mako',fill=True)
axs[1,0].set_title("Relationship between Polarity and Subjectivity")

#subjectivity distribution
sns.kdeplot(
   data=df_vis, x="subjectivity", hue="subj_label",
   fill=True, palette="Set1",
   alpha=.5, linewidth=1,ax=axs[1,1]
)
axs[1,1].set_title("Distribution of Subjectivity Scores")

plt.show()
```

    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\distributions.py:306: UserWarning: Dataset has 0 variance; skipping density estimate.
      warnings.warn(msg, UserWarning)
    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\distributions.py:1182: UserWarning: linewidths is ignored by contourf
      cset = contour_func(
    C:\Users\Haziq\Anaconda3\lib\site-packages\seaborn\distributions.py:306: UserWarning: Dataset has 0 variance; skipping density estimate.
      warnings.warn(msg, UserWarning)
    


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_96_1.jpg)
    



    <Figure size 720x720 with 0 Axes>


# Classification

## Pre-Processing

### Encoding the data


```python
df_label.tail()
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
      <th>cleaned_tweets</th>
      <th>Timestamp</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1995</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>2021-07-31 07:20:01</td>
      <td>0.089286</td>
      <td>0.273810</td>
      <td>positive</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>fight place semifinal cest srjdla</td>
      <td>2021-07-31 07:20:00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>neutral</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>little known village kabarwala thrower verg</td>
      <td>2021-07-31 07:20:00</td>
      <td>-0.187500</td>
      <td>0.500000</td>
      <td>negative</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>one win gold nesthy petecio fight woman feathe...</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.169444</td>
      <td>0.647222</td>
      <td>positive</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>hello tokyo</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>neutral</td>
      <td>objective</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_class = df_label.copy()
```


```python
df_class.tail()
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
      <th>cleaned_tweets</th>
      <th>Timestamp</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1995</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>2021-07-31 07:20:01</td>
      <td>0.089286</td>
      <td>0.273810</td>
      <td>positive</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>fight place semifinal cest srjdla</td>
      <td>2021-07-31 07:20:00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>neutral</td>
      <td>objective</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>little known village kabarwala thrower verg</td>
      <td>2021-07-31 07:20:00</td>
      <td>-0.187500</td>
      <td>0.500000</td>
      <td>negative</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>one win gold nesthy petecio fight woman feathe...</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.169444</td>
      <td>0.647222</td>
      <td>positive</td>
      <td>subjective</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>hello tokyo</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>neutral</td>
      <td>objective</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_class['polar_label'] = pd.factorize(df_class['polar_label'])[0]
df_class['subj_label'] = pd.factorize(df_class['subj_label'])[0]
```


```python
df_class.tail()
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
      <th>cleaned_tweets</th>
      <th>Timestamp</th>
      <th>polarity</th>
      <th>subjectivity</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1995</th>
      <td>indonesia men single rise anthony sinisuka gin...</td>
      <td>2021-07-31 07:20:01</td>
      <td>0.089286</td>
      <td>0.273810</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>fight place semifinal cest srjdla</td>
      <td>2021-07-31 07:20:00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>little known village kabarwala thrower verg</td>
      <td>2021-07-31 07:20:00</td>
      <td>-0.187500</td>
      <td>0.500000</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>one win gold nesthy petecio fight woman feathe...</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.169444</td>
      <td>0.647222</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>hello tokyo</td>
      <td>2021-07-31 07:19:59</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Polarity:

*   neutral = 1
*   positive = 2
*   negative = 0

Subjectivity:

*   subjective = 0
*   objective = 1
*   neutral = 2



### Word vectorization


```python
import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer, ENGLISH_STOP_WORDS

vect = CountVectorizer(max_features=200, token_pattern=r'[A-Za-z]+', stop_words=ENGLISH_STOP_WORDS)
vect.fit(df_class.cleaned_tweets)
vect_trans = vect.transform(df_class.cleaned_tweets)
tweet_class = pd.DataFrame(vect_trans.toarray(), columns=vect.get_feature_names())
tweet_class['polar_label'] = df_class.polar_label
tweet_class['subj_label'] = df_class.subj_label
tweet_class.tail(10)
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
      <th>aapne</th>
      <th>advance</th>
      <th>amazing</th>
      <th>amp</th>
      <th>anders</th>
      <th>anthony</th>
      <th>antonsen</th>
      <th>assured</th>
      <th>athle</th>
      <th>athlete</th>
      <th>...</th>
      <th>wanting</th>
      <th>watch</th>
      <th>win</th>
      <th>winning</th>
      <th>witness</th>
      <th>woman</th>
      <th>world</th>
      <th>wow</th>
      <th>polar_label</th>
      <th>subj_label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1990</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1991</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1992</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1994</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1997</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 202 columns</p>
</div>



## Training and Evaluation


```python
# Define the vector of targets and matrix of features
y = tweet_class.polar_label
X = tweet_class.drop('polar_label', axis=1)
```


```python
from sklearn.model_selection import train_test_split
# Split the data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.8, random_state=123, stratify=y)
```


```python
# Import the classification models
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.neural_network import MLPClassifier
```


```python
lr_model=LogisticRegression().fit(X_train,y_train)
svm_model=SVC().fit(X_train,y_train)
nn_model=MLPClassifier().fit(X_train,y_train)

print('Logistic Regression Accuracy: {:.2f}%'.format(lr_model.score(X_test,y_test)*100))
print('Support Vector Machine Accuracy: {:.2f}%'.format(svm_model.score(X_test,y_test)*100))
print('Neural Network Accuracy: {:.2f}%'.format(nn_model.score(X_test,y_test)*100))
```

    C:\Users\Haziq\Anaconda3\lib\site-packages\sklearn\neural_network\_multilayer_perceptron.py:582: ConvergenceWarning: Stochastic Optimizer: Maximum iterations (200) reached and the optimization hasn't converged yet.
      warnings.warn(
    

    Logistic Regression Accuracy: 81.75%
    Support Vector Machine Accuracy: 83.31%
    Neural Network Accuracy: 83.44%
    


```python
from sklearn.metrics import accuracy_score as acs
#make predictions
lr_pred = lr_model.predict(X_test)
svm_pred = svm_model.predict(X_test)
nn_pred = nn_model.predict(X_test)

#accuracy score
from sklearn.metrics import r2_score as r2
print(acs(lr_pred, y_test))
print(acs(svm_pred, y_test))
print(acs(nn_pred, y_test))
```

    0.8175
    0.833125
    0.834375
    

### Confusion Matrix


```python
import numpy as np
import seaborn as sns
from sklearn.metrics import confusion_matrix
fig, axs = plt.subplots(1,3)
plt.figure(figsize=(10,10))
fig.set_size_inches(40, 10)

lr_cmat = confusion_matrix(y_test,lr_pred)
svm_cmat = confusion_matrix(y_test,svm_pred)
nn_cmat = confusion_matrix(y_test,nn_pred)

color ='vlag'
sns.set(font_scale=2)
fig.suptitle("Prediction Model Confusion Matrix")

sns.heatmap(lr_cmat,annot=True,ax=axs[0],cmap=color)
axs[0].set_title('Logistic Regression')
axs[0].set_xlabel('Actual',fontsize =30)
axs[0].set_ylabel('Predicted',fontsize =30)

sns.heatmap(svm_cmat,annot=True,ax=axs[1],cmap=color)
axs[1].set_title('Support Vector machine')
axs[1].set_xlabel('Actual',fontsize =30)
axs[1].set_ylabel('Predicted',fontsize =30)

sns.heatmap(nn_cmat,annot=True,ax=axs[2],cmap=color)
axs[2].set_title('Neural Network')
axs[2].set_xlabel('Actual',fontsize =30)
axs[2].set_ylabel('Predicted',fontsize =30)

plt.show()
```


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_115_0.jpg)
    



    <Figure size 720x720 with 0 Axes>


### Classification Report


```python
import numpy as np
import seaborn as sns
from sklearn.metrics import classification_report
import pandas as pd
```


```python
true = y_test
target_names = list(['negative','neutral','positive'])
```


```python
lr_clf_report = classification_report(true,
                                   lr_pred,
                                   target_names=target_names,
                                   output_dict=True)

svm_clf_report = classification_report(true,
                                   svm_pred,
                                   target_names=target_names,
                                   output_dict=True)

nn_clf_report = classification_report(true,
                                   nn_pred,
                                   target_names=target_names,
                                   output_dict=True)
```


```python
fig, axs = plt.subplots(1,3)
plt.figure(figsize=(14,10))
fig.set_size_inches(30, 10)

fig.suptitle("Prediction Model Classification Report")

sns.set(font_scale=2)
color ='vlag'

sns.heatmap(pd.DataFrame(lr_clf_report).iloc[:-1, :].T, annot=True,ax=axs[0],cmap=color)
axs[0].set_title('Logistic Regression')
axs[0].tick_params(labelrotation=45,axis='y')


sns.heatmap(pd.DataFrame(svm_clf_report).iloc[:-1, :].T, annot=True,ax=axs[1],cmap=color)
axs[1].set_title('Support Vector machine')
axs[1].tick_params(labelrotation=45,axis='y')


sns.heatmap(pd.DataFrame(nn_clf_report).iloc[:-1, :].T, annot=True,ax=axs[2],cmap=color)
axs[2].set_title('Neural Network')
axs[2].tick_params(labelrotation=45,axis='y')

plt.show()
```


    
![jpg](https://raw.githubusercontent.com/ameerhaziq20/ameerhaziq20.github.io/main/_projects/Tokyo%202020%20Olympics%20Sentiment%20Analysis%20from%20Twitter/output_120_0.jpg)
    



    <Figure size 1008x720 with 0 Axes>


