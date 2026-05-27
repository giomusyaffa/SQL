# A couple of things that SQL can do, compiled in one file.
### Dataset
Source:
https://www.kaggle.com/datasets/noopurbhatt/retail-intelligence-fraud-detection-dataset

---

### Table Schema

| Column Name                      | Data Type    | Description                                 |
| -------------------------------- | ------------ | ------------------------------------------- |
| transaction_id                   | NVARCHAR(50) | Unique transaction identifier               |
| customer_id                      | NVARCHAR(50) | Unique customer identifier                  |
| transaction_timestamp            | NVARCHAR(50) | Date and time of transaction                |
| transaction_amount               | FLOAT        | Monetary value of transaction               |
| payment_method                   | NVARCHAR(50) | Payment method used                         |
| device_type                      | NVARCHAR(50) | Device used for transaction                 |
| location                         | NVARCHAR(50) | Transaction location/country                |
| merchant_category                | NVARCHAR(50) | Merchant business category                  |
| is_international                 | BIT          | Indicates international transaction         |
| transaction_frequency_24h        | TINYINT      | Number of transactions in last 24h          |
| avg_transaction_amount_7d        | FLOAT        | Average transaction amount over 7 days      |
| failed_transaction_count_24h     | TINYINT      | Failed transactions in last 24h             |
| account_age_days                 | SMALLINT     | Customer account age in days                |
| previous_fraud_flag              | BIT          | Indicates previous fraud history            |
| unusual_amount_flag              | BIT          | Flags unusual transaction amount            |
| unusual_location_flag            | BIT          | Flags suspicious location                   |
| multiple_transactions_short_time | BIT          | Multiple rapid transactions indicator       |
| high_risk_device_flag            | BIT          | Indicates risky device usage                |
| velocity_flag                    | BIT          | High transaction velocity indicator         |
| fraud_flag                       | BIT          | Fraud indicator (1 = Fraud, 0 = Legitimate) |
| fraud_risk                       | NVARCHAR(50) | Fraud risk classification                   |


---
## 1. WINDOW FUNCTIONS
### ROW()(NUMBER)

#### Code
 ```sql
SELECT *
FROM
(SELECT customer_id , transaction_amount,  ROW_NUMBER()OVER(ORDER BY transaction_amount DESC) AS rank
FROM retail_fraud_detection_100k) ranks
WHERE rank < 6;
```

#### Output
<img width="397" height="243" alt="image" src="https://github.com/user-attachments/assets/ebc9ca4c-266e-4a0b-8233-3246ca3e6d03" />

#### Explanation
Here we can see, by using 

