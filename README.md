# CASE STUDY: Bellabeat Data Analysis
###### [Click to visit my Tableau Public profile](https://public.tableau.com/app/profile/kartik.thakkar/vizzes)

## Contents 

- [Scenario](#scenario)
- [Tools and technologies](#tools-and-technologies)
- [Data Processing and Preparation](#data-processing-and-preparation)
- [Data Analysis](#data-analysis)
- [Data Visualizations](#data-visualizations)
- [Interesting trends and findings](#interesting-trends-and-findings)
- [Recommendations for business expansion](#recommendations-for-business-expansion)
- [Limitations](#limitations)

## Scenario

Bellabeat is a high-tech company that manufactures health-focused smart products. This is 2017 and the company is growing rapidly and has the potential to become a large player in the smart device industry. Currently, the company has 5 products in the market :
- Bellabeat app 📱 : An app that provides all the health-related insights and numbers to the users.
- Leaf 🏋️ : A wellness tracker that can be worn as a bracelet, a clip or a necklace. It tracks all the things like sleep, activity, and stress.
- Time ⌚ : A smartwatch that integrates with the app to provide easy and quick insights to the users.
- Spring 💧 : A smart water bottle that tracks water intake and is integrated with the app.
- Bellabeat membership 🪪 : A subscription-based program for users. Gives 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.

The goal is to analyze smart device usage data from a giant corporation in the industry to gain insights into how people are using their smart devices. The data is available publicly. The analysis will then be used to guide the marketing strategy for the company.


**The business task is to analyze Fitbit user data to find out trends and gain insights in terms of how users used the data combined with how their usage can be used to help the marketing team at Bellabeat effectively market and promote the usage of their products and services**

## Data: Source, Details and Issues

The data is publicly available on [Kaggle](https://www.kaggle.com/datasets/arashnic/fitbit) to download.

Here are some points that describe the data used for this analysis:
  - The data pertains to 33 unique users.
  - The data has 18 files each belonging to different aspects related to fitness like user activity, user sleep by time, and user weight logging. The data records are for a month.
  - All files are .csv format and the data as a whole is on daily, hourly and per-minute scales. Some data sources have data at the second level but it was not used in this analysis.

Quality of data: Overall it is good because it comes from a reliable source, it is original because it is taken firsthand from users, it is comprehensive and contains a lot of metrics and points to study trends, and it is also cited by many people for research and study purposes. However, it might be incomplete because users might not have provided some data for some metrics

**Bias** in the data ⚠️: There is a high chance that the data is biased because there are no mentions of the demographics of the users who were selected for the study. The data could be biased to people who live in cities and are working a 9-5 job. Or it could be representing a bunch of college kids who signed up for fun.


## Tools and technologies
- Microsoft Excel: to clean, prepare, and engineer data when needed.
- SQL, Google Bigquery: to store necessary data tables, query data from multiple tables, and engineer new tables for analysis.
- Tableau: to visualize findings and create dashboards

## Data Processing and Preparation
Microsoft Excel and SQL on Google Bigquery were extensively used hand in hand with data analysis to prepare the data or generate the data for analysis. The case study was a cyclic task where I noted down things that can be analyzed and processed the prepared data accordingly rather than following a linear system which restricts the thinking for interesting analysis. How this processing and preparation was carried out can be in seen the [Data Analysis](#data_analysis) section. On a broader level, this was how data was prepared or cleaned.

- Checking for duplicate and missing records
- Changing the date and time columns to the correct data format so that it was compatible to be uploaded on BigQuery
- Using `Pivot Table` and `VLOOKUP` functionalities to summarize and look for data in Microsoft Excel
- Using `JOINS`, `CASE-WHEN Statements`, and `AGGREGATE Functions` in SQL


## Data Analysis

#### 1. User Categorization to support Targeted Marketing strategies:
   - Since the whole data revolves around something that can attain certain levels, the users were categorized into certain groups based on their activeness.
   - The metric used to bin these users was their `MonthlyAverageSteps`.
   - SQL query was used to find out the `MonthlyAverageSteps` and the result set from the query was used in Excel to create a quick chart so that values to bin the users can be found out.
   - The binned values from the chart were used in `CASE-WHEN` queries to assign an 'Activeness' attribute to the user. The result set was stored in `user_categorization.csv` to be referenced in the future.
   - The 'Activeness' attribute had 4 unique values: 'Low', 'Moderate', 'High', and 'Very High'

  ![Steps Distribution](https://github.com/user-attachments/assets/0cbda0ef-ed20-4bfa-8a70-ad6a275439bf)

#### 2. Hourly move patterns throughout the day by user activeness to promote more device usage:
   - The data recorded the user activities for one month. To gain an insight into how the highly active users move throughout their day and compare their activity with users from other activeness levels, data was prepared and queried.
   - The original table `hourlySteps_merged.csv` was used to create a `Pivot Table` to arrange data for each user's activity into a wide format.
   - A `COMMON TABLE EXPRESSION` was created by using a `JOIN` to gather the activeness level for each user.
   - The CTE was queried to obtain average hourly steps by each category in terms of activeness.

#### 3. Sleep: User records and differences in sleep patterns by activeness for sleep-based product recommendations:
   - The original `sleepDay_merged.csv` file presented data like the number of sleep records for the day, time asleep, and time in bed for each day and user.
   - To analyze only the night sleep, the data was filtered to have data with only 1 sleep record for the day.
   - `VLOOKUP` was used to pull activeness for each user from the `user_categorization.csv` file.
   - An additional attribute was created to show the difference between total time spent in bed and total time spent asleep to analyze sleep delays against how active a user is.
     

  ![sleep_duration](https://github.com/user-attachments/assets/851f9537-878e-485f-aa9d-5b222a75e309)


#### 4. Deviations in sleep:
   - For each user who reported sleep data, the frequency of the report varied.
   - `AGGREGATE FUNCTIONS` from SQL were utilized to find deviations in sleep for a given person to better understand their sleep quality and habits.
   - A hypothesis was laid out claiming that the more active a user is in general, the more regular their sleeping habits are.
   - To quantify the 'regularity' or 'deviations' in sleep, *standard deviation* for the available sleep records for each user was used.

## Data Visualizations

- A [dashboard](https://public.tableau.com/views/BellabeatSummary_17214536730830/SmartDevicesUserDistribution?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link) to highlight key differences among users belonging to different activeness levels by using several metrics. It also shows the average number of steps a group takes for each hour of the day and the distribution of each group among all users.
- A [dashboard](https://public.tableau.com/views/SmartDevicesSleepInsights/BellabeatSleepInsights?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link) that provides a summary of unique users from each activeness group recording sleep data, including sleep duration distribution by activeness levels, individual values, and insights into sleep delays and user-wise sleep irregularity.

## Interesting trends and findings
1. Move Patterns 🚶: A general trend in the number of steps across users of all activeness levels is that it increases as the day starts and decreases by the last hour. Interestingly, the least active users never average out more than 275 steps a day while highly active users keep moving till the evening and even after that. Whereas, moderately active users average around a steady 500 steps a day, and active users average close to 1000 steps several hours of the day. Such less movement in the case of less active users can cause issues like obesity and joint pain ultimately leading to mobility issues.
2. Having an ideal sleep duration 🛌 : According to [National Center for Biotechnology  Information](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4434546/#:~:text=Adults%20should%20sleep%207%20or,and%20increased%20risk%20of%20death.), the recommended sleep duration is at least 7 hours of sleep (420 minutes). Sleeping less than 7 hours can lead to impaired immune function, obesity, depression, and many more issues.
     - Here are the percentage of times users slept less than the recommended sleep duration by their activeness levels: Active users: 49%, Less Active users: 46%,  Moderately active users: 66%, and Highly Active users: 33%.
     - This is the [visualization](https://public.tableau.com/views/SmartDevicesBelowRecommendedSleep/BelowRecommendedSleepDistribution?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link) for the same.
  
3. Delays in sleep 🕐: The number of steps for a day was plotted against the delay in sleep for that day for the user. An unusual finding in the plot was that while the two measures were not correlated, a large number of times when the users had a sleep delay of more than 90 minutes, the users were active.
4. Sleep irregularity ⏲️: The hypothesis that the sleeping patterns would be more stable for highly active users in comparison to users with relatively less activity in general was rejected and it was seen that it depended on user to user.

![Screenshot (82)](https://github.com/user-attachments/assets/41690e4e-118d-43ca-8a19-61cf870b9d25)


## Recommendations for business expansion

1. **Dynamic Free trials for users**: Less active and dormant users tend to have significantly fewer values for metrics like distances walked, and time spent doing an activity while having a high overall sedentary time. Given the limitation that there is not enough data about their lifestyle and demands from their workplace or school, instead of sending them constant reminders about their activity, they can be introduced to a lucrative program free of cost where they get to use the Bellabeat membership for a specific duration of time. Since the Bellabeat Membership provides personalized help and recommendations based on the lifestyle of the user, less active users will get to experience a change and will likely be willing to pay for the membership after their free trial ends. This recommendation of course is based on the assumption that there are existing customers who are using at least one of the health and wellness trackers.
2. **Social Media campaigns and personalized notification nudges for sleep**: A common issue across users from all activeness levels was related to sleep. A large number of times, users did not get adequate sleep, had trouble going to sleep, or had an irregular sleep schedule. A social media campaign  could be started that is timed for the AM hours so that users who resort to scrolling on social media when they can't sleep can be made aware of a solution. For existing users with at least one device, personalized notifications can be sent with floating prices for subscriptions so that they can have access to mindfulness activities and nutrition to enhance their overall sleep quality.
3. **Obtaining more data for analysis**: Since there is a lack of demographic and lifestyle-related data, existing analyses can be underpinned significantly with such data available. Encouraging users with what we can offer if they provide details of their lives through mailing systems can be a useful and efficient way to obtain more data.

## Limitations
Due to a small pool of unique users in the available data, analyses can be questioned. The given recommendations account for this and are more general than user or category-specific. With more users or at least with data for longer durations of time, many analyses can be generalized and granularity for recommendations can be refined.

  
