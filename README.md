# Predicting_Stock_With_Twitter_Sentiment

READ ME: Tesla Twitter Sentiment


# Collecting the Tweets

__TWINT__ was utilized to collect the tweets needed from Twitter. TWINT is an advanced Twitter scraping tool written in Python. TWINT allows you to search Twitter for tweets matching different search operators, scrape those tweets and save them into a file. More information on TWINT can be found here https://github.com/twintproject/twint.

The search parameters used in this project, were as follows:
 - Two search terms, company name and company ticker (__tesla__ and __tsla__)
 - Start date of search (__01/01/2018__)
 - End date of search (__07/15/2019__
 - Tweets in the English language
 
![TWINT_code_to_collect_tweets.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/TWINT_code_to_collect_tweets.png) 

Each time this function was run the tweets were stored in a csv file, within a folder called __'tesla_tweets'__.

It was found that the shorter the time period the search was run for, the more tweets would be collected. Since this was the case, each search was run over a two week period and then merged together to include 213,224 scraped tweets from January 1st, 2018 to July 14th, 2019 containing the words ‘telsa’ and/or ‘tsla’

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

The below graph shows that people tend to tweet more about Tesla on weekdays than weekends, with Wednesday being the most popular time to tweet about Tesla.

![Tweets_by_day_of_week.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Tweets_by_day_of_week.png)

![Tweets_by_day_of_year.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Tweets_by_day_of_year.png)

# Senitment Analysis on Tweets

Two methods of computing sentiment score on each tweet were utilized; Vader and TextBlob.

![Vader_sentiment_distribution.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Vader_sentiment_distribution.png)

![TextBlob_sentiment_distribution.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/TextBlob_sentiment_distribution.png)

![Top_days_vader.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Top_days_vader.png)

![Top_days_textblob.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Top_days_textblob.png)

![Worst_days_vader.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Worst_days_vader.png)

![Worst_days_textblob.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Worst_days_textblob.png)

The goal of this project is to be able to compute a daily sentiment score before the market closes and be able to predict if the stock will open at a higher price the next day. The stock market is only open from 9:30 am to 4:00 pm EST, and in order to make a model that could be used in real life situations, we would only be able to use the tweets prior to 4:00 pm to compute a sentiment score. In order to give some time to run the model the model and buy the actual stock on a trading platform like Robinhood, tweets after 3:55 pm were dropped from the dataset.

## Feature Engineering

After dropping all tweets published after 3:55 pm, four more sentiment columns were added to the dataset:

 - Drop all tweets that had a Vader sentiment score of 0 and then recalculate the daily average (__s1_no_0__)
 - Drop all tweets that had a Vader sentiment score of 0 and then recalculate the daily average (__s2_no_0__)
 - Rescaled all the Vader sentiment scores for each tweet with __MinMaxScaler()__ and then recalculate the daily average (__s1_scaled__)
  - Rescaled all the TextBlob sentiment scores for each tweet with __MinMaxScaler()__ and then recalculate the daily average (__s2_scaled__)
  
 # Modeling
 
Models were created to investigate two target variables; The difference between the opening price and the prior day's closing price (__open_close_diff__) and whether that difference would be negative or positive (__pos_neg__).

The time series for each individual target variable can be seen below:
 
 ![open_close_diff_timeseries.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/open_close_diff_timeseries.png)
 
 __Features of the plot__:
 - There appears to be no consistent trend throughout the time span. The mean is very close to zero and we see the data frequently crosses the mean line instead of staying on one side for that long.
 - There does not appear to be any seasonality within this time series.
 - There appears to be a few outliers, most noticably around October 2018.
 - Variance appears to be constant, despite a few outliers.
 - A Dickey-Fuller Test was used to confirm that this time series is stationary.
 
 ![pos_neg_timeseries.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/pos_neg_timeseries.png)
 
 __Features of the plot__: 
 - This graph is the same graph as the previous one, but instead of dollar amounts for the change in open and prior close stock price, only the direction is taken into account (+1 for a positive change, -1 for a negative change and 0 for a neutral change). This makes the graph a bit harder to interpret.
 - Although difficult to determine from this graph, there appears to be no consistent trend throughout the time span.
 - There does not appear to be any seasonality within this time series.
 - Due to the nature of this time series after classifying directions, outliers cannot be determined.
 - Variance appears to be constant.
 - A Dickey-Fuller Test was used to confirm that this time series is stationary.

In order to model each of the two target variables (__open_close_diff__) aand (__pos_neg__) against each exogenous sentiment variable, the blow function was created.

The first part of the function splits the data into a training and testing dataset. Each model will be trained off of all the data from January 5, 2018 to February 25, 2019.

![Train_test_split _code.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Train_test_split%20_code.png)

Next, in order to find the appropriate model the best values for p, d and q must be evaluated. This function finds the best p, q and d values by determining which combination of these values will produce the smallest Mean Absolute Error while still producing a model that has a p value less than 0.1.

![pqd_test.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/pqd_test.png)

Once the final order has been determined, the final model is able to be produced.

Predicitions are then made for the target variable using the selected exogenous variable.

![Model_and_predict.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Model_and_predict.png)

In order to view how our predicitions compare to the actual values, a dataframe is created with the actual and predicted values.

![Create_prediction_df.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Create_prediction_df.png)

When examining the already classified target variable (__pos_neg__), the prediction values are continous values from -1 to 1. In order to get classified predicition values, they are then reclassified as follows:
     - Predicition value < 0, becomes -1
     - Predicition value = 0, becomes 0
     - Prediction value > 0, becomes 1

After reclassifying the prediction values the precision of how the model classified each class is printed as well as the confusion matrix.

When modeling the raw, unclassified target variable (__open_close_diff__), the predicted values produced had all been very close to 0, even when the exogenous variable had been scaled (__s1_scaled__ & __s2_scaled__) and when the exogenous variable had all tweets with sentiment score 0 removed (__s1_no_0__ & __s2_no_0__). In order to usable predictions, the actual __open_close_diff__ values were classified as follows:
     - Actual value < 0, becomes -1
     - Actual value = 0, becomes 0
     - Actual value > 0, becomes 1
The prediction values were also reclassified:
     - Predicition value < 0, becomes -1
     - Predicition value = 0, becomes 0
     - Prediction value > 0, becomes 1
     
![Reclassification_code.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Reclassification_code.png)

Although the model will be created using the actual open and closing differences, the results will be readable in the same way that the results for the target variable __pos_neg__ are.

After recalssifying the actua and predicted values, the precision for each class and confusion matrix for each class can be viewed.


# Results

After running the models it was clear that this was best kept as a classification problem; __Will the stock open tomorrow at a higher price than today's close?__

Using a SARIMAX model each sentiment feauture was used as an exogenous factor to predict the difference between the opening price and the prior day's closing price (open_close_diff) and whether that difference would be positive or negative (pos_neg).

## The Best Model

Best Target Feature: __open_close_diff__
Best Exogneous Feature: __sentiment_1__
Best Model: __ARIMAX(open_close_diff, sentiment_1), order =(3,1,4)__

Predict that the stock will open tomorrow at a lower price than today’s closing price: 
Precision: 0.47457627

Predict that the stock will open tomorrow at the same price as today’s closing price:
Precision: 0.0

__Predict that the stock will open tomorrow at a higher price than today’s closing price:
Precision: 0.54054054__

![Final_confusion_matrix.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Final_confusion_matrix.png)

Out of the 37 times the model predicted that there would be an increase from one day’s closing price and the following day’s opening price, it was correct 20 times

If you were to collect tweets using TWINT containing the words ‘tesla’ and ‘tsla’ from 12:00 AM and 3:55 PM and use Vader to compute a daily sentiment score and use an ARIMAX model, you could determine if the price of the stock will open at a higher price than the close price with 54% precision.  If you were to buy the stock at about closing price right before the market closes 100 times, you would make a profit 54 times.

## Final Model Adjustments

After reviewing predictions from all models, it is clear that there is some form of bias to each model, either they are over predicting positive values or they are over predicting negative values. In the final model, the model is over-predicting days when the __open_close_diff__ is negative. 

To adjust for this, the final model was re-run but when classifying the predicted values to [-1,0,1] in the original model, the predicted values were classified as follows:
     - Predicition value < 0, becomes -1
     - Predicition value = 0, becomes 0
     - Prediction value > 0, becomes 1
 
The model was re-run but to adjust for over predicting negative values, the cutoff of the classification was tested as seen in the code below:

![Code_for_adjusted_cut_off.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Code_for_adjusted_cut_off.png)

The final classification rule was as follows:
     - Predicition value < -.3, becomes -1
     - Predicition value = -.3, becomes 0
     - Prediction value > -.3, becomes 1
     
 This adjustment produced higher precision values.
 
 ![Adjusted_final_confusion_matrix.png](https://github.com/erikajane/Predicting_Stock_With_Twitter_Sentiment/blob/master/Images/Adjusted_final_confusion_matrix.png)
 
 __The final prediction that the stock will open tomorrow at a higher price than today’s closing price:
Precision: 0.58823529__
 