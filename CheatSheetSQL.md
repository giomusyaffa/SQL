# 🗃️ Things That SQL Can Do, Compiled in One File

> **Source:** [Retail Intelligence Fraud Detection Dataset Kaggle](https://www.kaggle.com/datasets/noopurbhatt/retail-intelligence-fraud-detection-dataset)

This cheat sheet covers practical SQL techniques I've been exploring, from ranking functions to date manipulation. More sections coming soon!

---

## 📋 Table of Contents
- [Window Functions](#-window-functions)
  - [ROW\_NUMBER()](#1-row_number)
  - [DENSE\_RANK()](#2-dense_rank)
  - [RANK()](#3-rank)
  - [PERCENT\_RANK()](#4-percent_rank)
  - [LAG()](#5-lag)
- [Date Functions](#-date-functions)
  - [DATEPART()](#6-datepart)
  - [DATENAME()](#7-datename)
  - [DATETRUNC()](#8-datetrunc)
  - [DATEDIFF()](#9-datediff)

---

## 🪟 Window Functions

Window functions let you perform calculations **across a set of rows related to the current row** — without collapsing them into a single output like `GROUP BY` does. Think of it as running an aggregation while still keeping every individual row visible.

**Scenario:** 

We have a table of transactions across different countries. 

Our goal is to rank which countries have the **highest average transaction amount**.

<img width="1761" height="361" alt="image" src="https://github.com/user-attachments/assets/2dbc82c2-a8a3-42da-b033-c922246d4980" />

---

### 1. ROW_NUMBER()

#### 💻 Code
```sql
SELECT 
    location,
    ROUND(AVG(transaction_amount), 0) AS avg_transaction_amt,
    ROW_NUMBER() OVER (ORDER BY ROUND(AVG(transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### 📊 Output
<img width="379" height="227" alt="image" src="https://github.com/user-attachments/assets/3e8d7be1-7c53-4623-90c6-6d332e7c27ac" />

#### 📝 Explanation
ROW_NUMBER() assigns a unique sequential number to every row, ordered by the column you specify. 
Even when two rows have the same value (USA and India both at 121), they still get different numbers.

### 2. DENSE_RANK()

#### 💻 Code
```sql
SELECT 
    location,
    ROUND(AVG(transaction_amount), 0) AS avg_transaction_amt,
    DENSE_RANK() OVER (ORDER BY ROUND(AVG(transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### 📊 Output
<img width="393" height="221" alt="image" src="https://github.com/user-attachments/assets/dc8aa5d9-7e86-437f-8013-5f5cf2de07ec" />

#### 📝 Explanation

DENSE_RANK() gives tied rows the same rank, 
and the next rank continues right after with no gaps. USA, India, and Germany all get rank 1, then UK gets rank 2 (not 4).

---

### 3. RANK()

#### 💻 Code
```sql
SELECT 
    location,
    ROUND(AVG(transaction_amount), 0) AS avg_transaction_amt,
    RANK() OVER (ORDER BY ROUND(AVG(transaction_amount), 0) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### 📊 Output
<img width="361" height="217" alt="image" src="https://github.com/user-attachments/assets/f0d8e120-eef1-449f-8588-2dce61f49a20" />

#### 📝 Explanation
RANK() also gives tied rows the same rank, but skips numbers afterward to account for the tie. 
USA, Germany, and India all get rank 1, so the next rank jumps to 4, not 2.

---

### 4. PERCENT_RANK()

#### 💻 Code
```sql
SELECT 
    location,
    ROUND(AVG(transaction_amount), 2) AS avg_transaction_amt,
    PERCENT_RANK() OVER (ORDER BY ROUND(AVG(transaction_amount), 2) DESC) AS ranks
FROM retail_fraud_detection_100k
GROUP BY location;
```

#### 📊 Output
<img width="365" height="213" alt="image" src="https://github.com/user-attachments/assets/70f34bf1-6ffa-4ae9-83e5-62e981162dbe" />


#### 📝 Explanation
PERCENT_RANK() shows each row's relative position as a value between 0 and 1, calculated as 

(RANK - 1) / (total rows - 1). 

The top row always gets 0.0 and the bottom always gets 1.0.

---

### 🔍 Bonus: Filtering Top N Results

What if you only want to see the **Top 3 countries**? Since you can't use a window function directly in a `WHERE` clause, wrap it in a subquery:

#### 💻 Code
```sql
SELECT *
FROM (
    SELECT 
        location,
        ROUND(AVG(transaction_amount), 2) AS avg_transaction_amt,
        ROW_NUMBER() OVER (ORDER BY ROUND(AVG(transaction_amount), 2) DESC) AS ranks
    FROM retail_fraud_detection_100k
    GROUP BY location
) AS tempo
WHERE ranks < 4;
```

#### 📊 Output
<img width="386" height="153" alt="image" src="https://github.com/user-attachments/assets/4a89e76e-3a8a-4307-b0e8-2f5fb454e6e2" />


#### 📝 Explanation
You can't filter on a window function directly in a WHERE clause, so we wrap the ranked query as a subquery first. 
The outer WHERE ranks < 4 then filters it down to the top 3.

---

### 5. LAG()

#### 💻 Code
```sql
SELECT
    customer_id,
    transaction_timestamp AS purchase_date,
    transaction_amount,
    LAG(transaction_amount) OVER (
        PARTITION BY customer_id
        ORDER BY transaction_timestamp DESC
    ) AS previous_spent
FROM retail_fraud_detection_100k;
```

#### 📊 Output
<img width="1252" height="439" alt="image" src="https://github.com/user-attachments/assets/f633dc03-c046-4b2f-9e6a-c8ca78ab0ac3" />


#### 📝 Explanation
LAG() pulls the value from the previous row within a partition. Here it shows each customer's prior transaction amount 
alongside their current one. The first transaction per customer returns NULL since there's nothing before it.

---

## 📅 Date Functions

Date functions allow you to extract, name, truncate, and compare date/time values stored in your database. 
They're essential for time-series analysis, reporting by period, and calculating durations.

---

### 6. DATEPART()

#### 💻 Code
```sql
SELECT 
    transaction_timestamp,
    DATEPART(MILLISECOND, transaction_timestamp) AS millisecond,
    DATEPART(MINUTE,      transaction_timestamp) AS minute,
    DATEPART(HOUR,        transaction_timestamp) AS hour,
    DATEPART(DAY,         transaction_timestamp) AS day,
    DATEPART(DAYOFYEAR,   transaction_timestamp) AS day_of_year,
    DATEPART(MONTH,       transaction_timestamp) AS month
FROM retail_fraud_detection_100k;
```

#### 📊 Output
<img width="1408" height="503" alt="image" src="https://github.com/user-attachments/assets/e297f754-09df-46e9-a198-65242a2be84e" />

#### 📝 Explanation
DATEPART() extracts a specific numeric component from a datetime, hour, day, month, etc. For example, 
DATEPART(MONTH, '2026-03-04') returns 3. Useful for grouping or filtering by a particular time unit.

---

### 7. DATENAME()

#### 💻 Code
```sql
SELECT 
    transaction_timestamp,
    DATENAME(WEEKDAY, transaction_timestamp) AS day,
    DATENAME(MONTH,   transaction_timestamp) AS month
FROM retail_fraud_detection_100k;
```

#### 📊 Output
<img width="1410" height="532" alt="image" src="https://github.com/user-attachments/assets/989e5181-6a1a-4be3-9c55-4ea852368064" />


#### 📝 Explanation
DATENAME() works like DATEPART(), but returns a string instead of a number. 
So rather than getting 3 for March, you get "March",  making outputs much more readable in reports.

---

### 8. DATETRUNC()

#### 💻 Code
```sql
SELECT 
    transaction_timestamp,
    DATETRUNC(MINUTE, transaction_timestamp) AS time_by_minute,
    DATETRUNC(HOUR,   transaction_timestamp) AS time_by_hour,
    DATETRUNC(DAY,    transaction_timestamp) AS time_by_day
FROM retail_fraud_detection_100k;
```

#### 📊 Output
<img width="1405" height="501" alt="image" src="https://github.com/user-attachments/assets/8f57cc57-c699-4bce-8225-50ec5effbda6" />


#### 📝 Explanation
DATETRUNC() snaps a datetime down to the start of the specified unit, minute, hour, day, etc. 
It returns a full datetime with the smaller units zeroed out, making it great for grouping transactions into time buckets.

---

### 9. DATEDIFF()

> **New dataset:** For this function, we use a patient appointment dataset that has two date columns: `booking_date` and `appointment_date`.

#### 💻 Code
```sql
SELECT 
    booking_date,
    appointment_date,
    DATEDIFF(DAY,   booking_date, appointment_date) AS difference_in_days,
    DATEDIFF(MONTH, booking_date, appointment_date) AS difference_in_months
FROM patient_no_show_dataset;
```

#### 📊 Output
<img width="1402" height="500" alt="image" src="https://github.com/user-attachments/assets/c9eb1a0c-3f6c-45c8-af52-aabab4d7179b" />


#### 📝 Explanation
DATEDIFF() returns the difference between two dates as an integer, in the unit you choose. 
Here it shows how many days (and months) each patient waited between booking and their appointment.

---

## 🚧 More Coming Soon

This cheat sheet is a work in progress! Next up:
- **CTEs** (Common Table Expressions)
- **JOINS** (INNER, LEFT, RIGHT, FULL OUTER)
- **Subqueries & Correlated Subqueries**
- **Aggregate Functions** (SUM, COUNT, AVG with GROUP BY)
- **CASE WHEN** statements
- **String Functions**

---
