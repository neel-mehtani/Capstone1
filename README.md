# Beijing Air Quality Metrics

![alt text](/images/trump_clinton.png)


## Motivation

## Overview

This project is comprised of 3 parts.  

Part 1: Data Acquistion and Management

Part 2: Feature Integration and EDAs

Part 3: Hypotheses Tests

## Part 1

### The Data

The data was sourced from UCIs Beijing Multi-Site Air-Quality Data Archive repository. Overall, there were 12 data files, each containing pollutant and climatic data logs for every site used in compiling the data set, split by the hour of every day of a month for the 4 year time span that the dataset recorded. 

Once unzipped, the data was initially aggregated using PySpark modules to provide a basis by which a master dataset could be used to derive new subsets. However, this task was later replaced using Pandas as it provided similar functionality. Any hourly entry that contained N/A values across any of the data features was dropped to keep aggregation simple, without distorting the aggregated attributes by much as the each day contained several hourly records. Due to the chunky size of the data (~420,000 data instances for 12 sites), 6 sites were narrowed down based on their geographical proximity within different parts of Beijing. The chosen sites were: Dongsi, Wanliu, Tiantan, Gucheng, Huariou, and Shunyi.

As mentioned above, the master dataset from which newer relations could be tabulated was developed by splicing the data into different year groups for each site. The yearly categories for these sites were established by parsing in the string values of the day, month, and year of each individual record into datetime objects, which enabled splitting the datetime objects based on year they were acquired in. Thus, the master dataset was combined based on several years' information across various sites. 

With the primary information table being categorized by datetime, new categories such as seasonal months could also be implemented on the data to uncover new categorical relations to some pollution metrics. Multi-level categorization by year and season furthered the project's scope to look into how pollution metrics and standards varied for these parameters across different domains in Beijing based on the sites that were incorporated into the working master data-set, namely: central (Dongsi, Tiantan), mid (Gucheng, Wanliu), and outer-Beijing (Shunyi, Huariou). 

### Features and Attributes
Pollutant Data:  Physical evidence of various chemical pollutants collected from all sites across a 4 year span. A person who used a hashtag or phrase associated with a Clinton supporter.  Ex. "imwithher" - see `streamingclinton.py`

| Feature  |Attributes     |
| ------------- |:-------------:|
| CO | float|
| SO2| float|
| NO2 |float|
| PM 2.5 | float|
| PM 10|float|
| O3 |float|

Trump Supporter:  A person who used a hashtag or phrase associated with a Clinton supporter.  Ex. "makeamericagreatagain" - see `streamingtrump.py`
### Observations



Twitter data is incredibly messy and twitter users tend to use hashtags generously.  A hashtag is not always a good indication that a person is a Clinton or Trump supporter.

For the same time period I collected 5.70 GB of data for Trump and 1.34 GB of data for Clinton.

Twitter allows its users to write their own user descriptions.  The following wordclouds were produced from Clinton supporter's user descriptions and Trump supporter's user descriptions.  
![alt text](/images/combined_twitter.png)

Even though Clinton and Trump supporters have drastically different views, the way a person describes him/herself is surprisingly similar.  It is also surprising that with all the hateful election rhetoric floating around on Twitter that the most common used word in a user description is actually "love."




## Part 2

### The Data

#### Speeches and Debates
Campaign speeches and presidential debates were used in doing this analysis.

The speeches and debate transcripts were collected from a variety of sources:

All speeches collected were given in 2016.  

It is also important to note that the official Trump campaign site did not publish Trump's actual campaign speeches so all of Trump's speeches came from third party sources.


#### Tweets
Additionaly, I collected 3,200 (Twitter's max from the REST API) historical tweets from Clinton and Trump and saved the hashtags.  

#### User Tags
The screen names from Clinton tweeters and Trump tweeters were stored in a separate file from part 1.


### Wordclouds
The following wordclouds represent the most commonly used words in the campaign speeches.  The top two charts have no filtering and the bottom two charts only pull adjectives and nouns and eliminate the most commonly used words such as "people" and "American".
![alt text](/images/wordclouds.png)



### Clustering
I used TFIDF and an NMF model to create topic clusters.  Note that the colors do not have any specific meaning.  They were just meant to highlight topics of interest.
![alt text](/images/termiteplots.png)
Clinton spent a lot of time on the campaign trail targeting minority groups.  She spoke to many African American communities, women’s organizations and veterans, which you can see highlighted here. Trump’s topics are less defined but we can see some topics on the economy and national defense.

With a little more filtering Clinton’s topic change slightly, but are still clearly defined.  The topics are less clear for Trump. Immigration jumps out, but the other topics tend to fall apart.  This may be an indication of why the Trump campaign wouldn’t publish the actual transcripts.
![alt text](/images/termiteplots2.png)



### Twitter Bots
Based on the campaign speeches and debate transcripts, I created a Twitter bot for both Clinton and Trump.  The vocabulary for each candidate was constructed from the collected the collected speech and debate transcripts .

The tweet generation model uses the Markov model where the following word is based on the preceding trigram.

Clinton's dictionary consists of 142,523 unique trigrams.
Trump's dictionary consists of 149,892 unique trigrams.


I then collected hashtags from Clinton's and Trump's historical tweets using Twitter's REST API.  I collected the most commonly used hashtags from each candidate and then randomly added one, two or none to the generated tweet.


 I started the debate by tweeting the other candidate online and then the bots tweeted back and forth to each other every 3 minutes.   Each tweet was stored in an SQL database and when that database was updated, the corresponding bot knew to tweet back and used the words from the most recent tweet as the input for the response tweet.

I then decided to do a social experiment. From the data collected in part 1, I randomly selected a user to tag.  My initial goal was to see how many followers I could obtain in the remaining week.  Unfortunately after I started going this, I was quickly suspended from using Twitter's API.


![alt text](/images/twitterwar.png)


https://twitter.com/KillaryHilton_
https://twitter.com/TonaldDrump___


## Part 3

### The Data
The textacy library has a corpus that contains over 11,000 congressional speeches (1996-2016) from 13 different congress people involved in the 2016 election. The congress people recommendations are limited to the following people:


| Republicans   |Democrats      | Independents |
| ------------- |:-------------:| -----:|
| Marco Rubio   | Bernie Sanders | Jim Webb |
| Ted Cruz    | Barack Obama     |    |
| Rand Paul | Hillary Clinton      |   |
| Mike Pence | Joe Biden    |     |
| Lindsey Graham |Lincoln Chafee   |    |
| John Kasich |       |     |
| Rick Santorum |      |     |   |

The model uses TFIDF and a Naive Bayes model to find the probability that specific keywords are from a particular party or candidate.  The model only focuses on nouns, verbs and adjectives.  I hope to add a sentiment component in the future.  

Example output:
![alt text](/images/website.png)


The web app was created through Flask and can be accessed at:  www.discovercongress.co
