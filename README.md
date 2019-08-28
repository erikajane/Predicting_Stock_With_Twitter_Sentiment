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
 - Two search terms, company name and company ticker (__tesla__ and __tsla)
 - Start date of search
 - End date of search
 - Tweets in the English language
 
![TWINT_code_to_collect_tweets.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/TWINT_code_to_collect_tweets.png) 

were a Start Date, an End Date, and two search terms, in this case the company name and the company ticker. ‘2018-01-01’ was used as a Start Date and ‘07-14-2019’ was used for an End Date. The two search terms we wanted to search for were company name, ‘telsa’, and company ticker, ‘tsla’. In the function  ‘get_tweets’, it is clarified to only scrape Tweets that were in English.

After the below code was run, a csv file was saved containing 213,224 scraped tweets from January 1st, 2018 and July 14th, 2019 containing the words ‘telsa’ and/or ‘tsla’

# Results

After running the models it was clear that this was best kept as a classification problem; __will the stock open at a higher price than today's close?__

Using a SARIMAX model 

Predict that the stock will open tomorrow at a higher price than today’s closing price:
Precision: 0.54054054
Out of the 37 times the model predicted that there would be an increase from one day’s closing price and the following day’s opening price, it was correct 20 times


If you were to collect tweets using TWINT containing the words ‘tesla’ and ‘tsla’ from 12:00 AM and 3:55 PM and use Vader to compute a daily sentiment score and use an ARIMAX model, you could determine if the price of the stock will open at a higher price than the close price with 54% precision.  If you were to buy the stock at about closing price right before the market closes 100 times, you would make a profit 54 times.