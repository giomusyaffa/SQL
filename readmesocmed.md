# Step-by-Step Analysis on the Social Media Addiction & Mental Health Dataset 🧠📱

## Project Overview
This project analyzes a Social Media Addiction & Mental Health dataset from Kaggle using SQL Server.  
The goal is to demonstrate real-world SQL analysis across multiple interconnected tables, exploring how social media behavior, AI-driven algorithms, and digital habits impact global mental health outcomes.

---

## Dataset Description
- **Source:** [Kaggle — Social Media Addiction & Mental Health Dataset](https://www.kaggle.com/datasets/abdulmaliklodhra/social-media-addiction-and-mental-health-dataset)
- **Timeframe:** 2010 – 2060 (historical + forecasted data)
- **Size:** ~25,000 rows across 10 CSV files, 113 columns
- **Tool Used:** Microsoft SQL Server (SSMS)

### Tables
| Table | Description |
|---|---|
| `mental_health_trends` | Anxiety, depression, stress scores by user |
| `social_media_usage` | Platform, screen time, doomscrolling behavior |
| `screen_time_behavior` | Focus span, multitasking, app switching |
| `sleep_disruption` | Sleep hours, quality, insomnia risk |
| `teen_behavior_patterns` | Social comparison, peer pressure, body image |
| `dopamine_trigger_metrics` | Reward loops, dopamine trigger scores |
| `ai_recommendation_impact` | Algorithmic manipulation & engagement metrics |
| `digital_detox_behavior` | Detox duration, success, wellbeing improvement |
| `cyberbullying_impact` | Exposure, self-harm risk, emotional resilience |
| `future_psychological_forecast` | Predictive mental health indicators (2030–2060) |

---

## SQL Analysis / Queries

### 1. 
A mental health research team wants to analyze whether heavy social media usage is associated with higher anxiety levels. 

Write a query to categorize users based on daily screen time:
- Less than 2 hours → 'Low Usage'
- Between 2 and 5 hours → 'Medium Usage'
- More than 5 hours → 'Heavy Usage'

Tables Used:
- mental_health_trends
- social_media_usage

```sql
SELECT 
	AVG (m.anxiety_score) AS average_anxiety_score , 
	COUNT (*) AS total_users, 
	CASE
		WHEN s.daily_screen_time_hours < 2 THEN 'Low Usage'
		WHEN s.daily_screen_time_hours BETWEEN 2 AND 5 THEN 'Medium Usage'
		WHEN s.daily_screen_time_hours > 5 THEN 'Heavy Usage'
	END AS 'Usage_Catergory'
FROM mental_health_trends m 
JOIN social_media_usage s
ON m.user_id = s.user_id
GROUP BY 
	CASE
		WHEN s.daily_screen_time_hours < 2 THEN 'Low Usage'
		WHEN s.daily_screen_time_hours BETWEEN 2 AND 5 THEN 'Medium Usage'
		WHEN s.daily_screen_time_hours > 5 THEN 'Heavy Usage'
	END
ORDER BY average_anxiety_score DESC;
```
<img width="959" height="539" alt="image" src="https://github.com/user-attachments/assets/95ece01f-0038-446a-bfbf-00d2532392b6" />



---

### 2. 
A digital wellness company wants to identify which social media platform is associated with the highest depression levels. Write a query to return:

- platform name
- average depression score
- average daily screen time
- total users

Sort the results by average depression score in descending order.

Tables Used:
- social_media_usage
- mental_health_trends

```sql
SELECT 
m.platform,
COUNT (*) AS Total_Users,
AVG (m.depression_score) AS average_depres_score,
AVG (s.daily_screen_time_hours) AS average_daily_screen_time
FROM mental_health_trends m
JOIN social_media_usage s
ON m.user_id = s.user_id
GROUP BY m.platform
ORDER BY average_depres_score DESC;
```
<img width="959" height="539" alt="image" src="https://github.com/user-attachments/assets/f4787764-ef8d-40c1-b884-33acc5310cae" />

---

### 3. 
A psychologist wants to identify users whose depression score is above the global average depression score. Write a query to return:

- user_id
- depression_score

Sort the results by depression score in descending order.

Tables Used:
- mental_health_trends
```sql
SELECT
    user_id,
    depression_score
FROM mental_health_trends
WHERE depression_score > (SELECT AVG(depression_score) FROM mental_health_trends)
ORDER BY depression_score DESC;
```
<img width="1406" height="937" alt="image" src="https://github.com/user-attachments/assets/93702187-8046-46ac-8231-a3239ca272c5" />

---

### 4.
A sleep research institute wants to investigate whether doomscrolling behavior affects sleep quality. Write a query to return:

- average doomscrolling frequency
- average sleep quality score
- average insomnia risk score

using the relevant tables.

Tables Used:
- social_media_usage
- sleep_disruption

```sql
SELECT 
AVG (smu.doomscrolling_frequency) AS doomsc_freq, 
AVG (sd.sleep_quality_score) AS sleep_qual_scr,
AVG (sd.fatigue_level) AS fatigue_lvl
FROM social_media_usage smu
JOIN sleep_disruption sd
ON smu.user_id = sd.user_id;
```
<img width="1400" height="940" alt="image" src="https://github.com/user-attachments/assets/cab30bcc-adf6-4538-aea5-5469d8572507" />

---

### 5. 
A social media company wants to determine which platform has the highest average anxiety levels among users. Write a query to return:

- platform
- average anxiety score
- average daily screen time
- total users

Sort the results by average anxiety score in descending order.

Tables Used:
- social_media_usage
- mental_health_trends

```sql
SELECT
    sm.platform,
    AVG(m.anxiety_score) AS avg_anxiety,
    AVG(sm.daily_screen_time_hours) AS avg_screen_time,
    COUNT(*) AS total_users
FROM social_media_usage sm
JOIN mental_health_trends m ON sm.user_id = m.user_id
GROUP BY sm.platform
ORDER BY avg_anxiety DESC;
```
<img width="1403" height="942" alt="image" src="https://github.com/user-attachments/assets/4c14d709-0300-4c38-b85e-97b8e22a05f9" />

---

### 6. 
A digital detox startup wants to identify users who successfully completed a detox program. Write a query to return:

- user_id
- detox duration
- wellbeing improvement score
- anxiety score
- depression score

Only include users whose detox was successful. Sort by highest wellbeing improvement score first.

Tables Used:
- digital_detox_behavior
- mental_health_trends

```sql
SELECT 
	d.user_id, 
	d.offline_activity_hours_weekly, 
	d.wellbeing_improvement_score,
	m.anxiety_score,
	m.depression_score
FROM mental_health_trends m
JOIN digital_detox_behavior d
ON m.user_id = d.user_id
WHERE d.successful_detox = '1'
ORDER BY d.wellbeing_improvement_score DESC;

```
<img width="1407" height="936" alt="image" src="https://github.com/user-attachments/assets/5d476e65-0011-46d5-8e96-a5b1e68794c1" />

---

### 7.
A mental health organization wants to identify users who never attempted a digital detox program. Write a query to return:

- user_id
- anxiety score
- depression score

using the appropriate join logic.

Tables Used:
- mental_health_trends
- digital_detox_behavior
  
```sql
SELECT
    m.user_id,
    m.anxiety_score,
    m.depression_score
FROM mental_health_trends m
LEFT JOIN digital_detox_behavior d ON m.user_id = d.user_id
WHERE d.user_id IS NULL;
```
<img width="1402" height="934" alt="image" src="https://github.com/user-attachments/assets/53627c30-7e7e-4dfb-90b0-4bb297496e44" />


---

### 8. 
A global research institute wants to compare mental health trends across countries. Write a query to return:

- country
- average depression score
- average anxiety score

Sort by highest average depression score first.

Tables Used:
mental_health_trends

```sql
SELECT
	country,
	AVG (depression_score) AS Average_Depr_scr,
	AVG (anxiety_score) AS Average_anxiety_scr
FROM mental_health_trends
GROUP BY country
ORDER BY Average_anxiety_scr DESC;
```
<img width="1409" height="929" alt="image" src="https://github.com/user-attachments/assets/d14d938b-64c8-4080-8b04-b206bc073621" />

---

### 9. 
A youth behavior study wants to analyze whether social comparison behavior increases body image anxiety among teenagers. Write a query to return:

- average social comparison index
- average body image anxiety score
- average peer pressure score

Tables Used:
teen_behavior_patterns

```sql
SELECT
	AVG (social_comparison_index) AS avg_comp_index,
	AVG (body_image_anxiety_score) AS avg_body_img_anx_scr,
	AVG (peer_pressure_score) AS avg_peer_press_scr
FROM teen_behavior_patterns;
```
<img width="1409" height="941" alt="image" src="https://github.com/user-attachments/assets/b31f525e-a08c-4035-bc0b-65242c597bd2" />

---

### 10. 
A global mental health institute wants to rank countries based on stress levels. Write a query to return:

- user_id
- stress_level
- rank based on highest anxiety score

using an appropriate window function.

Tables Used:
mental_health_trends

```sql
SELECT
	country,
	ROUND (AVG (stress_level), 1) AS score,
	ROW_NUMBER() OVER(ORDER BY ROUND (AVG (stress_level), 1) DESC) AS row_number,
	RANK() OVER(ORDER BY ROUND (AVG (stress_level), 1) DESC) AS rank,
	DENSE_RANK() OVER(ORDER BY ROUND (AVG (stress_level), 1) DESC) AS dense_rank
FROM mental_health_trends
GROUP BY country
ORDER BY score DESC;
```
<img width="1385" height="941" alt="image" src="https://github.com/user-attachments/assets/378768ec-d385-49f6-8116-9ec93d02c657" />

---

### 12. Top 3 Most Anxious Users Per Country
```sql
WITH RankedUsers AS (
    SELECT
        user_id,
        country,
        anxiety_score,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY anxiety_score DESC) AS rn
    FROM mental_health_trends
)
SELECT user_id, country, anxiety_score
FROM RankedUsers
WHERE rn <= 3;
```

---

### 13. Compare Anxiety vs Depression (UNION ALL)
```sql
SELECT user_id, anxiety_score AS score, 'Anxiety' AS metric
FROM mental_health_trends
UNION ALL
SELECT user_id, depression_score, 'Depression'
FROM mental_health_trends;
```

---

### 14. Average Sleep by Screen Time Category
```sql
SELECT
    CASE
        WHEN sm.daily_screen_time_hours < 2 THEN 'Low Usage'
        WHEN sm.daily_screen_time_hours BETWEEN 2 AND 5 THEN 'Medium Usage'
        ELSE 'Heavy Usage'
    END AS usage_category,
    AVG(sl.avg_sleep_hours) AS avg_sleep_hours,
    AVG(sl.sleep_quality_score) AS avg_sleep_quality
FROM social_media_usage sm
JOIN sleep_disruption sl ON sm.user_id = sl.user_id
GROUP BY
    CASE
        WHEN sm.daily_screen_time_hours < 2 THEN 'Low Usage'
        WHEN sm.daily_screen_time_hours BETWEEN 2 AND 5 THEN 'Medium Usage'
        ELSE 'Heavy Usage'
    END
ORDER BY avg_sleep_hours ASC;
```

---

### 15. Users With High AI Manipulation Risk
```sql
SELECT TOP 20
    ai.user_id,
    ai.emotional_manipulation_index,
    ai.content_personalization_level,
    ai.engagement_boost_factor
FROM ai_recommendation_impact ai
ORDER BY ai.emotional_manipulation_index DESC;
```

---

### 16. Dopamine Triggers vs Sleep & Anxiety
```sql
SELECT
    d.user_id,
    d.dopamine_trigger_score,
    sl.sleep_quality_score,
    m.anxiety_score
FROM dopamine_trigger_metrics d
JOIN sleep_disruption sl ON d.user_id = sl.user_id
JOIN mental_health_trends m ON d.user_id = m.user_id
ORDER BY d.dopamine_trigger_score DESC;
```

---

### 17. Yearly Mental Health Trend
```sql
SELECT
    year,
    AVG(anxiety_score) AS avg_anxiety,
    AVG(depression_score) AS avg_depression,
    AVG(stress_level) AS avg_stress
FROM mental_health_trends
GROUP BY year
ORDER BY year ASC;
```

---

### 18. Countries With Average Anxiety Above 70
```sql
SELECT
    country,
    AVG(anxiety_score) AS avg_anxiety
FROM mental_health_trends
GROUP BY country
HAVING AVG(anxiety_score) > 70
ORDER BY avg_anxiety DESC;
```

---

### 19. High-Risk Users: High Dopamine + Poor Sleep
```sql
WITH Addiction AS (
    SELECT user_id, dopamine_trigger_score, reward_loop_frequency
    FROM dopamine_trigger_metrics
),
SleepRisk AS (
    SELECT user_id, insomnia_risk_score, sleep_quality_score
    FROM sleep_disruption
)
SELECT
    a.user_id,
    a.dopamine_trigger_score,
    s.insomnia_risk_score,
    s.sleep_quality_score
FROM Addiction a
JOIN SleepRisk s ON a.user_id = s.user_id
WHERE a.dopamine_trigger_score > 80
  AND s.insomnia_risk_score > 70
ORDER BY a.dopamine_trigger_score DESC;
```

---

### 20. All Detox Records Including Missing Mental Health Data (RIGHT JOIN)
```sql
SELECT
    m.user_id,
    m.anxiety_score,
    d.detox_duration_days
FROM mental_health_trends m
RIGHT JOIN digital_detox_behavior d ON m.user_id = d.user_id;
```

---

## Key Concepts Demonstrated
- `JOIN`, `LEFT JOIN`, `RIGHT JOIN` across multiple tables
- Aggregate functions: `AVG()`, `COUNT()`
- Subqueries and `WITH` (CTEs)
- Window functions: `RANK()`, `ROW_NUMBER() OVER (PARTITION BY ...)`
- `CASE WHEN` for custom categorization
- `UNION ALL` for combining result sets
- `HAVING` for post-aggregation filtering
