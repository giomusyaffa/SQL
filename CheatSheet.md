# Things that SQL can do, compiled in one file.
SOURCE : https://www.kaggle.com/datasets/noopurbhatt/retail-intelligence-fraud-detection-dataset

<img width="1761" height="361" alt="image" src="https://github.com/user-attachments/assets/2dbc82c2-a8a3-42da-b033-c922246d4980" />

## WINDOW FUNCTIONS
let's say we have a table consisting a list of countries with it respecting average transactions, and we want to make a ranking of which countries have the highest amount of transactions

<img width="1269" height="537" alt="image" src="https://github.com/user-attachments/assets/806a839a-89d9-482c-8f24-cc5712df005c" />



### 1. ROW()(NUMBER)
#### Code
 ```sql
SELECT 
location,
ROUND (AVG (transaction_amount),0) AS avg_transaction_amt,
ROW_NUMBER()OVER(ORDER BY ROUND (AVG (transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### Output
<img width="379" height="227" alt="image" src="https://github.com/user-attachments/assets/3e8d7be1-7c53-4623-90c6-6d332e7c27ac" />


#### Explanation
Here we can see, by using 

### 2. DENSE_RANK()
#### Code
 ```sql
SELECT 
location,
ROUND (AVG (transaction_amount),0) AS avg_transaction_amt,
DENSE_RANK()OVER(ORDER BY ROUND (AVG (transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### Output
<img width="393" height="221" alt="image" src="https://github.com/user-attachments/assets/dc8aa5d9-7e86-437f-8013-5f5cf2de07ec" />

#### Explanation
The rank of a row is defined as 1 plus the number rankings that precede the ranking of the row. If two or more rows have the same value, these rows get the same rank. However, in contrast to the RANK function, if two or more rows tie, there is no gap in the sequence of ranked values. For example, if two rows are ranked 1, the next ranking is still 2.

### 3. RANK()
#### Code
 ```sql
SELECT 
location,
ROUND (AVG (transaction_amount),0) AS avg_transaction_amt,
RANK()OVER(ORDER BY ROUND (AVG (transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### Output
<img width="361" height="217" alt="image" src="https://github.com/user-attachments/assets/f0d8e120-eef1-449f-8588-2dce61f49a20" />

#### Explanation
The rank of a row is defined as 1 plus the number of rows whose rankings precede that of the row. If two or more rows have the same value, these rows get the same rank as well. Results can have a gap in the sequence of consecutive ranked values. For example, if two rows are ranked 1, the next ranking is 3. The DENSE_RANK function uses a different rule for ranking rows that include non-unique values.

### 4. PERCENT_RANK()
#### Code
 ```sql
SELECT 
location,
ROUND (AVG (transaction_amount),2) AS avg_transaction_amt,
PERCENT_RANK()OVER(ORDER BY ROUND (AVG (transaction_amount), 2) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### Output
<img width="365" height="213" alt="image" src="https://github.com/user-attachments/assets/70f34bf1-6ffa-4ae9-83e5-62e981162dbe" />


#### Explanation
The PERCENT_RANK function is an OLAP ranking function that calculates a ranking value for each row in an OLAP window, normalized to a range from 0 to 1.

Each PERCENT_RANK value is computed as the row's RANK minus 1, divided by the number of rows in the partition minus 1. Values closer to 1 generally represent higher rankings and values closer to 0 generally represent lower rankings.

### Now what if you want to see the Top 3 Countries?

#### Code
 ```sql
SELECT *
FROM (SELECT 
location,
ROUND (AVG (transaction_amount),2) AS avg_transaction_amt,
ROW_NUMBER()OVER(ORDER BY ROUND (AVG (transaction_amount), 2) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location) AS tempo
WHERE ranks < 4;
```

#### Output
<img width="386" height="153" alt="image" src="https://github.com/user-attachments/assets/4a89e76e-3a8a-4307-b0e8-2f5fb454e6e2" />



#### Explanation
Here we can see, by using 

### 5. LAG()


#### Code
 ```sql
SELECT
    customer_id,
    transaction_timestamp AS last_purchase_date,
    transaction_amount,

    LAG(transaction_amount) OVER(
        PARTITION BY customer_id
        ORDER BY transaction_timestamp DESC
    ) AS PreviousSpent

FROM retail_fraud_detection_100k;
```

#### Output
<img width="1252" height="439" alt="image" src="https://github.com/user-attachments/assets/f633dc03-c046-4b2f-9e6a-c8ca78ab0ac3" />





#### Explanation
Here we can see, by using 

