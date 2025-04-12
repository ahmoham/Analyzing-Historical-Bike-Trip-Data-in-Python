# INTRODUCTION
This case study is the Capstone Project of the Google Data Analytics Professional Certificate. The 6 steps of Data Analysis are used to present this analysis. These steps include Ask, Prepare, Process, Analyze, Share, and Act.

**Title: Cyclistic Case Study**

**Author: Abdullahi Mohamed**

Date: 3 April 2025

![cyclistic](https://github.com/user-attachments/assets/541c27a5-3ebb-4708-9fb0-fb44dc32f986)


# Cyclistic: How Does a Bike-Share Navigate Speedy Success?
## STEP 1: Ask
### 1.0 Background
Cyclistic is a bike-share company located in Chicago. Since 2016, Cyclistic has grown to a fleet of over 5000 bicycles located across over 600 stations. The bikes can be unlocked from one station and returned to any other station in the system anytime. The director of marketing, Lily Moreno, believes that an analysis of Cyclistic historical bike trip data would reveal key insights for future revenue growth.

### 1.2 Business Task
Analyze Cyclistic historical bike trip data to gain insights on how casual riders and annual members use bikes differently and support the marketing strategy aimed at converting casual riders to annual members.

### 1.3 Business Objectives
1. How do annual members and casual riders use Cyclistic bikes dierently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?
### 1.4 Deliverables:
- A clear statement of the business task
- A description of all data sources used
- Documentation of any cleaning or manipulation of data
- A summary of your analysis
- Supporting visualizations and key findings
- Your top three recommendations based on your analysis
### 1.5 Key Stakeholders
- Lily Moreno: Director of Marketing
- Cyclistic marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Cyclistic's marketing strategy.
- Cyclistic executive team: A team of executives that will decide whether to approve the recommended marketing program.
## STEP 2: Prepare
### 2.1 Information on Data Source
The data used for analysis was collected by Cyclistic (through Motivate International Inc) and is located on their public website. The historical trip data was organized by month and year dating back to May 2022. The data was saved as CSV files within zip folders. Each file contained information on the following fields:

- ride_id: a unique ID per ride
- rideable_type: the type of bicycle used
- started_at: the date and time that the bicycle was checked out
- ended_at: the date and time that the bicycle was checked in
- start_station_name: the name of the station at the start of the trip
- start_station_id: a unique identifier for the start station
- end_station_name: the name of the station at the end of the trip
- end_station_id : a unique identifier for the end station
- start_lat: the latitude of the start station
- start_lng: the longitude of the start station
- end_lat: the latitude of the end station
- end_lng: the longitude of the end station
- member_casual: a field indicating whether the rider is a causal or member

## STEP 3: Process
### 3.1 Summary of data cleaning steps
- Removed null values from data set using the .dropna fuction.
- Removed duplicate rows.
- Changed the data type of started_at and ended_at variables from object to datetime using pd.ti_datetime.
- Added 3 new columns: trip_duration, month, and day_of_week.
- Removed rows with a trip_duration less than 1 minute and greater than 24 hours.
## STEP 4: Analyze
### 4.1 Perform calculations
Finding the average trip duration for casual riders and members.
​
```python bike_tripsclean.groupby(['member_casual'])['trip_duration'].mean()
member_casual
casual    23.430949
member    12.511454
Name: trip_duration, dtype: float64
```
#### Count of riders by membership status. 
```python​
bike_tripsclean['member_casual'].value_counts()
​
bike_tripsclean['member_casual'].value_counts(normalize=True) * 100
member    60.362648
casual    39.637352
Name: member_casual, dtype: float64
```

#### Count of total monthly rides by membership status. 
```python​
bike_tripsclean.groupby(['member_casual'])['month'].value_counts()
member_casual  month    
casual         July         306526
               June         287502
               August       265702
               September    217445
               May          216898
               October      148840
               April        108137
               November      72355
               March         45728
               February      32137
               December      30972
               January       29014
member         August       328513
               July         324245
               June         322209
               September    307779
               May          277114
               October      257407
               April        206710
               November     178724
               March        148771
               January      115413
               February     113677
               December     101607
Name: month, dtype: int64
```

#### Count of total daily rides by membership status. 
```python
bike_tripsclean.groupby(['member_casual'])['day_of_week'].value_counts()
member_casual  day_of_week
casual         Saturday       356689
               Sunday         296551
               Friday         257058
               Thursday       234160
               Wednesday      209103
               Monday         206762
               Tuesday        200933
member         Wednesday      430931
               Thursday       430049
               Tuesday        424563
               Monday         377488
               Friday         376584
               Saturday       338994
               Sunday         303560
Name: day_of_week, dtype: int64
```

## STEP 5: Share
### 5.1 Supporting Visualizations and Findings



### Total Trips by Membership Type
![total trips by membership type](https://github.com/user-attachments/assets/6a27c62c-47fb-4a5b-859d-0dcb8eabc49b)

Riders with an annual membership comprised 60.4% of all trips between May 2022 and April 2023.
Casual riders comprised 39.6% of all trips between May 2022 and April 2023.


### Types of Bikes by Membership Type
![type of bikes ](https://github.com/user-attachments/assets/a0825ab5-5922-4748-b05f-033bd1bf70fa)

The classic bike was the prefered bike for most riders followed by electric bike and docked bike.
The docked bike was only used by casual riders.
Members prefered to use the classic bike over the electric bike (1.72 million > 0.96 million).


### Average Trip Duration for Riders
![average trip duration](https://github.com/user-attachments/assets/e58081f5-6e6d-4cd4-8572-e818a22b3bb8)


The average trip duration for members and casual riders was 12.5 minutes and 23.4 minutes respectively.
Casual riders take less trips than members, but have longer trips on average.


### Total Monthly Trips
![total monthly trips](https://github.com/user-attachments/assets/b78535aa-cbb5-46bc-8525-fd2160558f78)


The highest volume of trips were taken in the summer months (June, July, & August). As the weather improves, riders are more likely to use bikes for recreation, leisure, or comutting to work.
Both casual riders and members tend to follow the same monthly trip trend with more rides in the summer and less rides in the winter.


### Total Daily Trips
![total daily trips](https://github.com/user-attachments/assets/f9ebd10b-d23b-4701-bd29-34b09cfbc0fd)

Casual riders are more likely to use bikes during the weekend (Saturday & Sunday) compared to the weekdays. This may suggest that casual riders are performing activities of leisure or recreation.
Members are more likely to use bikes during the weekday (Monday - Friday) compared to the weekends. This may suggest that members are commuting to and from work or school.
## 5.2 Dashboard

![cyclistic final dashboard](https://github.com/user-attachments/assets/4ec080b1-78a9-4b8e-ae61-93fd7511e867)

## STEP 6: Act
### 6.1 Revist Business Objectives
**1. How do annual members and casual riders use Cyclistic bikes differently?**

- Annual members typically use Cyclistic bikes during the weekday compared to casual riders that typically use bikes during the weekend.
- Using bikes during the weekday may suggest commuting to and from work or school.
- Bike use during the weekend may suggest recreational or leisure activities.
- Casual riders also had longer bike trips on average compared to annual members.
- Longer bike trips may support the suggestion of recreational or leisure activities as these riders are more flexible with the length of their trips.
- Annual members make up 60% of all Cyclistic bike riders compared to 40% for casual riders.

**2. Why would casual riders buy Cyclistic annual memberships?**

- Casual riders would buy Cyclistic annual memberships if they believe it fits well with their lifestlye.
- An annual membership might be a large commitment for casual riders to make the switch. 
- Casual riders are more likely to use Cyclistic bikes on the weekends and in the summer months.
- Offer memberships at varying lengths to appeal to a broader section of casual riders (1 month, 3 months, 12 months). 

**3. How can Cyclistic use digital media to influence casual riders to become members?**

- Cyclistic can use social media influencers to promote their annual memberships to their casual riders.
- Social media influencers have large followings and are great at promoting services to their fans.
- A campaign centered around social media influencers will likely increase the traffic and engagement of Cyclistic bikes.

### 6.2 Recommendations
Here are my top three recommendations for the marketing team to inform their next campaign:

1. Offer membership at different lengths (1 month, 3 months, and 6 months) to convert more casual riders to members.
2. Partner with social media influencers to promote the annnual membership to casual riders.
3. Improve the membership experience. Create incentives for casual riders to become members. This can include a reward system or member-exclusive events.
