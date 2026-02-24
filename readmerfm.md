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

#### C. Invalid Country
First, let's find the Invalid Values
<img width="1497" height="938" alt="image" src="https://github.com/user-attachments/assets/ee500180-0407-4ff5-8414-eb242f6e50fd" />

Then, we can replace 
