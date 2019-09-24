# Predicting_Stock_With_Twitter_Sentiment

READ ME: Tesla Twitter Sentiment


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

# Senitment Analysis on Tweets

Two methods of computing sentiment score on each tweet were utilized; Vader and TextBlob.

![Vader_sentiment_distribution.png](http://localhost:8888/view/Images/Vader_sentiment_distribution.png)

![TextBlob_sentiment_distribution.png](http://localhost:8888/view/Images/TextBlob_sentiment_distribution.png)

![Top_days_vader.png](http://localhost:8888/view/Images/Top_days_vader.png)

![Top_days_textblob.png](http://localhost:8888/view/Images/Top_days_textblob.png)

![Worst_days_vader.png](http://localhost:8888/view/Images/Worst_days_vader.png)

![Worst_days_textblob.png](http://localhost:8888/view/Images/Worst_days_textblob.png)

The goal of this project is to be able to compute a daily sentiment score before the market closes and be able to predict if the stock will open at a higher price the next day. The stock market is only open from 9:30 am to 4:00 pm, and in order to make a model that could be used in real life situations, we would only be able to use the tweets prior to 4:00 pm to compute a sentiment score. In order to give some time to run the model the model and buy the actual stock on a trading platform like Robinhood, tweets after 3:55 pm were dropped from the dataset.

## Feature Engineering

After dropping all tweets published after 3:55 pm, four more sentiment columns were added to the dataset:

 - Drop all tweets that had a Vader sentiment score of 0 and then recalculate the daily average (__s1_no_0__)
 - Drop all tweets that had a Vader sentiment score of 0 and then recalculate the daily average (__s2_no_0__)
 - Rescaled all the Vader sentiment scores for each tweet with __MinMaxScaler()__ and then recalculate the daily average (__s1_scaled__)
  - Rescaled all the TextBlob sentiment scores for each tweet with __MinMaxScaler()__ and then recalculate the daily average (__s2_scaled__)


# Results

After running the models it was clear that this was best kept as a classification problem; __Will the stock open tomorrow at a higher price than today's close?__

Using a SARIMAX model each sentiment feauture was used as an exogenous factor to predict the difference between the opening price and the prior day's closing price (open_close_diff) and whether that difference would be positive or negative (pos_neg).

## The Best Model

Model: ARIMAX(open_close_diff, sentiment_1), order =(3,1,4)

Predict that the stock will open tomorrow at a lower price than today’s closing price: 
Precision: 0.47457627
Predict that the stock will open tomorrow at the same price as today’s closing price:
Precision: 0.0
Predict that the stock will open tomorrow at a higher price than today’s closing price:
Precision: 0.54054054

Out of the 37 times the model predicted that there would be an increase from one day’s closing price and the following day’s opening price, it was correct 20 times


If you were to collect tweets using TWINT containing the words ‘tesla’ and ‘tsla’ from 12:00 AM and 3:55 PM and use Vader to compute a daily sentiment score and use an ARIMAX model, you could determine if the price of the stock will open at a higher price than the close price with 54% precision.  If you were to buy the stock at about closing price right before the market closes 100 times, you would make a profit 54 times.