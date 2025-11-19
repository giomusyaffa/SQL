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
  - `transaction_id` - Unique ID for each transaction  
  - `account_id` - Account initiating the transaction  
  - `amount` - Transaction amount  
  - `transaction_time` - Timestamp of the transaction  
  - `payment_type` - Method / type of the transaction (if available)  
  - `is_fraud` - Binary label: `1` = fraudulent, `0` = normal

## SQL Analysis / Queries  
1. **Basic Fraud Metrics:** total transactions, ratio of fraud  
2. **Amount Distribution:** categorize amounts into buckets (e.g. <100, 100–1k, etc.)  
3. **Time Trend Analysis:** fraud trend by date  
4. **High-Risk Accounts:** identify accounts with high fraud rate  
5. **Payment Type Risk:** see which payment methods are more vulnerable to fraud  

## Step 1
Import the .csv file into the dataset that you made in SQL

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b37adb11-db72-40e9-aa68-3cc3d23410dc" />

As you can see from the picture above I already made my dataset for this project called "Portfolio", and then to import the .csv file that you've download from Kaggle, go to tasks, and import flat file.

<img width="1161" height="1075" alt="image" src="https://github.com/user-attachments/assets/1c84cf5e-9f21-4759-9871-a159a117b77f" />

Then choose your file, and it's going to show something like this:

<img width="1160" height="1077" alt="image" src="https://github.com/user-attachments/assets/8a15abe4-f915-48b9-a676-0bac1ac74d73" />

