# NLP_course_project
## 1	Problem
The goal is to do the sentiment analysis on the tweet of the Italian population has regarding the various vaccines on the market against Covid-19. In particular, the vaccines that have been taken into account in this analysis are those approved to date by the EMA (Pfizer,  Astrazeneca, Moderna) plus Sputnik.
It was also decided to consider the Sputnik (Russian) vaccine to observe whether the sentiment differs significantly in comparison to the approved vaccines of Italy and the EU.
The data was taken from the social network Twitter some with a developer account allows you to download the tweets and data associated with them quite easily.
To identify the Italian population it was decided to assume that Italian users are all those who have written a tweet in Italian and that have as their place of profile an Italian city or the Italian state.
From this analysis we expect most tweets to be marked with negative sentiment (like 70%). This is because normally people tend more to complain on social networks than to express positive concepts. In addition, complaints are increasingly resonant and affect the user who is therefore more likely to share them.

## 2	Datasets
The data that has been used all comes from the Social Network Twitter. This is because such a social network allows you to download tweets and other data, which we will see later, easily once you have a developer account.
To download the data it was necessary to get hold of the various tokens and secret keys (accesstoken, accesstokensecret, apikey, apisecretkey) that are made available once the developer account is obtained. When this is done you have used the Python Tweepy library which easily allows you to download the data. In particular, to make the work even easier, we used the function "Cursor " of the library, which automatically downloads tweets according to the search criteria used (the programmer does not have to deal with the tweets pagination).
The search criteria used to download the tweets we are interested in are:
data_since = (start date of research) set at 18/04/2021
data_until = (research end date) set at 28/04/2021
language = set to 'it'
search_word = (keyword) set independently of the vaccine under consideration
As mentioned above we have not only downloaded the text of the tweet, but many other data so as to make, even future analyses, much more complete and consistent. In particular, the lables are:
[ tweets , is_quoted, tweet_quoted,  name_of_who_I_answered  , id user_str,  lang_user,  creation_date_of_tweet,  result_type,  number_of_retweet,  result_type,  number_of_retweet,  retweeted,  source,  user_name,  user_screen_name,  location,  user_description,  number_of_follower,  number_of_friend,  is_verified,  creation_date_of_account,  like  ]
Once all this data was downloaded, we moved on to cleaning it.
## 3	Pre-processing
The process of pre-processing the text is extremely important in order to make documents less subject to noise and variance.
For this project it was decided to apply two types of pre-processing, one hard and one soft. This is done because not all the algorithms that we are going to use will need a hard pre-processing.
Before applied pre-processing, it was necessary to delete the lines that had the same tweets. This is done because many people simply retweeted without any comment. So in our dataset there were many lines with the same tweet maybe retweeted by two or more different users. This decision was dictated by two factors: the first is that we did not want to assume that: if a person retweets a tweet we assume that he thinks in a equal way to the text that he retweeted. The second motivation concerns the computational cost, in fact working and processing a huge amount of tweets was heavy and extremely time-consuming.
So once you deleted all the rows that had the exact same tweet, you could proceed with the pre-processing bellow. 
### 3.1	Hard preprocessing
For hard pre-processing it was decided to delete all user names, hashtags,  links and special characters and all numbers were transformed into 0 using regex. Before deleting it, however, this data was saved in special columns which were used during analysis paragraph.
In addition, the sentence was also processed by placing it in  lower case and each word  was reduced to its lemma. POS parsing was used to hold words that had the following attributes {'NOUN', 'VERB', 'ADJ', 'ADV', 'PROPN'}. 
### 3.2	Soft pre-processing
As for soft pre-processing, it was decided to delete only the links and special characters. Each username has been replaced with the word "user".
It was decided, in contrast to hard pre-processing, to keep hashtags without the special character "#" assuming that during the training of algorithms this more data could help in a good prediction or a better understanding of the text.
### 3.3	Other types of pre-processing
For the analyzes that we will see later it was also necessary to clean up the column concerning the location of the various user profiles. This is because there were names that indicated the same place, but written differently (e.g. "Milano","Milan", "Milano, Lombardia", "Milano, Italia" , all these combinations were collapsed in the word "Milano"). In this case, special regular expressions were created for each main city. Creating a regular expression for each city would have been too onerous and even without much sense because very few tweets were collected in non-main cities (as we will see in the analysis part).
Another pre-processing that needed to be done was on the artificially created column used to save the hashtags. In fact, with them were also saved the special characters not useful and not significant for the future analysis. So always through regex these special characters have been deleted.

## 4	Sentiment analysis  with BERT
As the first model I decided to use is the model based on BERT built by Bocconi University (feel-it). This model has the peculiarity of being pre-trained on data in Italian and therefore usable on the tweets I downloaded.
This model had the task of doing the sentiment analysis on the text of the clean tweets with the soft  pre-processing procedure. In fact, this neural network was created to understand natural language and therefore there was no need to do hard pre-processing to decrease noise and variance. 
The first model used was this not only to get a general idea of sentiment in the various tweets, but also to use its predictions as the true labels for other models. In fact, the predictions provided by the Bocconi model have been used as a labels for all the models that we will see later.
For analysis purposes and to make a check on the predictions of sentiment I also used the analysis of emotion made available always by the same model.

## 5	Analysis
At this point we have the dataset that has two more columns: one for sentiments and one for emotions.
First we decided to see which words appear the most through a "WordCloud" representation. (In the representation " WordCloud" stopwords have not been taken into account).

