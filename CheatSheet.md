# A couple of things that SQL can do, compiled in one file.

## 1. WINDOW FUNCTIONS
let's say we have a table consisting a list of countries with it respecting average transactions, and we want to make a ranking of which countries have the highest amount of transactions

<img width="893" height="530" alt="image" src="https://github.com/user-attachments/assets/d7972e0a-cfad-44e6-82fa-757569e17940" />

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

