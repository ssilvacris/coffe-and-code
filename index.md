# Words Most Mentioned in New York Times in 2020


This project aims to evaluate the headlines of the New York newspaper between January and July 2020 and see how the words like COVID and Virus have taken on relevance over these months. 

According to WHO, coronavirus was first reported in Wuhan (China) on December 31, 2020.

On March 11, 2020, WHO declared COVID a pandemic. The COVID-19 related timeline can be checked [here](https://www.who.int/news-room/detail/29-06-2020-covidtimeline).

It's possible to imagine the words _Coronavirus_, _virus_ and _pandemic_ have been most cited since January until July 2020. But how many others were relevant in the same period?  How do we can analyze them?

To respond to these questions, I have chosen one source and two tools:

- Source: New York Times newspaper to select the headlines of the articles from January to July.
- Tools: The Natural Language Toolkit (**NLTK**), a **Python** library for handling natural language processing (**NLP**), and **Tableau**, a visual analytics platform.



## Getting the newspaper headlines 

First, I needed to create a database with newspaper headlines. Among some ways to do this, I chose the new york times because of the importance of their articles, their coverage during the pandemic, as new york was among the most affected cities in the world, and also because of its articles search API.

APIs (application programming interfaces) can be used to programmatically access New York Times data. First of all, it is necessary to create an API key and with it have access to search for files since 1851, a list of best sellers, movie reviews, among others. You can see this site [here](https://developer.nytimes.com)

I created my API key and my goal was to get the machetes for the specified period and with that, I created my database that looked like this:

![Image of table](https://github.com/ssilvacris/coffee-and-code/blob/master/headline-articles.png)

The figure above shows just a few lines from the database that runs through July.


## Getting the Most Cited Words

The Natural Language Toolkit (NLTK) is a Python library for handling natural language processing (NLP).

### Libraries and Dataset
```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import nltk
```


```python
months_df = pd.read_csv('months.csv', parse_dates=True, index_col='date')
months_df.head()
```

```python
months_df.info()
```

### Installing NLTK modules
```python
nltk.download()
```

## Exploratory Data Analysis (EDA)

I need analyse and clean all the headline.
First, I will join each headline in one string.
Then the string will be  splitted into a list where each word is a list item.

```python
# join the strings of every row
sentences_jan = jan_df['headline'].str.cat(sep = '')
sentences_jan
```

### Tokenization

```python
from nltk.tokenize import word_tokenize

tok_jan = word_tokenize(sentences_jan)
tok_jan
```

### Punctuation and Stopwords Removal

from nltk.corpus import stopwords


```python
# remove stopwords
stop = stopwords.words('english')
tok_jan = [token for token in tok_jan if token not in stop]

# remove words less than three letters
tok_jan = [word for word in tok_jan if len(word) >= 3]
```

### See the most cited words of each month

```python
plt.figure(figsize=(20,20))


wc = WordCloud(max_font_size=50, max_words=100, background_color="white")
wordcloud_jan = wc.generate_from_text(' '.join(tok_jan))

plt.imshow(wordcloud_jan, interpolation="bilinear")
plt.axis("off")
plt.show()
```


January                    |  February                 
:-------------------------:|:-------------------------:
![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_jan.png)  |  ![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_feb.png)|


March                      |  April 
:-------------------------:|:-------------------------:
![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_mar.png)  |  ![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_apr.png)


May                        |  June 
:-------------------------:|:-------------------------:
![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_may.png)  |  ![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_jun.png)

July                       |   
:-------------------------:|
![](https://github.com/ssilvacris/coffee-and-code/blob/master/wordcloud_jul.png)  |  


### Getting the frequency distribution of the words. 
