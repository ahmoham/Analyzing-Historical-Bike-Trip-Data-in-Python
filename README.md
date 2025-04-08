# Analyzing Historical Bike Trip Data in Python (Google Case Study)

# INTRODUCTION

This case study is the Capstone Project of the **Google Data Analytics Professional Certificate**. The **6 steps of Data Analysis** are used to present this analysis. These steps include **Ask**, **Prepare**, **Process**, **Analyze**, **Share**, and **Act**. 

Title: **Cyclistic Case Study**

Author: Abdullahi Mohamed

Date: 3 April 2025


![](cyclistic.png)

## Cyclistic: How Does a Bike-Share Navigate Speedy Success?

## STEP 1: Ask

####  1.0 Background
Cyclistic is a bike-share company located in Chicago. Since 2016, Cyclistic has grown to a fleet of over 5000 bicycles located across over 600 stations. The bikes can be unlocked from one station and returned to any other station in the system anytime. 
The director of marketing, Lily Moreno, believes that an analysis of Cyclistic historical bike trip data would reveal key insights for future revenue growth. 

#### 1.2 Business Task

Goal: Analyze Cyclistic historical bike trip data to gain insights on how casual riders and annual members use bikes differently and support the marketing strategy aimed at converting casual riders to annual members. 

#### 1.3 Business Objectives
1. How do annual members and casual riders use Cyclistic bikes dierently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

#### 1.4 Deliverables:
1. A clear statement of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top three recommendations based on your analysis

#### 1.5 Key Stakeholders
1. Lily Moreno: Director of Marketing 
2. Cyclistic marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Cyclistic's marketing strategy. 
3. Cyclistic executive team: A team of executives that will decide whether to approve the recommended marketing program. 
---

## STEP 2: Prepare

#### 2.1 Information on Data Source
The data used for analysis was collected by Cyclistic (through Motivate International Inc) and is located on their public website. The historical trip data was organized by month and year dating back to May 2022. The data was saved as CSV files within zip folders. Each file contained information on the following fields: 
1. ride_id: a unique ID per ride
2. rideable_type: the type of bicycle used
3. started_at: the date and time that the bicycle was checked out
4. ended_at: the date and time that the bicycle was checked in
5. start_station_name: the name of the station at the start of the trip
6. start_station_id: a unique identifier for the start station
7. end_station_name: the name of the station at the end of the trip
8. end_station_id : a unique identifier for the end station
9. start_lat: the latitude of the start station
10. start_lng: the longitude of the start station
11. end_lat: the latitude of the end station
12. end_lng: the longitude of the end station
13. member_casual: a field indicating whether the rider is a causal or member

#### 2.2 Limitations of Dataset
1. The data is collected from the May 2022 to April 2023. Since this data is over 3 years old, rider's habits and trip frequency may have changed. The data set may not be timely and relevant to find accurate reccommendations for the marketing team's campaign.

#### 2.3 Is Data ROCCC?
A good data source is **ROCCC** which stands for **R**eliable, **O**riginal, **C**omprehensive, **C**urrent, and **C**ited. 
1. Reliable - LOW - Data is collected from a third party (Motivate International Inc), therefore reliability is unknown.
2. Original - LOW - Datasets are provided by a third-party.
3. Comprehensive - HIGH - Variables can be used to answer the business task.
4. Current - LOW - Data is 3 years old and is not relevant. 
5. Cited - LOW - Unknown since the data is collected by a third-party.

Overall, the dataset is considered bad quality and any insights gathered from this data should not be used to produce business recommendations. 

#### 2.4 Data Selection:

The following file is selected and copied for analysis.   

   - biketrips_merged.csv

---

## STEP 3: Process

#### 3.1 Preparing the Environment
The `numPy, Pandas, matplotlib, datetime` packages are imported and aliased. 

#import packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt 
import datetime as dt 

#### 3.2 Importing dataset
Reading in the selected file.


#read_csv fucntion to read the required CSV file
bike_trips = pd.read_csv(r'C:\Users\abdul\Downloads\Cyclistic Datasets\CSV\biketrips_merged.csv')

#### 3.3 Data cleaning and manipulation

**Steps**
1. Explore the data
2. Check for missing or null values.
3. Remove duplicate data. 
3. Ensure the data is consistant and standardized. 

Previewing the data using `.head(10)`.

# Preview the first 10 rows of the data set
bike_trips.head(10)

Checking the datatype for our variables using `.info()`. 

bike_trips.info()

Dropping duplicate values from our data set using`.drop_duplicates()`.

bike_trips = bike_trips.drop_duplicates()

Checking for columns with missing values using `isnull().sum()`. 

missing_value_counts = bike_trips.isnull().sum()

missing_value_counts

Variables start_station_name, start_station_id, end_station_name, end_station_id, end_lat_ and end_lng are missing values. Since the size of our data set is large (> 5 million rows), removing rows with missing data will not have a significant impact on our data set. 

# Drop the rows with missings values using .dropna and create a new data set
bike_tripsclean = bike_trips.dropna().copy()

# Confirm that all null values have been removed
missing_value_counts = bike_tripsclean.isnull().sum()

print(missing_value_counts)

The datatype for `started_at` and `ended_at` are objects and need to be changed to datetime with `pd.to_datetime()`. 

# Changed the data type of started_at & ended_at from an object type to a date data type using pd.to_datetime.
bike_tripsclean['started_at'] = pd.to_datetime(bike_tripsclean['started_at'])
bike_tripsclean['ended_at'] = pd.to_datetime(bike_tripsclean['ended_at'])

bike_tripsclean.info()

Creating a trip duration variable from the difference between start time and the end time. Converted the trip duration variable to minutes. 

bike_tripsclean['trip_duration'] = round((bike_tripsclean['ended_at'] - bike_tripsclean['started_at']).dt.total_seconds() / 60, 3)

bike_tripsclean['trip_duration'].describe()

A negative minimum value for the trip duration suggests that an error in the `started_at` and `ended_at` variables for certain riders. Likewise, a maximum value of over 22 days suggests an error as well. To ensure that the data we are analyzing is accurate and consistent, all values with a `trip_duration` below 1 minute and above 24 hours will be removed from this data set. 

bike_tripsclean = bike_tripsclean[(bike_tripsclean['trip_duration'] > 1) & (bike_tripsclean['trip_duration'] < 1440)].copy()

bike_tripsclean['trip_duration'].describe()

Adding a `month` and `day_of_week` column to the data set. 

# Adding the month column by extracting from date variables

bike_tripsclean['month'] = bike_tripsclean['started_at'].dt.strftime('%B')

# Adding the day_of_week column by extracting from date variables
bike_tripsclean['day_of_week'] = bike_tripsclean['started_at'].dt.strftime('%A')

bike_tripsclean.head(10)

#### 3.4 Summary of data cleaning steps

1. Removed null values from data set using the .dropna fuction.
2. Removed duplicate rows. 
3. Changed the data type of started_at and ended_at variables from object to datetime using pd.ti_datetime. 
4. Added 3 new columns: trip_duration, month, and day_of_week. 
5. Removed rows with a trip_duration less than 1 minute and greater than 24 hours. 

## STEP 4: Analyze

#### Perform calculations

Finding the average trip duration for casual riders and members. 

# Average trip duration grouped my membership status. 

bike_tripsclean.groupby(['member_casual'])['trip_duration'].mean()

 Counting the number of riders by membership status. 

# Count of riders by membership status. 

bike_tripsclean['member_casual'].value_counts()

bike_tripsclean['member_casual'].value_counts(normalize=True) * 100

Count of total monthly rides by membership status.

# Count of total monthly rides by membership status. 

bike_tripsclean.groupby(['member_casual'])['month'].value_counts()

Count of total daily rides by membership status (Monday to Sunday). 

# Count of total daily rides by membership status. 

bike_tripsclean.groupby(['member_casual'])['day_of_week'].value_counts()

bike_tripsclean.groupby(['member_casual'])['month'].value_counts()

bike_tripsclean.groupby(['member_casual'])['rideable_type'].value_counts(normalize=True) * 100

## STEP 5: Share

### 5.1 Supporting Visualizations and Findings

Exporting the dataset into a csv file to load into Power BI desktop. 

# Exporting the datset into a csv file. 
bike_tripsclean.to_csv('bike_tripsclean', index=False)

![total%20trips%20by%20membership%20type.png](attachment:total%20trips%20by%20membership%20type.png)

#### Total Trips by Membership Type

1. Riders with an **annual membership** comprised **60.4%** of all trips between May 2022 and April 2023. 
2. **Casual** riders comprised **39.6%** of all trips between May 2022 and April 2023. 

![type%20of%20bikes%20.png](attachment:type%20of%20bikes%20.png)

#### Types of Bikes by Membership Type

1. The **classic bike** was the prefered bike for most riders followed by **electric bike** and **docked bike**. 
2. The **docked bike** was only used by casual riders. 
3. Members prefered to use the **classic bike** over the **electric bike** (1.72 million > 0.96 million). 

![average%20trip%20duration.png](attachment:average%20trip%20duration.png)

#### Average Trip Duration for Riders

1. The average trip duration for members and casual riders was **12.5** minutes and **23.4** minutes respectively. 
2. Casual riders take **less trips** than members, but have **longer trips** on average. 

![total%20monthly%20trips.png](attachment:total%20monthly%20trips.png)

#### Total Monthly Trips

1. The highest volume of trips were taken in the summer months (June, July, & August). As the weather improves, riders are more likely to use bikes for recreation, leisure, or comutting to work. 
2. Both casual riders and members tend to follow the same monthly trip trend with more rides in the summer and less rides in the winter. 

![total%20daily%20trips.png](attachment:total%20daily%20trips.png)

#### Total Daily Trips 

1. Casual riders are **more likely to use bikes during the weekend** (Saturday & Sunday) compared to the weekdays. This may suggest that casual riders are performing activities of leisure or recreation. 
2. Members are **more likely to use bikes during the weekday** (Monday - Friday) compared to the weekends. This may suggest that members are commuting to and from work or school. 

### 5.2 Dashboard
![cyclistic%20final%20dashboard-2.png](attachment:cyclistic%20final%20dashboard-2.png)

### STEP 6: Act

#### 6.1 Revist Business Objectives

**1. How do annual members and casual riders use Cyclistic bikes differently?**

Annual members typically use Cyclistic bikes during the weekday compared to casual riders that typically use bikes during the weekend. Using bikes during the weekday may suggest commuting to and from work or school. Bike use during the weekend may suggest recreational or leisure activities. Casual riders also had longer bike trips on average compared to annual members. Longer bike trips may support the suggestion of recreational or leisure activities as these riders are more flexible with the length of their trips. Annual members make up 60% of all Cyclistic bike riders compared to 40% for casual riders. 

**2. Why would casual riders buy Cyclistic annual memberships?**

Casual riders would buy Cyclistic annual memberships if they believe it fits well with their lifestlye. A barrier stopping casual riders from buying annual memberships is the 1 year commitment. Casual riders are more likely to use Cyclistic bikes on the weekends and in the summer months so a 1 year membership may not fit with their lifestyle. A suggestion would be to offer memberships at varying lengths to appeal to a broader section of casual riders. Introducing membership lengths of 1 month, 3 months, and 6 months may convert some casual riders to members and removed the barrier of a 1 year commitment. 

**3. How can Cyclistic use digital media to influence casual riders to become members?**

Cyclistic can use social media influencers to promote their annual memberships to their casual riders. Social media influencers have large followings and are great at promoting services to their fans. A campaign centered around social media influencers will likely increase the traffic and engagement of Cyclistic bikes. 

### 6.2 Recommendations 

Here are my top three recommendations for the marketing team to inform their next campaign: 

1. Offer membership at different lengths (1 month, 3 months, and 6 months) to convert more casual riders to members.
2. Partner with social media influencers to promote the annnual membership to casual riders.
3. Improve the membership experience. Create incentives for casual riders to become members. This can include a reward system or member-exclusive events. 
