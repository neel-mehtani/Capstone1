# Beijing Air Quality Metrics

![alt text](/beijing_wallpaper.jpg)


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
Pollutant Data:  Physical evidence of various chemical pollutants collected from all sites across a 4 year span. 

| Feature  |Type     |
| ------------- |:-------------:|
| CO |float|
| SO2 |float|
| NO2 |float|
| PM 2.5 |float|
| PM 10|float|
| O3 |float|

Meteorological Data: Recorded metrics of meteorological information from all sites across a 4 year span relating climatic properties to pollution.  

| Feature  |Type     |
| ------------- |:-------------:|
| WSPM |float|
| TEMP |float|
| DEWP |float|
| PRES |float|
| RAIN |float|


Computed Features: The following features were inserted either into the master dataset or in subset tables based on either quick numerical calculation or categorization. 

| Feature  |Type     |
| ------------- |:-------------:|
| date |datetime|
| AQI PM2.5| float|
| AQI PM10 |float|
| AQI Level | string |
| Season | string|
| Climate | string |
| Wind | string |

### Feature Relations

Due to the vast number of data features that the dataset handles, the initial step was to identify some of the underlying correlations between the data features. This was better understood by implementing Seaborn's .heatmap() visualization tool to identify the correlation coefficients between the various metrics. 

![alt text](/cap1_heatmap.png)

The heatmap matrix clearly indicates highest positive correlations between pollutant data features as opposed to the climatic data features, serving as an initial pointer to some relevant data insights. 


## Part 2

### Feature Integration and EDAs

#### AQI Indices and Categories

The most prevalent computed-data metric in this project was the air quality index (AQI). The AQI is a standard pollution level metric measured on the basis of the concentration of particulate matter pollutants, namely PM 2.5 and PM 10. These indices were calculated for everyday of air quality recorded across all sites. AQI index values fall into specific pollution level standards based on the numerical range in which the index value falls. The computation and categorization are as follows: 

![alt text](/aqi_equation.png)

![alt text](/aqi_categories.png)

Using the yearly definitions of data entries from the master dataset, the AQI PM2.5 distribution was tracked over the years, between 2013-2017. 

![alt text](/aqi_boxplots.png)

On a finer level, the AQI index level trends for each site were also captured on a yearly basis on a trend-line graph, so as to gain an understanding of how these box distributions were constituted from the individual sites used for metric analysis. 
Following from the initial heatmap relational matrix, the correlation between CO and the AQI PM2.5 was tracked over the years to see if there was a prevalent change in pollutant correlation in tandem with the AQI indices trends that each site exhibited.

![alt text](/cap1_graphs1.png)

#### Tweets
Additionaly, I collected 3,200 (Twitter's max from the REST API) historical tweets from Clinton and Trump and saved the hashtags.  

#### User Tags
The screen names from Clinton tweeters and Trump tweeters were stored in a separate file from part 1.

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
