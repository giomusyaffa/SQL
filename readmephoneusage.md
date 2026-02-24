# Step-by-Step Analysis on The Screen Time, Sleep & Stress Analysis Dataset Dataset 😴📱☕

## Project Overview  
This project analyzes the correlation between screen time, sleep quality, stress level, caffeine intake, etc.

## Dataset Description  
- **Source:** Kaggle - Screen Time, Sleep & Stress Analysis
- **Key Features:**
    - Age
    - Gender
    - Occupation
    - Device_Type
    - Daily_Phone_Hours
    - Social_Media_Hours
    - Work_Productivity_Score
    - Sleep_Hours
    - Stress_Level
    - App_Usage_Count
    - Caffeine_Intake_Cups
    - Weekend_Screen_Time_Hours

## SQL Analysis / Queries  
1. Data Quality Checks (NULL & Duplicate Values)
2. Distribution Analysis
3. Multi-Factor Group Analysis

## Step 1: Data Quality Checks
### 1. Check NULL Values
Before analyzing this data, the first thing we have to do is to check for NULL Values
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/98914d3c-0d66-461f-b2ab-aba9a01e787e" />

As we can see from the query above, by using the `CASE WHEN` statement, which is a conditional expression that will return a value when the condition is met (same like if statement in Excel), 

`SUM(CASE WHEN User_ID IS NULL THEN 1 ELSE 0 END) AS column name`

will return `1` when there is a NULL Value and `0` if there isn't 

### 2. Duplicate Values
This is also important to check to avoid unnecessary errors in analyzing the data
<img width="1919" height="934" alt="image" src="https://github.com/user-attachments/assets/2d0c1222-71a8-47c5-9aa2-8224ce57058e" />

As we can see from the query above, by using the `HAVING` clause  with the `COUNT()` function, we can filter groups of data based on the count of rows within each group.

In this case, for the User ID, it is crucial not to have duplicate values.


Now, after checking that this dataset doesn't have any missing or duplicated values, we can start to analyze it.

## Step 2: Distribution Analysis
### 1. Stress Level Distribution
Let's see the percentage of users in each level of stress from 1 to 10
<img width="1610" height="937" alt="image" src="https://github.com/user-attachments/assets/57fc5031-42da-407b-a084-7ae9511166c5" />

By using this query, the stress distribution is pretty even, averaging about 5000 users for each level of stress, so there's no dominant point where the users peak at a certain level, meaning most users are not stressed.

### 2. Age Distribution
To really see the demographic in this dataset, I classify ages between 18 - 24, 25 - 34, 35 - 44, 45 - 54, and 55 +

<img width="1619" height="899" alt="image" src="https://github.com/user-attachments/assets/4a076163-2c1c-4697-a697-9b07b8765b74" />

As we can see in the query above, ages 25 - 54 are significantly higher than other age groups, which is true for this project, seeing how that range is most likely digital natives.

## Step 3: Multi-Factor Group Analysis
### 1. Sleep V Stress
