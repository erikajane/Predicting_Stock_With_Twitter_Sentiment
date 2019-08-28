# Predicting_Stock_With_Twitter_Sentiment

## Introduction

![TWINT_code_to_collect_tweets](https://raw.githubusercontent.com/username/projectname/branch/path/to/img.png)

READ ME: Tesla Twitter Sentiment

Project Overview

Intro:
Can the stock market be predicted?
Relationship between stocks a public sentiment
Relationship between public sentiment and twitter
What question is this project trying to answer:
Can we predict if the Tesla stock price will be higher at tomorrow’s opening than today’s close using sentiment from Twitter?

Basic Outline
Approached the questions as a binary classification problem with target variable as the direction of the price at opening price from the prior day’s close (-1 = Price of stock went down, 0 = Price of stock stayed the same, 1=Price of stock went up)
Obtain tweets from twitter from January 1st, 2018 to July 14th, 2019 that contained either world ‘tesla’ or ‘tsla’ using TWINT
Over 200,000 tweets were collected
Each tweet was cleaned, tokenized and lemmatized
Each tweet was given two sentiment scores, using Vader (Valence Aware Dictionary for Sentiment Reasoning) and TextBlob
After some EDA, tweets from after 3:55 PM were removed from the dataset
The tweets from each day were then aggregated to find a daily mean sentiment score for the day, one daily sentiment score using Vader and one using TextBlob
After feature engineering, four more variables were created, Vader daily sentiment was recalculated after removing all the tweets with 0 sentiment as well as for TextBlob, the other two were features that had turned the sentiment scores into classes (-1=Negative daily sentiment score, 0=Neutral daily sentiment score, +1=Positive daily sentiment score)
Daily sentiment scores were then merged with historical stock data from Yahoo Finance
Friday sentiment scores were than aggregated so they were the average of Friday, Saturday and Sunday sentiment scores
Removed all days without stock data
Feature engineering: Took the day’s opening price and subtracted the prior day’s closing price as well as shifted the sentiment scores
Using a SARIMAX model, with Seasonal Order of (0,0,0,0), each of the sentiment scores were modeled with best operational orders to find the best model

# Collecting the Tweets

__TWINT__ was utilized to collect the tweets needed from Twitter. TWINT is an advanced Twitter scraping tool written in Python. TWINT allows you to search Twitter for tweets matching different search operators, scrape those tweets and save them into a file.

The search parameters used in this project, were as follows:
 - Two search terms, company name and company ticker (__tesla__ and __tsla__)
 - Start date of search (__01/01/2018__)
 - End date of search (__07/15/2019__
 - Tweets in the English language
 
![TWINT_code_to_collect_tweets.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/TWINT_code_to_collect_tweets.png) 

Each time this function was run the tweets were stored in a csv file, within a folder called __'tesla_tweets'__.

It was found that the shorter the time period the search was run for the more tweets would be collected. Since this was the case, each search was run over a two week period and then merged together to include 213,224 scraped tweets from January 1st, 2018 to July 14th, 2019 containing the words ‘telsa’ and/or ‘tsla’

# Cleaning the Twitter Data

Before obtaining the sentiment of each tweet, basic NLP preprossing steps were preformed. Http links were removed, special characters and numbers were removed, the tweets were converted to all lowercase strings and then each tweet was tokenized.

![Clean_and_tokenize_tweets.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Clean_and_tokenize_tweets.png)

Next, each tweet was lemmatized.

![Lemmatize_tweets.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Lemmatize_tweets.png)

![Word_cloud_1.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Word_cloud_1.png)

When looking at the word cloud after our cleaning, tokenizing and lemmatizing we can see that 'tesla', 'tsla', teslaq', '#' appear very heavily in the dataset. These terms were removed from the tweets and below appears the new word cloud.

![Word_cloud_2.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Word_cloud_2.png)

The below graph shows the top 25 words included in this dataset, with the first three being __"model", "elon"__ and __"musk__.

![Most_popular_words.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Most_popular_words.png)

The below graph shows that the most popular time to tweet about Tesla is between 9:00 am and 5:00 pm with 9:00 am being the most popular time to tweet.

![Tweets_by_time_of_day.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Tweets_by_time_of_day.png)

The below graph shows that people tend to tweet more about Tesla on weekdays that weekends, with Wednesday being the most popular time to tweet about Tesla.

![Tweets_by_day_of_week.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Tweets_by_day_of_week.png)

![Tweets_by_day_of_year.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Tweets_by_day_of_year.png)


# Results

After running the models it was clear that this was best kept as a classification problem; __will the stock open at a higher price than today's close?__

Using a SARIMAX model 

Predict that the stock will open tomorrow at a higher price than today’s closing price:
Precision: 0.54054054
Out of the 37 times the model predicted that there would be an increase from one day’s closing price and the following day’s opening price, it was correct 20 times


If you were to collect tweets using TWINT containing the words ‘tesla’ and ‘tsla’ from 12:00 AM and 3:55 PM and use Vader to compute a daily sentiment score and use an ARIMAX model, you could determine if the price of the stock will open at a higher price than the close price with 54% precision.  If you were to buy the stock at about closing price right before the market closes 100 times, you would make a profit 54 times.