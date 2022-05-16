# Bellabeat
This repository contains the capstone project for Google Data Analytics Specialization 

Hi!!
So in this particular data analysis expedition we are supposed to imagine Bellabeat to be a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market.

## Stakeholders

1. Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer
2. Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
3. Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy. You joined this team six months ago and have been busy learning about Bellabeat’’s mission and business goals — as well as how you, as a junior data analyst, can help Bellabeat achieve them.

## Products

1. Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.

2. Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.

3. Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.

4. Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.

5. Bellabeat membership: Bellabeat also offers a subscription-based membership program for users.
Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.

### Our analysis will be done in phases which will be- ask, prepare, process, analyze, share, and act.

## Ask

Some of the questions we might ask in order to get started on our analysis are:

1.What are some trends in smart device usage?
2.How could these trends apply to Bellabeat customers?
3.How could these trends help influence Bellabeat marketing strategy?

This leads us to identify our product in focus and business task we are focusing on.
The product we'll be focussing on will be the Bellabeat app as it tracks the data from all the other devices and will hence help us focus on the bigger picture.

Now for the **Business task**- Help identify ways in which we can improve customer engagement by analysing how the product helps them and where it's lacking.

## Prepare

Let's start with gathering the dataset.
[FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit): This Kaggle data set contains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits.

The data is organised in both long as well as wide formatwhich gives of detailed as well as overall insights of the data, though the data has some missing values which have to be dealt with.

Does the data ROCCC?
R-Reliable-the data is reliable as it has been obtained from a public domain
O-Original-since it has been directly taken from people
C-Comprehensive-the data covers most aspect of data and is not just random
C-Current-the data is recently made avilable i.e. an year ago
C-Cited-it has been uploaded on a legit platform
So yes the data can be considered a good data as it ROCCC.
We can download the data onto our local machines.

## Process

Now we need to process the data, for this I have used Bigquery, which means we need to upload the data by creating new dataset in bigquery and then adding our data as tables.
The data we have data has to be converted to “YYYY-MM-DD hh:mm:ss” format so that it could be uploaded to Bigquery as table, which we can do using spreadsheets.
So before uploading to Bigquery the data should be converted, another way of doing that will be by using date-time SQL commands.Either way is fine.. While we're in spreadsheets we can remove extra columns, empty fields and duplicate data. 
Now that the clean processed data sits in the Bigquery as tables, we can begin to analyse and discover some facts.

## Analyse
Let's begin analysing our data:--- 

1.There are many fitness trackers in the market, so what distinguishes these products from one another? One of the factors is accuracy. How do we determine the accuracy of our devices? One way to determine accuracy is by taking the difference in Total distance and tracker distance.
We name this difference as disterror

**query: **

    SELECT  id, TotalDistance-TrackerDistance as disterror
    FROM bellabeat-346910.dataset.dailyActivity_merged
    WHERE TotalDistance-TrackerDistance>0

The results are as follows:

|id         |disterror|
|-----------|---------|
|7007744171 |0.759999752|
|7007744171 |0.8100004196|
|7007744171 |1.159999847|
|7007744171 |0.4600000381|
|7007744171 |0.9799995422|
|6962181067 |0.03999996185|
|7007744171 |1.049999237|
|7007744171 |1.06000042|
|7007744171 |1.140000343|
|6962181067 |1.829999924|
|6962181067 |0.1900005341|
|7007744171 |1.069999695|
|7007744171 |1.159999847|
|7007744171 |0.9000005722|
|7007744171 |0.8800001144|

2. Not all available data can be used for analysis, checking consistency of the data is important, i.e. there are to equal number of entries present in each dataset to be considered.
query: 

    SELECT COUNT(*)
    FROM table_name
    
 This statement can be used to count numver of entries in each table.
 
 results:-
 
dailyActivity_merged-940
dailyActivity_merged-940
dailyIntensities_merged-940
dailySteps_merged-940
sleepDay_merged-413

There is a discrepancy in the number of entries.

3. One metabolic equivalent (MET) is defined as the amount of oxygen consumed while sitting at rest and is equal to 3.5 ml O2 per kg body weight x min.

Total METs of a person in a day.

query:-

    SELECT  DISTINCT Id,SUM(METs) as totalMETs
    FROM bellabeat-346910.dataset.minuteMETsNarrow_merged
    GROUP BY Id
    
results:-


|Id         |totalMETs|
|-----------|------|
|4057192912 |63640|
|8053475328 |750596|
|1644430081 |599294|
|4558609924 |677866|
|4319703577 |611761|
|2320127002 |580488|
|1503960366 |717081|
|8877689391 |866178|
|2347167796 |390497|
|4388161847 |722167|
|6775888955 |433411|
|5553957443 |635426|
|3372868164 |412708|
|7086361926 |700993|
|2873212765 |674989|
|6290855005 |526386|
|5577150313 |796171|
|4445114986 |588773|
|4020332650 |535746|
|6117666160 |583309|
|8378563200 |707056|
|8583815059 |566525|
|2026352035 |615451|
|7007744171 |611358|
|1927972279 |470228|
|2022484408 |747732|
|8792009665 |484899|
|6962181067 |684724|
|3977333714 |645253|
|4702921684 |656096|
|1844505072 |520968|
|1624580081 |552972|
|8253242879 |342043|

We can categorize this data into three categories light , moderate and heavy.

|Light         |Moderate     |Heavy   |
|--------------------|--------------------|--------------------|
|Sitting at a desk|Housework (cleaning, sweeping)|Walking at very brisk pace|
|Sitting, playing cards|Weight training (lighter weights)|Bicycling 12–14 mph (flat terrain)|
|Strolling at a slow pace|Brisk walking (3.5–4 mph)|Singles tennis|
|Standing at a desk|Golf (walking, pulling clubs)|Circuit training (minimal rest)|
|Washing dishes|Weight training (heavier weights)|Shoveling, digging ditches|
|Hatha yoga|Yard work (mowing, moderate effort)|Competitive soccer|
|Fishing (sitting)|Swimming laps (leisurely pace)|Running (7 mph)|

    INSERT INTO dataset.thetemptable
    SELECT  Id,
        CASE 
            WHEN METs<=12 THEN "Light"
            WHEN (12 < METs) and (METs <= 20) THEN "Moderate"
            ELSE "Heavy"
        END as category   
    FROM bellabeat-346910.dataset.minuteMETsNarrow_merged
    ORDER BY Id

4. We can find average sleeptime of a person.

query:-

    AVG sleeptime of a persoon in a day
    SELECT Id,avg(TotalMinutesAsleep)/60 avgsleephrs 
    FROM bellabeat-346910.dataset.sleepDay_merged
    GROUP BY Id
    
 results:-
 
 
| Id        |avgsleephrs|
|-----------|-----------|
|1503960366 |6|
|1644430081 |4.|9
|1844505072 |10.87|
|1927972279 |6.95|
|2026352035 |8.44|
|2320127002 |1.02|
|2347167796 |7.45|
|3977333714 |4.89|
|4020332650 |5.82|
|4319703577 |7.94|
|4388161847 |6.72|
|4445114986 |6.42|
|4558609924 |2.13|
|4702921684 |7.02|
|5553957443 |7.72|
|5577150313 |7.2|
|6117666160 |7.98|
|6775888955 |5.83|
|6962181067 |7.47|
|7007744171 |1.14|
|7086361926 |7.55|
|8053475328 |4.95|
|8378563200 |7.39|
|8792009665 |7.26|

we can see that the min. sleep time here is 1.02 hrs and the max sleep hours is 10.87 hrs.

## Share
 the next step in data analysis is sharing the findings.I used github as a platform to share my analysis. One can use other platforms such as kaggle, or websites etc to share.
 
 [Sleep hours visualization](https://github.com/dhanyamk2948/Bellabeat/blob/main/viz.png)
 
 
key findings are:---

1.Accuracy can be the product's USP.
2.Various video tutorials can be posted online as to how one can increase METs.
3.Daily, monthly half yearly and yearlt goals must be made available to the customers on the app, so that they can set goals fro themselves and achieve them like a particualr amount of sleep hours, or particular walking distance so that it ensures customer engagement on the app as well as device.

## Act

These insights can be shared with the business\marketing team and suitable decisions can be made.




 









 
 
 







 










