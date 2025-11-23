# Step-by-Step Analysis on The IBM Transactions for Anti Money Laundering (AML) 💰

## Project Overview  
This project uses a **Fraudulent Transactions** dataset from Kaggle to perform data analysis and risk monitoring using SQL. The main goals are to:  
- Explore transaction behavior  
- Identify fraud patterns  
- Build basic fraud metrics and KPIs  
- Demonstrate real-world SQL usage in financial risk analytics  

## Dataset Description  
- **Source:** Kaggle - Fraudulent Transactions Data  
- **Data Size:** Transaction-level data  
- **Key Features:**
  - transaction_id - Unique ID for each transaction  
  - account_id - Account initiating the transaction  
  - amount - Transaction amount  
  - transaction_time - Timestamp of the transaction  
  - payment_type - Method / type of the transaction (if available)  
  - is_fraud - Binary label: 1 = fraudulent, 0 = normal

## SQL Analysis / Queries  
1. **Basic Fraud Metrics:** total transactions, ratio of fraud  
2. **Amount Distribution:** categorize amounts into buckets (e.g. <100, 100–1k, etc.)  
3. **Time Trend Analysis:** fraud trend by date  
4. **High-Risk Accounts:** identify accounts with high fraud rate  
5. **Payment Type Risk:** see which payment methods are more vulnerable to fraud  

## Step 1: Import the Data
Import the .csv file into the dataset that you made in SQL

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ff6d1c31-0e24-44cb-aabc-890ff697b3b1" />


As you can see from the picture above I already made my dataset for this project called "Portfolio", and then to import the .csv file that you've download from Kaggle, go to tasks, and import flat file.

Then, after inputting the file you would see something like this

<img width="1919" height="931" alt="image" src="https://github.com/user-attachments/assets/fe8986c3-4ad1-422f-b60d-c8c0360ce807" /><br />

Now, let's start writing the queries.

## Step 2: Basic Keywords (SELECT, WHERE , ORDER BY, GROUP BY)
### 1. SELECT FROM
Starting from the very basics of SQL is `SELECT`, this keyword basically "selects" what you wanna see from your table, for example:

<img width="1919" height="947" alt="image" src="https://github.com/user-attachments/assets/f76aa790-a061-46ae-a49f-88587f272143" />

This query above shows  the customer_id, gender, and sentiment form the data. Simple right?

