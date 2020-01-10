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

Following this, the site level trends were combined into distinct domains of Outer, Mid, and Central Beijing. The average % change in AQI PM2.5 levels were computed and their trends were followed to capture how AQI levels varied over time based on distinct domain categorizations and see how these related to the overall picture. 

![alt text](/aqi_domain_changes.png)

Furthermore, the domain categorization not only allowed a depiction of how AQI indices changed relatively over the years, but indeed allowed us to track how the frequency of various standards of air quality changed in each of these regions over the seasons each year. 

![alt text](/pollcount1_domain.png)

![alt text](/pollcount2_domain.png)

![alt text](/pollcount3_domain.png)

![alt text](/pollcount4_domain.png)

## Part 3

### Hypothesis Testing

#### Variation in Pollution Standard Frequency

With my data categorized by the various seasons for each year, the data could further be parsed into categories of cold months [Fall and Winter seasons] and warmer months [Summer and Spring seasons]. Under these segments, the counts of days of different pollution standards could be compiled and examined against one another.

A simple hypothesis that could be postulated against this data would be that there is no difference in the frequency of 'Good' (i.e. safe) air quality days between the cold and warm months. This requires a simple two sample test of proportion of good days in each sample category. Therefore, the null and alt hypotheses would be:

Null Hyp (H0): cold['good count'] = warm['good count'] 

Alt Hyp (Ha): cold['good count'] != warm['good count']


After averaging the numbers from all seasons for all years, the basis of the hypothesis test was constructed off of these numbers:

![alt text](/hyptest1_numbers.png)

shared_sample_var = 0.0010456477313459237

shared_sample_freq = 0.09516616314199396

good_day_difference = 0.02846870643480813

p_value = 1 - difference_in_proportions.cdf(good_day_difference)
p_value = 0.189

The rejection threshold (alpha): 0.05. 

Since the p_value > 0.05, we fail to reject the H0 (null hyp). Therefore we say that there is statistical evidence to show a relevant difference in the frequency of good days between cold and warm months. 






