# Step-by-Step Analysis on The Customer Sentiment Dataset 🛍️

## Project Overview  
This project analyzes a Customer Sentiment dataset from Kaggle using SQL.
The goal for this project is to demonstrate real-world SQL analysis, similar to what analysts or data engineers perform in e-commerce, customer experience, and marketing analytics.


## Dataset Description  
- **Source:** Kaggle - Customer Sentiment Dataset
- **Key Features:**
    - customer_id : unique ID (Primary Key)
    - gender, age_group, region
    - product_category, purchase_channel, platform
    - customer_rating (1–5)
    - review_text
    - sentiment (positive / neutral / negative)
    - response_time_hours
    - issue_resolved
    - complaint_registered

## SQL Analysis / Queries  
1. Explore customer behavior across demographics and purchase channels
2. Analyze sentiment and satisfaction levels
3. Identify patterns in low ratings, negative reviews, and unresolved issues

## Step 1: Import the Data
Import the .csv file into the dataset that you made in SQL

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ff6d1c31-0e24-44cb-aabc-890ff697b3b1" />


As you can see from the picture above I already made my dataset for this project called "Portfolio", and then to import the .csv file that you've download from Kaggle, go to tasks, and import flat file.

Then, after importing the file you would see something like this

<img width="1919" height="931" alt="image" src="https://github.com/user-attachments/assets/fe8986c3-4ad1-422f-b60d-c8c0360ce807" /><br />

First thing first, let's get to know some of the basics queries that will be used in this project.

## Step 2: Basic Keywords (SELECT, WHERE , GROUP BY, ORDER BY)
### 1. SELECT FROM
Starting from the very basics of SQL is `SELECT`, this keyword basically "selects" what you wanna see from your table, for example:

<img width="1919" height="947" alt="image" src="https://github.com/user-attachments/assets/f76aa790-a061-46ae-a49f-88587f272143" />

This query above shows  the customer_id, gender, and sentiment form the data. Simple right?

### 2. WHERE
And then we got `WHERE`, this filters rows before grouping or aggregation.

Think of it as: “Only show rows that meet this condition.”

<img width="1919" height="944" alt="image" src="https://github.com/user-attachments/assets/cd33b013-66d9-4fe3-ae18-acae846bd041" />

As you can see from the picture above, we can use the `WHERE` keyword to essentially filter the rows to show only the negatives sentiment. 

You can also combine WHERE conditions using AND, OR, and parentheses, for example let's say we wanna see the negatives sentiment but only for online purchases through amazon, the query would look like this:

<img width="1685" height="830" alt="image" src="https://github.com/user-attachments/assets/4d22f3c1-61c5-4d6d-8d0f-3dac28e8e6ca" />


### 3. GROUP BY
Groups rows with the same value into one group, often before using aggregate functions (COUNT, AVG, SUM).

An application for this can be used to count total number of reviews for each sentiment category (POSITIVE, NEUTRAL, NEGATIVE)

<img width="1919" height="940" alt="image" src="https://github.com/user-attachments/assets/3d66f199-6ff8-4536-b439-66ea0308f34c" />

### ORDER BY
Sort the results in ascending (ASC) or descending (DESC) order. For example:

<img width="1919" height="933" alt="image" src="https://github.com/user-attachments/assets/9257cf82-ff3a-4c22-b1d5-460e1630662a" />

We can sort the ratings given by the customer on a descending order (5 to 1)

Now let's combine them together to write the full query

## Step 3: Full Complete Structure Query

### 1. Which Product Categories Receive Most Negative Reviews?

<img width="1919" height="937" alt="image" src="https://github.com/user-attachments/assets/45aafdd1-2f55-4016-a115-39470385cc7e" />

### 2. Sentiment by Age Group

<img width="1919" height="932" alt="image" src="https://github.com/user-attachments/assets/54edcd40-b59f-45a9-8e9c-f72784763a06" />

### 3. Platform Performance, Which Platforms Have Best Ratings?
<img width="1918" height="934" alt="image" src="https://github.com/user-attachments/assets/e974b2c0-b7a4-4a46-9d75-0d7ce5f111d2" />

### 4. Does resolving issues increase positivity?
<img width="1919" height="941" alt="image" src="https://github.com/user-attachments/assets/1374fa26-2794-4023-9fa2-7862c1e39e09" />






