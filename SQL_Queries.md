#### Checking for duplicate entries for a single user in dailyActivity table (none found)

```SQL
SELECT Id, (COUNT(ActivityDate) -  COUNT(DISTINCT ActivityDate)) as dupe_check
FROM `bellabeat-429004.Fitbit_Data.dailyActivity_merged`
GROUP BY  Id
HAVING dupe_check>0

```

#### Finding the average monthly number of steps for each user to use the average total steps to bin customers

```SQL
SELECT
  Id, AVG(TotalSteps) as MonthlyAverageSteps
FROM
  bellabeat-429004.Fitbit_Data.dailyActivity_merged
GROUP BY
  Id
ORDER BY
  MonthlyAverageSteps DESC

```

#### Using conditional statements to assign a class to a user based on their activity. Binning based on values from histogram. Saving the result table as a csv and adding it to the bigquery database so that it can be used everywhere.

```SQL
SELECT 
  Id,
  CASE 
    WHEN CAST(ROUND(AVG(TotalSteps)) AS INT64) <= 4816 THEN "Less"
    WHEN CAST(ROUND(AVG(TotalSteps)) AS INT64) BETWEEN 4817 AND 8716 THEN "Moderate"
    WHEN CAST(ROUND(AVG(TotalSteps)) AS INT64) BETWEEN 8717 AND 12616 THEN "High"
    ELSE  "Very High"
  END AS Activeness
FROM
  bellabeat-429004.Fitbit_Data.dailyActivity_merged
GROUP BY 
  Id


```

#### Getting a high level summary of users based on their activity levels

```SQL
SELECT 
  user_cat.Activeness,
  ROUND(AVG(daily_activity.TotalDistance)) as avg_distance,
  ROUND(AVG(daily_activity.TotalSteps)) as avg_steps,
  ROUND(AVG(daily_activity.VeryActiveMinutes)) as avg_active_time,
  ROUND(AVG(daily_activity.SedentaryMinutes))  as avg_sedentry_time,
  ROUND(AVG(daily_activity.Calories)) as avg_calories

FROM 
  `bellabeat-429004.Fitbit_Data.dailyActivity_merged` AS daily_activity LEFT    
   JOIN   bellabeat-429004.Fitbit_Data.user_categorization AS user_cat ON 
   daily_activity.Id = user_cat.Id

GROUP BY 
  user_cat.Activeness

ORDER BY
  CASE user_cat.Activeness
    WHEN "Low" THEN 1
    WHEN "Moderate" THEN 2
    WHEN "High" THEN 3
    WHEN "Very High" THEN 4
  END


```

#### Getting a table that gives average hourly steps for each user throughout the time their data was recorded. Then group the average hourly activities based on the activeness to see the hourly activity behaviour across different activeness levels of users. The primary table for this was created by pivoting an originaltable called ‘hourlySteps_merged.csv

```SQL
WITH user_hourlysteps_avg AS 
(
SELECT uhs.*, ucat.Activeness
FROM `bellabeat-429004.Fitbit_Data.user_hourlyStepsAvg` as uhs JOIN 
bellabeat-429004.Fitbit_Data.user_categorization as ucat ON uhs.Id=ucat.Id
)


SELECT 
    Activeness,
    ROUND(AVG(`12 AM`)) AS `00`,
    ROUND(AVG(`1 AM`)) AS `01`,
    ROUND(AVG(`2 AM`)) AS `02`,
    ROUND(AVG(`3 AM`)) AS `03`,
    ROUND(AVG(`4 AM`)) AS `04`,
    ROUND(AVG(`5 AM`)) AS `05`,
    ROUND(AVG(`6 AM`)) AS `06`,
    ROUND(AVG(`7 AM`)) AS `07`,
    ROUND(AVG(`8 AM`)) AS `08`,
    ROUND(AVG(`9 AM`)) AS `09`,
    ROUND(AVG(`10 AM`)) AS `10`,
    ROUND(AVG(`11 AM`)) AS `11`,
    ROUND(AVG(`12 PM`)) AS `12`,
    ROUND(AVG(`1 PM`)) AS `13`,
    ROUND(AVG(`2 PM`)) AS `14`,
    ROUND(AVG(`3 PM`)) AS `15`,
    ROUND(AVG(`4 PM`)) AS `16`,
    ROUND(AVG(`5 PM`)) AS `17`,
    ROUND(AVG(`6 PM`)) AS `18`,
    ROUND(AVG(`7 PM`)) AS `19`,
    ROUND(AVG(`8 PM`)) AS `20`,
    ROUND(AVG(`9 PM`)) AS `21`,
    ROUND(AVG(`10 PM`)) AS `22`,
    ROUND(AVG(`11 PM`)) AS `23`
FROM user_hourlysteps_avg
GROUP BY Activeness

```

#### Getting steps in the day for the users with same day night sleep

```SQL
SELECT
  ons.*, da.TotalSteps as daySteps
FROM 
  bellabeat-429004.Fitbit_Data.only_night_sleepers as ons 
  JOIN
  bellabeat-429004.Fitbit_Data.dailyActivity_merged as da
  ON ons.Id=da.Id AND ons.SleepDay=da.ActivityDate

```

#### Getting sleep deviations

```SQL
SELECT Id, Activeness, ROUND(AVG(TotalMinutesAsleep)) as AvgSleep, COUNT(Id) as records,
MIN(TotalMinutesAsleep) as minSleep, MAX(TotalMinutesAsleep) as maxSleep, 
ROUND(STDDEV(TotalMinutesAsleep),1)as sleepDeviation
FROM 
  `bellabeat-429004.Fitbit_Data.only_night_sleepers` 
GROUP BY
  Id, Activeness

```

