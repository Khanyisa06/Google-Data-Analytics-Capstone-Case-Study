# Bike_Share_Data_Analysis

Welcome to the Cyclistic bike-share analysis case study! In this case study,I perform many real-world tasks of a junior data
analyst. I work for a fictional company, Cyclistic, and meet different characters and team members. In order to answer the
key business questions, I follow the steps of the data analysis process: **ask, prepare, process, analyze, share, and act**.
 
- I will work in the marketing analyst team at Cyclistic , a bike-share company in Chicago as a junior data analyst .
- As a team we have to understand how casual customers and annual subscriber use Cyclistic bikes differently.
- From these insights, design a new marketing strategy to convert Casual Customer into Annual Subcriber.
 
 ## *Table of Contents*
 
  2. [Objectives](#objectives)
  3. [Data Sources](#data-sources)
  4. [Project Tools](#project-tools)
  5. [Data cleaning Steps](#data-cleaning-steps)
  6. [Exploratory Data Analysis](#exploratory-data-analysis)
  7. [Data Analysis](#data-analysis)
  8. [Findings](#findings)
  9. [Recommendation](#recommendation)
  10. [Limitations](#limitations)
  11. [References](#references)

## Objectives

__Produce a report with the following deliverables:__

1. A clear statement of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top three recommendations based on your analysis
 
## Business Task

The primary business task is to understand how casual riders and annual members use Cyclistic bikes differently. As a junior data analyst in the marketing team, the goal is to gather insights that will inform the development of a targeted marketing strategy. The overarching objective is to maximize the number of annual memberships(convert Casual Customer  into Annual Subscriber), as identified by the director of marketing. 

To achieve this, the data analysis process will be followed, encompassing key steps such as asking relevant questions, preparing and processing the data, conducting a thorough analysis, and ultimately sharing actionable insights. The outcome of this analysis will not only address the immediate task of understanding user behavior but will also serve as the foundation for strategic decisions aimed at enhancing Cyclistic's future success in the bike-share market.


## Data Sources

__I used Cyclistic’s historical trip data to analyze and identify trends.__ [Download the Cyclistic trip data here](https://divvy-tripdata.s3.amazonaws.com/index.html).
  
**Note:**
- The datasets have a different name because Cyclistic is a fictional company.
- For the purposes of this case study, the datasets are appropriateand will enable you to answer the business questions.
- The data has been made available by Motivate International Inc. [Under this license](https://divvybikes.com/data-license-agreement)
- You can choose to work with an entire year of data, or just one quarter of a year.
- If you are working in Google Sheets, there are some files that might be too large to view.
- This is public data that you can use to explore how different customer types are using Cyclistic bikes.
- But note that data-privacy issues prohibit you from using riders’ personally identifiable information

## Project Tools

- R Studio/Posit Cloud
- EXCEL
- Tableau
- SQL Server Management Studio
- Google 
 
## Data cleaning Steps

__Below I outline the data cleaning steps I performed for the datasets using both Excel and SQL Server Management Studio (SSMS).__ 

__*For Cyclist_Trip_Data I:*__
1. load the data for inspection
1. Deleted unnecessary columns.
2. Removed rides with durations greater than 24 hours or less than one minute.
3. Created a "Ride_Length" column calculated as (Ended_at - Started_at) in HH:MM:SS format.
4. Created a "Day_of_Week" column using the WEEKDAY function on "Started_at" and formatted as a number with 0 decimals.
5. Renamed the "member" column to "Customer_Type," replacing "casual" with "Casual Customer" and "member" with "Cyclist_Member."
6. Filtered and deleted rows with blank values.
7. Manually corrected any inconsistent or erroneous data.

__*For Quarterly data  I:*__
1. Renamed columns to standardize names
 - "tripduraton" to "Trip_Duration"
 - "usertype" to "User_Type"
 - "birthyear" to "Birth_Year", Replaced null values in the Birth_Year column with 0.
 - "gender" to "Gender" , Replaced null values in the "Gender" column with "Not-Applicable."
2. Converted time from seconds to minutes for Trip_duration.
3. Formatted "Start_Time" and "End_Time" as "y:m:d h:s."
4. Filtered and deleted rows with blank values.
 
## Exploratory Data Analysis

1. __*Distinguishing Usage Patterns:*__
   - Explore ride frequency, duration, and popular routes for both annual members and  Casual Customer.
   - Analyze peak usage times for each group to understand when they use Cyclistic bikes differently.
   - Investigate any noticeable patterns or trends in user behavior, such as preferred ride times or specific days of the week.

2. __*Motivations for Annual Memberships:*__
   - Examine data related to  Casual Customer who transitioned to annual memberships.
   - Identify common factors among these riders, such as frequency of use, cost-effectiveness, or special promotions.
   - Investigate if there are certain time periods when  Casual Customer are more likely to convert to annual memberships.

3. __*Digital Media Influence:*__
   - Analyze the effectiveness of current digital media campaigns in attracting  Casual Customer.
   - Identify key demographics of Casual  Customer and tailor digital content accordingly.
   - Assess the correlation between promotional campaigns and the conversion of Casual Customer to Annual memberships. 

## Data Analysis
   
__To extract valuable insights and patterns fro the datasets I utilized SQL queries to analyze the Cyclist_Trip_Data and Quarterly(merged into one full yer) datasets__

1. __*Determine the distribution of user types (Casual Customer vs. Annual Subscriber)*__
``` sql
SELECT
    User_Type,
    COUNT(*) AS User_Count
FROM
    DBO.Cyclist_Data_FullYear
GROUP BY
    User_Type;
```

2. __*Determine the number of users per month (Casual Customer Rider vs. Annual Subscriber)*__
``` sql
 SELECT
    CASE MONTH(Start_Time)
        WHEN 1 THEN 'Jan'
        WHEN 2 THEN 'Feb'
        WHEN 3 THEN 'Mar'
        WHEN 4 THEN 'Apr'
        WHEN 5 THEN 'May'
        WHEN 6 THEN 'Jun'
        WHEN 7 THEN 'Jul'
        WHEN 8 THEN 'Aug'
        WHEN 9 THEN 'Sep'
        WHEN 10 THEN 'Oct'
        WHEN 11 THEN 'Nov'
        WHEN 12 THEN 'Dec'
    END AS Month,
    User_Type,
    COUNT(*) AS  Monthly_Users   
FROM
    DBO.Cyclist_Data_FullYear
GROUP BY
    MONTH(Start_Time),
    User_Type
ORDER BY
       Monthly_Users    DESC;
```

3. __*Determine the number of users by gender (Annual Subscriber)*__
``` sql
   SELECT
    User_Type,
    Gender,
    COUNT(*) AS Gender_Count
FROM
   DBO.Cyclist_Data_FullYear
 WHERE 
   User_Type = 'Subscriber' 
 
GROUP BY
    User_Type,
    Gender;
```
4. __* Determine the prefered bike type (Casual Customer vs. Annual Subscriber)*__
``` sql
 SELECT
	User_Type, Ride_Type,
	 COUNT(*) AS Ride_Count
  FROM
 DBO.Cyclist_Trip_Data
GROUP BY
 Ride_Type,
  User_Type
ORDER BY
  User_Type,
Ride_Count DESC;
```

5. __*Determine the average trip duration the distribution of user types (Casual Customer vs. Annual Subscriber)*__
``` sql
 SELECT
    User_Type,
    AVG(Trip_Duration) AS Avg_Trip_Duration
FROM
    DBO.Cyclist_Data_FullYear
GROUP BY
    User_Type
ORDER BY
    Avg_Trip_Duration DESC;
```
6. __*Determine the popular ride start and end  stations (Casual Customer vs. Annual Subscriber)*__
``` sql
   SELECT  
    User_Type,
    From_Station_Name,
    To_Station_Name,
    COUNT(*) AS Route_Count
FROM
     DBO.Cyclist_Data_FullYear
GROUP BY
    User_Type,
    From_Station_Name,
    To_Station_Name
ORDER BY
    User_Type,
    Route_Count DESC;
```

7. __*Determine the popular weekday for rides (Casual Customer vs. Annual Subscriber)*__
``` sql
 SELECT
    CASE
        WHEN Day_of_week = 1 THEN 'Monday'
        WHEN Day_of_week = 2 THEN 'Tuesday'
        WHEN Day_of_week = 3 THEN 'Wednesday'
        WHEN Day_of_week = 4 THEN 'Thursday'
        WHEN Day_of_week = 5 THEN 'Friday'
        WHEN Day_of_week = 6 THEN 'Saturday'
        WHEN Day_of_week = 7 THEN 'Sunday'
        ELSE 'Unknown'  -- Add this line to handle unexpected values
    END AS Day_of_week,
    User_Type,
    COUNT(*) AS Ride_Count
FROM
    DBO.Cyclist_Trip_Data
GROUP BY
    Day_of_week,
    User_Type
ORDER BY
 User_Type,
    Ride_Count DESC;
```

8. __*Determine the popular time of the day for for rides (Casual Customer vs. Annual Subscriber)*__
``` sql
   SELECT
    User_Type,
    COUNT(*) AS Ride_Count,
    DATEPART(HOUR,  Started_at) AS Hour_Of_Day
FROM
    DBO.Cyclist_Trip_Data
GROUP BY
    User_Type,
    DATEPART(HOUR,  Started_at)
ORDER BY
    Ride_Count DESC;
```

9. __*Determine the popular average time of the day for rides (Casual Customervs. Annual Subscriber)*__
``` sql
  
	 SELECT
    User_Type,
    AVG(CAST(DATEPART(HOUR, Started_at) AS int)) AS Avg_Hour_of_Day
FROM
    DBO.Cyclist_Trip_Data
GROUP BY
   User_Type;
```

10. __*Determine the popular  birth year (Annual Subscriber)*__
``` sql
  SELECT  User_Type,Birth_Year, COUNT(*) AS Birthyear_Count
	FROM DBO.Cyclist_Data_FullYear  
	GROUP BY User_Type,  Birth_Year
	ORDER BY  Birthyear_Count DESC
```

## Findings

__Based on my analysis of the data these are my summerized findings__

- __*Annual Subcribers ride more frequently than casual Customer*__

  ![User_Destributin](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/4737d00d-668f-4a96-abcd-6241371f1a6f)
  
- __*On average, Customers have a longer trip duration of 14 minutes compared to Subscribers, who have an average trip duration of 9 minutes*__

![Average Trip Duration](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/aea3a59d-7884-4bb3-ba06-3123359ab93f)
  
- __*June and May stand out as the peak months for user activity, with both annual subscribers and casual customers showing the highest engagement in these periods.*__

 ![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/fe56c334-6813-4229-b62d-e2fc4215cb84)

-  __*The gender distribution within the subscriber category, with a higher number of male subscribers compared to female subscribers.*__

  ![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/fb90aeb4-277d-4b5e-ba95-7ff1f137b32d)

- __*The classic bike is more popular than the electric bike, with significantly higher usage by both subscribers and customers*__
  
  ![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/1f92203c-e5bd-4dee-bae4-10115b678399)

- __*The route from "Lake Shore Dr & Monroe St" to "Streeter Dr & Grand Ave" is the most popular among customers*__
- __*Streeter Dr & Grand Ave" is a central station for customers, being either the starting or ending point in several popular routes.*__
- __*Canal St & Adams St" to "Michigan Ave & Washington St" is the most frequented route among subscribers.*__
- __*Stations like "Michigan Ave & Washington St," "Canal St & Adams St," and "Columbus Dr & Randolph St" are central to the subscriber routes.*__
  
  ![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/777a2dce-2b6a-404c-ad42-131ca295b190)


- __*For Customers, Monday is the peak day, while for Subscribers, Wednesday is the peak day.*__
- __*For Customers and Subscribers Sunday has a lower ride counts.*__
- __*Customers contribute more to the overall ride counts on weekends, especially on Saturday.*__
- __*Subscribers contribute more to the overall ride counts on weekdays*__
 
 ![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/85ff58f4-bf76-43f6-a2dd-f30817b6b5d8)

- __* Service is more popular during the daytime, especially in the afternoon, for both Subscribers and Customers.*__
- __*There is a significant drop in ride counts during late-night hours (from 22:00 to 5:00), for both Subscribers and Customers.*__
- __*The busiest hours seem to be in the afternoon, particularly around 5 PM (17:00) by Subscribers and Customers, respectively.*__
 
![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/367f50bf-d64f-4608-9898-4e8f98a23d1a)
  
- __*subscribers born in the 1990s and early 1980s are more numerous compared to those born in the 1970s and earlier.*__
- __*There is a noticeable presence of subscribers born in the late 1990s and early 2000s, suggesting engagement from a younger audience.*__
- __*The is a general decrease as birth years move away from the 1990s, indicating a decline in subscriber numbers for older age groups..*__
- __*The birth year 1992 has the highest number of subscribers.*__
  
![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/e828e3cf-7630-4c37-ba53-a3ee6bc21379)


__HERE IS THE WHOLE DATA DISPLAYED IN A DASHBOARD__

- __*Annual Subscribers*__

![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/1028b03c-28b3-483a-9136-07a320d69a58)

- __*Casual Customers*__

![image](https://github.com/Khanyisa06/Cyclist_Bike-Share_Analysis/assets/106344554/e489f63e-8550-4713-8e01-3275ceec091c)


both Subscribers and Customers have an average start time for rides at 13:00 PM. This suggests that, on average, users from both categories tend to initiate their rides around 1:00 PM.

## Recommendation
__These are my reccomandations__
- Implement referral programs that reward current annual members for referring casual riders who successfully convert to annual memberships.
- Leverage word-of-mouth marketing to encourage casual customers to consider the benefits of long-term membership.
- Introduce limited-time promotional offers or discounts for casual customers who sign up for annual memberships.
- Provide free trial periods or bonus ride credits to encourage the transition to annual memberships.
- Develop targeted marketing campaigns to attract more users, particularly females, to the subscriber category.
- Allocate resources efficiently during peak months like June and May to meet increased demand.
- Ensure an adequate supply of classic bikes, considering their popularity among both subscribers and customers.
- Optimize bike station facilities and services at popular locations like "Streeter Dr & Grand Ave" and "Canal St & Adams St" to enhance the overall user experience.
- Provide incentives for users to explore less congested routes, balancing the load on popular routes.
- Implement promotions or discounts during non-peak hours to encourage bike usage during these times.
- Leverage the insight about Monday being a peak day for customers and Wednesday for subscribers to create targeted promotions on these days.
- Investigate reasons behind the decline in subscriber numbers for older age groups and develop strategies to re-engage these segments.
- Enhance the user interface of the bike-sharing platform to provide a seamless experience, especially during peak hours.
- Explore the integration of technologies such as mobile apps or notifications to keep users informed about bike availability and promotions.
- Tailor marketing efforts to address the preferences and behaviors of users born in the late 1990s and early 2000s, considering their higher engagement.

## Limitations
- I have removed all the tables and colmns from the dataset that I didnt need because they would have affected the accuracy of my analysis and findings
- All rides greater than 24 hours and less than 1 minute are irrelevent


## References

