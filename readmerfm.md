# Step-by-Step Analysis on The E-Commerce Customer RFM Dataset 💸

## Project Overview  
This project cleans and analyzes a customer segmentation technique that evaluates client value based on three metrics: Recency (last purchase date), Frequency (number of transactions), and Monetary Value (total spent). 
## Dataset Description  
- **Source:** Kaggle - EDA, RFM, Ad Strategy & Cost Optimization
- **Key Features:**
    - CustomerID
    - Demographics (Gender, Age, Country, City)
    - Branch info
    - Last Purchase Date
    - Total Transactions
    - Total Spent
    - Currency
    - Payment Type
    - Credit Limit

## SQL Analysis / Queries  
1. Data Quality Checks (NULL & Duplicate Values)
2. Customer Demographic Analysis
3. Revenue Analysis
4. Payment Behavior Analysis
5. Recency Analysis

## Step 1: Data Quality Checks
### 1. Check Anomaly Values
Before analyzing this data, the first thing we have to do is to check for NULL Values.

Right on top, we can see that when selecting the first 1000 lines, there are a couple anomaly that show, which are in `Age`, showing ages above 100, and `Country` showing Invalid_Country.

<img width="1497" height="939" alt="image" src="https://github.com/user-attachments/assets/1f134e74-b522-4e82-b40d-c3164d96d82c" />

So, seeing as there are a lot of anomalies in this dataset, let's make a clean copy. NEVER update raw data directly, always create a cleaned version of the table.

<img width="1496" height="887" alt="image" src="https://github.com/user-attachments/assets/6cef32e8-fd6d-40aa-b3c1-e0592d1b9e19" />

By using this, we can safely clean `arpand_ecommerce_cleaned`.

<img width="1495" height="915" alt="image" src="https://github.com/user-attachments/assets/f653fdfc-f856-4bca-ba5e-ceeaf549aa50" />

Running this, we can see clearly the Age anomaly -10 and 150. So how do we clean these anomalies?

#### A. Replace Invalid Ages with the Median Age
<img width="1453" height="830" alt="image" src="https://github.com/user-attachments/assets/73fb17fb-7976-4b93-9599-f7e6f856b9ee" />

After getting the median, 49, we can now replace the invalid ages.

<img width="1494" height="890" alt="image" src="https://github.com/user-attachments/assets/5480ca4c-094b-49ea-83da-288ac2dcd60c" />

#### B. Standardize Gender
I also recheck the `Gender` column using `SELECT DISTINCT`, we see that there is a `nan` value, which is an anomaly.

<img width="1500" height="660" alt="image" src="https://github.com/user-attachments/assets/a3cd53dd-e4a0-4572-a534-5a4d02c2c0c8" />

So the solution is to replace gender other than female and male with Unknown

<img width="1499" height="944" alt="image" src="https://github.com/user-attachments/assets/9632c5a4-6f86-4dc8-91a7-d0ab3dbeac3d" />

#### C. Invalid Country and City
First, let's find the Invalid Values
<img width="1497" height="938" alt="image" src="https://github.com/user-attachments/assets/ee500180-0407-4ff5-8414-eb242f6e50fd" />

Then, we can replace the Invalid_Country and Invalid_City with NULL, but before that, modify the column to allow NULL.

<img width="1501" height="942" alt="image" src="https://github.com/user-attachments/assets/4581f595-59a5-44d1-bdb4-285f073998bd" />

<img width="1503" height="942" alt="image" src="https://github.com/user-attachments/assets/f142e381-d63b-44d9-b5b9-34b697e22585" />

And after running those two, we can now change the Invalid values to NULL
<img width="1489" height="937" alt="image" src="https://github.com/user-attachments/assets/b155c234-cc72-4da4-bc1a-626e78c44c58" />

Now, we can start the analysis.

## Step 2: Customer Demographic Analysis

### 1. Gender Distribution

<img width="1497" height="932" alt="image" src="https://github.com/user-attachments/assets/ccc7c445-55b8-4238-a3c1-fcfaaf8c23dd" />

As you can see, now we have classified gender into three: `Unknown, Male, and Female`. 

Looking at the first line `COUNT(*) * 100.0 / SUM(COUNT(*)) OVER()`, this calculates percentage.

`COUNT(*)`: number in that gender group

`SUM(COUNT(*)) OVER()`:total customers in entire table

`OVER()`: Is basically to calculate over the whole result set.

`ROUND(...,2)`: Rounds percentage to 2 decimal places.

The results show that both `Male` and `Female` have an even ratio, about 45:45, and the rest are for the `Unknown` gender, which is about 10%, meaning `Male` and `Female` dominate the market.

### 2. Age Distribution

<img width="1502" height="936" alt="image" src="https://github.com/user-attachments/assets/7533e63d-6b76-413e-8d2e-f57c36bd458a" />

Using `CASE WHEN`, which is basically the same as `IF` function in Excel, will return a value when the conditions are met.

Here we set the conditions based on the age group: 18 to 25,  26 to 35, 36 to 50, and 50+

A pretty interesting thing to notice here is that the age group that has the largest number of customers belongs to the 50+ age group, with a total customer base almost twice that of the 36-50 age group. With the lowest number of total customers belongs to the 18 to 25 age group.

## Step 3: Revenue Analysis

### 1. Top 10 Highest Spending Customers

<img width="1493" height="937" alt="image" src="https://github.com/user-attachments/assets/6fe00d73-bb73-49ec-af25-7833a2471bab" />

Using the `SELECT and FROM` statement, we classify 4 distinct categories: Customer ID, Country, Total Transactions, and Total Spent. 

The point of running this query is to know which customer spends the most, and we can see belongs to the customer with the ID 7419535828 from Germany. 

I use `ORDER BY TotalSpent DESC` to show the top 10 customer based on their total transaction in descending order.

### 2. Revenue by Country

<img width="1491" height="928" alt="image" src="https://github.com/user-attachments/assets/d1cb27c0-92ff-478b-982d-646dedee5ff0" />

This shows that the most profitable country is Singapore, followed by Canada and Germany while the least profitable one is South Africa.

Although if you see the Average Customer Spend:

<img width="1498" height="938" alt="image" src="https://github.com/user-attachments/assets/e339133e-d120-4aa9-b05d-d18765012ba8" />

Singapore is still number one, but South Africa has moved up to second place.

But you can also see that the NULL value is pretty significant, with 300 Customers, which means that the missing value really determines the validity of the data; in other words, those 300 people can belong to South Africa, pushing the country's revenue. This missing data could significantly affect country-level revenue analysis and market ranking.

### 3. Revenue by Branch

<img width="1496" height="940" alt="image" src="https://github.com/user-attachments/assets/6d21e48a-8aed-43a7-9dc3-b354763b92d2" />

Once again, we see that Singapore dominates the market by the highest revenue, even with the lowest number of customers than other branches. This means Singapore has a highly developed market economy, surpassing other developed countries in the West and Asia.

## Step 4: Payment Behavior Analysis

### 1. Credit Vs Instant Payment
<img width="1496" height="938" alt="image" src="https://github.com/user-attachments/assets/1c66867e-d34f-42cb-9280-9329926937be" />

Using `SELECT ... FROM`, I classify payment type with the number of customers and average spent

Seeing the result, Credit has a significantly lower number of customers than Instant payment, which means most people prefer to pay using instant payment. This is important to evaluate why Credit payment is not popular among customers.

### 2. Credit Utilization Rate
<img width="1502" height="934" alt="image" src="https://github.com/user-attachments/assets/e542154e-5863-41da-863c-220bdea5a759" />

Same method with gender distribution above, we count the percentage of CreditUsagePayment based on credit limit and total spent.

Customers have a reluctance to use credit payment with a high credit limit.

## Step 5: Recency Analysis

### 1. How Recent Are Customers?

<img width="1501" height="939" alt="image" src="https://github.com/user-attachments/assets/12dfae75-6d27-48cb-a3a4-9e5b6f67006f" />

Since this dataset is pretty old, the most recent purchases to date are from early 2023. We can see this by running this query:

<img width="1501" height="938" alt="image" src="https://github.com/user-attachments/assets/001757d6-eb40-4470-9092-409ede8c6682" />

Which is why the number is so high, but let's continue the analysis.

<img width="1503" height="930" alt="image" src="https://github.com/user-attachments/assets/94b3e9fa-0c82-44a8-bbcd-3e23cdab4f8d" />

The query above is the relative recency within a closed dataset, here we put 500 in the recently active class, because it is closer to the minimum (369 days), and (800 days) in the  moderately inactive, closer to the average (737 days), and numbers above that we classify as highly inactive.

Due to the historical nature of the dataset (minimum recency = 369 days), standard 30/90-day activity thresholds were not applicable. Recency segmentation was instead derived using the nearest value distribution to create meaningful relative customer groupings.

