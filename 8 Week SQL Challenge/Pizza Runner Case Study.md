# 🍕 Case Study: [Pizza Runner](https://8weeksqlchallenge.com/case-study-2/)


### 🗂️ Table of Contents  
• <i> [Business Use Case](#Business_Use_Case) </i>  
• <i> [ERD](#ERD) </i>  
• <i> [Dataset Schema SQL Code](#Dataset_Schema_SQL_Code) </i>  
• <i> [Questions & Solutions](#Questions_&_Solutions) </i>   
• <i> [Business Recommendations](#Business_Recommendations) </i> 
  
  ---
### Business Use Case
Pizza Runner is a startup delivering pizza through freelance runners using a custom-built mobile app. Danny, the founder, wants to analyze customer orders, runner logistics, and pizza customizations to improve delivery efficiency, understand customer preferences, and refine the menu. He has collected data on orders, runners, and ingredients and needs SQL queries to generate insights and build simple datasets for better business decisions.

  ---
### ERD
<p align="center" width="100%">
    <img width="70%" src="https://github.com/user-attachments/assets/f9839556-459d-45eb-907c-4482bb89e469">
</p>


  ---
  
### Dataset Schema SQL Code

**Schema (PostgreSQL v13)**
<ul>
	
 ```sql
CREATE SCHEMA pizza_runner;
SET search_path = pizza_runner;

DROP TABLE IF EXISTS runners;
CREATE TABLE runners (
  "runner_id" INTEGER,
  "registration_date" DATE
);
INSERT INTO runners
  ("runner_id", "registration_date")
VALUES
  (1, '2021-01-01'),
  (2, '2021-01-03'),
  (3, '2021-01-08'),
  (4, '2021-01-15');


DROP TABLE IF EXISTS customer_orders;
CREATE TABLE customer_orders (
  "order_id" INTEGER,
  "customer_id" INTEGER,
  "pizza_id" INTEGER,
  "exclusions" VARCHAR(4),
  "extras" VARCHAR(4),
  "order_time" TIMESTAMP
);

INSERT INTO customer_orders
  ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
VALUES
  ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
  ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
  ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
  ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
  ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
  ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
  ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
  ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
  ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
  ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
  ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');


DROP TABLE IF EXISTS runner_orders;
CREATE TABLE runner_orders (
  "order_id" INTEGER,
  "runner_id" INTEGER,
  "pickup_time" VARCHAR(19),
  "distance" VARCHAR(7),
  "duration" VARCHAR(10),
  "cancellation" VARCHAR(23)
);

INSERT INTO runner_orders
  ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
VALUES
  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


DROP TABLE IF EXISTS pizza_names;
CREATE TABLE pizza_names (
  "pizza_id" INTEGER,
  "pizza_name" TEXT
);
INSERT INTO pizza_names
  ("pizza_id", "pizza_name")
VALUES
  (1, 'Meatlovers'),
  (2, 'Vegetarian');


DROP TABLE IF EXISTS pizza_recipes;
CREATE TABLE pizza_recipes (
  "pizza_id" INTEGER,
  "toppings" TEXT
);
INSERT INTO pizza_recipes
  ("pizza_id", "toppings")
VALUES
  (1, '1, 2, 3, 4, 5, 6, 8, 10'),
  (2, '4, 6, 7, 9, 11, 12');


DROP TABLE IF EXISTS pizza_toppings;
CREATE TABLE pizza_toppings (
  "topping_id" INTEGER,
  "topping_name" TEXT
);
INSERT INTO pizza_toppings
  ("topping_id", "topping_name")
VALUES
  (1, 'Bacon'),
  (2, 'BBQ Sauce'),
  (3, 'Beef'),
  (4, 'Cheese'),
  (5, 'Chicken'),
  (6, 'Mushrooms'),
  (7, 'Onions'),
  (8, 'Pepperoni'),
  (9, 'Peppers'),
  (10, 'Salami'),
  (11, 'Tomatoes'),
  (12, 'Tomato Sauce');
```
</ul>

---

### Questions & Solutions

For each question, I have included a SQL Query Solution, its output, and a brief response to the information requested.  
  
**➡️ Data Cleaning and Manipulation ⬅️**  
Prior to beginning the analysis and answering any questions, there are two tables that need some work done! Below it can be seen that the first table that needs to be cleaned is the *customer_orders* table:

<p align="center" width="100%">
    <img width="100%" src="https://github.com/user-attachments/assets/893f9af0-d1d9-4184-8f3e-36a09030dddf">
</p>
  
From the screenshot, we can see that there are blank spaces as well as null values in both the *exclusions* and the *extras* columns, which needs to be standardized. For that, I created a temporary table from the original *customer_orders* table and replaced any blank or non-standard values in the two aforementioned columns with NULL:
```sql
CREATE TEMP TABLE customer_orders_temp AS
SELECT 
  order_id, 
  customer_id, 
  pizza_id, 

  CASE
    WHEN TRIM(LOWER(exclusions)) = 'null' OR TRIM(exclusions) = '' THEN NULL
    ELSE TRIM(exclusions)
  END AS exclusions,

  CASE
    WHEN TRIM(LOWER(extras)) = 'null' OR TRIM(extras) = '' THEN NULL
    ELSE TRIM(extras)
  END AS extras,

  order_time

FROM pizza_runner.customer_orders;
```
The second table that needs to be cleaned is the *runner_orders* table. It displays the same issue as the previous table, with blank spaces and null values in the same column, which were all replaced with NULL for standardization. On that table, the data types in the *pickup_time*, *distance*, and *duration* columns must also be altered:


<p align="center" width="100%">
    <img width="100%" src="https://github.com/user-attachments/assets/8722e1c2-fd3e-4ad4-a629-80ec9267bc38">
</p>

For that, a similar approach was used:
```sql
CREATE TEMP TABLE runner_orders_temp AS
SELECT 
  order_id, 
  runner_id,  

  CASE
    WHEN pickup_time ILIKE 'null' OR TRIM(pickup_time) = '' THEN NULL
    ELSE CAST(pickup_time AS TIMESTAMP)
  END AS pickup_time,

  CASE
    WHEN distance ILIKE 'null' OR TRIM(distance) = '' THEN NULL
    WHEN distance LIKE '%km%' THEN CAST(REPLACE(REPLACE(distance, 'km', ''), ' ', '') AS FLOAT)
    ELSE CAST(distance AS FLOAT)
  END AS distance_km,

  CASE
    WHEN duration ILIKE 'null' OR TRIM(duration) = '' THEN NULL
    WHEN duration LIKE '%minutes%' THEN CAST(TRIM(REPLACE(duration, 'minutes', '')) AS INTEGER)
    WHEN duration LIKE '%minute' THEN CAST(TRIM(REPLACE(duration, 'minute', '')) AS INTEGER)
    WHEN duration LIKE '%mins' THEN CAST(TRIM(REPLACE(duration, 'mins', '')) AS INTEGER)
    ELSE CAST(duration AS INTEGER)
  END AS duration_mins,

  CASE
    WHEN cancellation ILIKE 'null' OR TRIM(cancellation) = '' THEN NULL
    ELSE cancellation
  END AS cancellation

FROM pizza_runner.runner_orders;
```
These temp tables will be used for the queries answering the case questions.

  ---
  
**➡️ Pizza Metrics Questions ⬅️**  
  
**1️⃣ How many pizzas were ordered?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT COUNT(*) AS total_pizzas_ordered
FROM customer_orders_temp;
```
• **Output:**

| total_pizzas_ordered |
| -------------------- |
| 14                   |


• **Response:**
14 pizzas were ordered in total.
</ul>

---

**2️⃣ How many unique customer orders were made?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT COUNT(DISTINCT order_id)
FROM customer_orders_temp;
```
• **Output:**  
| count |
| ----- |
| 10    |
  
• **Response:**
10 unique customer orders were made.
</ul>

---

**3️⃣ How many successful orders were delivered by each runner?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT runner_id, COUNT(order_id) AS successful_orders
FROM runner_orders_temp
WHERE cancellation IS NULL
GROUP BY runner_id;
```
• **Output:**  
| runner_id | successful_orders |
| --------- | ----------------- |
| 1         | 4                 |
| 2         | 3                 |
| 3         | 1                 |
  
• **Response:**
4 orders were successfully completed by runner_id 1, 3 orders by runner_id 2, and 1 order by runner_id 3.
</ul>

---

**4️⃣ How many of each type of pizza was delivered?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT pizza_names.pizza_name, COUNT(customer_orders_temp.pizza_id) AS pizzas_delivered
FROM customer_orders_temp
JOIN pizza_names ON pizza_names.pizza_id = customer_orders_temp.pizza_id
JOIN runner_orders_temp ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.cancellation IS NULL
GROUP BY pizza_names.pizza_name;
```
• **Output:**
| pizza_name | pizzas_delivered |
| ---------- | ---------------- |
| Meatlovers | 9                |
| Vegetarian | 3                |
  
• **Response:**  
There were 9 meatlovers pizzas delivered, and 3 vegetarian pizzas.
</ul>

---

**5️⃣ How many Vegetarian and Meatlovers were ordered by each customer?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT customer_orders_temp.customer_id, pizza_names.pizza_name, COUNT(customer_orders_temp.pizza_id) AS pizzas_ordered
FROM customer_orders_temp
JOIN pizza_names ON pizza_names.pizza_id = customer_orders_temp.pizza_id
JOIN runner_orders_temp ON customer_orders_temp.order_id = runner_orders_temp.order_id
GROUP BY customer_orders_temp.customer_id,pizza_names.pizza_name
ORDER BY customer_orders_temp.customer_id ASC;
```
• **Output:**  
| customer_id | pizza_name | pizzas_ordered |
| ----------- | ---------- | -------------- |
| 101         | Meatlovers | 2              |
| 101         | Vegetarian | 1              |
| 102         | Meatlovers | 2              |
| 102         | Vegetarian | 1              |
| 103         | Meatlovers | 3              |
| 103         | Vegetarian | 1              |
| 104         | Meatlovers | 3              |
| 105         | Vegetarian | 1              |
  
• **Response:**
Customer_id 101 ordered 2 meatlovers pizzas and 1 vegetarian; customer_id 102 ordered 2 meatlovers pizzas and 1 vegeratian; customer_id 103 ordered 3 meatlovers pizzas and 1 vegeratian; customer_id 104 ordered 3 meatlovers pizzas; and customer_id 105 ordered 1 vegetarian pizza.
</ul>

---

**6️⃣ What was the maximum number of pizzas delivered in a single order?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT customer_orders_temp.order_id, COUNT(customer_orders_temp.pizza_id) AS pizza_count
FROM customer_orders_temp
JOIN runner_orders_temp ON customer_orders_temp.order_id=runner_orders_temp.order_id
WHERE runner_orders_temp.cancellation IS NULL
GROUP BY customer_orders_temp.order_id
ORDER BY pizza_count DESC
LIMIT 1;
```
• **Output:**  
| order_id | pizza_count |
| -------- | ----------- |
| 4        | 3           |
  
• **Response:**
The maximum number if pizzas in a single order was 3 pizzas.
</ul>

---

**7️⃣ For each customer, how many delivered pizzas had at least 1 change and how many had no changes?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT customer_orders_temp.customer_id,
COUNT(CASE 
    WHEN customer_orders_temp.exclusions IS NOT NULL 
      OR customer_orders_temp.extras IS NOT NULL 
    THEN 1 END) AS changed_pizzas,
COUNT(CASE 
    WHEN customer_orders_temp.exclusions IS NULL 
      AND customer_orders_temp.extras IS NULL 
    THEN 1 END) AS no_change_pizzas
FROM customer_orders_temp
JOIN runner_orders_temp ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.cancellation IS NULL
GROUP BY customer_orders_temp.customer_id;
```
• **Output:**  
| customer_id | changed_pizzas | no_change_pizzas |
| ----------- | -------------- | ---------------- |
| 101         | 0              | 2                |
| 102         | 0              | 3                |
| 103         | 3              | 0                |
| 104         | 2              | 1                |
| 105         | 1              | 0                |
  
• **Response:**
Customer_id 1 had 0 changed pizzas and 2 unchanged pizzas; 102 had 0 changed pizzas and 3 unchanged pizzas; 103 had 3 changed pizzas and 0 unchanged pizzas; 104 had 2 changed pizzas and 1 unchanged pizza; and 105 had 1 changed pizza and 0 unchanged pizzas.
</ul>

---

**8️⃣ How many pizzas were delivered that had both exclusions and extras?**  
<ul>
	
• **SQL Query Solution:**
```sql
SELECT COUNT(*) AS pizzas_with_both
FROM customer_orders_temp
JOIN runner_orders_temp ON runner_orders_temp.order_id = customer_orders_temp.order_id                                     
WHERE runner_orders_temp.cancellation IS NULL
AND customer_orders_temp.exclusions IS NOT NULL
AND customer_orders_temp.extras IS NOT NULL;
```
• **Output:**  
| pizzas_with_both |
| ---------------- |
| 1                |
  
• **Response:**
Only one delivered pizza had both exclusions and extras.
</ul>
	
---

**9️⃣ What was the total volume of pizzas ordered for each hour of the day?**  
<ul>
	
• **SQL Query Solution:**
```sql
SELECT EXTRACT(HOUR FROM order_time) AS order_hour, COUNT(pizza_id) AS pizza_volume
FROM customer_orders_temp
GROUP BY order_hour
ORDER BY order_hour;
```
• **Output:**  
| order_hour | pizza_volume |
| ---------- | ------------ |
| 11         | 1            |
| 13         | 3            |
| 18         | 3            |
| 19         | 1            |
| 21         | 3            |
| 23         | 3            |

• **Response:**
1 pizza was ordered at 11am; 3 pizzas at 1pm; 3 pizzas at 6pm; 1 pizza at 7pm; 3 pizzas at 9pm; and 3 pizzas at 11pm.
</ul>

---

**🔟 What was the volume of orders for each day of the week?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT TO_CHAR(order_time, 'Day') AS weekday, COUNT(order_id) AS order_volume
FROM customer_orders_temp
GROUP BY weekday, EXTRACT(DOW FROM order_time)
ORDER BY EXTRACT(DOW FROM order_time);
```
• **Output:**  
| weekday   | order_volume |
| --------- | ------------ |
| Wednesday | 5            |
| Thursday  | 3            |
| Friday    | 1            |
| Saturday  | 5            |
  
• **Response:**
On Wednesdays, there were 5 orders; on Thursdays, there were 3 orders; on Fridays, there was 1 order; and on Saturdays there were 5 orders.  
</ul>  

---
  
**➡️ Runner and Customer Experience ⬅️**  
  
**1️⃣ How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)**  
<ul>

• **SQL Query Solution:**
```sql
SELECT DATE_TRUNC('week', registration_date - INTERVAL '4 days') + INTERVAL '4 days' AS week_start, COUNT(runner_id) as runners_signup
FROM runners
GROUP BY week_start
ORDER BY week_start;
```
• **Output:**
| week_start          | runners_signup |
| ------------------- | -------------- |
| 2021-01-01 00:00:00 | 2              |
| 2021-01-08 00:00:00 | 1              |
| 2021-01-15 00:00:00 | 1              |

• **Response:**
On week 1, starting 2021-01-01, 2 runners signed up; on week 2, 1 runner signed up; on week 3, 1 runner signed up.
</ul>

---

**2️⃣ What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT runner_orders_temp.runner_id, AVG(EXTRACT(EPOCH FROM (runner_orders_temp.pickup_time - customer_orders_temp.order_time)) / 60) AS avg_time_to_pickup_mins
FROM customer_orders_temp
JOIN runner_orders_temp ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.cancellation IS NULL
GROUP BY runner_orders_temp.runner_id;
```
• **Output:**  
| runner_id | avg_time_to_pickup_mins |
| --------- | ----------------------- |
| 1         | 15.677777777777777      |
| 2         | 23.720000000000002      |
| 3         | 10.466666666666667      |
  
• **Response:**
Runner 1 takes an average of 15.7 minutes for pick up; runner 2 has an average of 23.7 minutes until pick up; and runner 3 has an average of 10.5 minutes until pick up.
</ul>

---

**3️⃣ Is there any relationship between the number of pizzas and how long the order takes to prepare?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  num_pizzas_in_order, 
  ROUND(AVG(time_to_pickup_mins)::NUMERIC, 2) AS avg_pickup_time
FROM (
  SELECT 
    customer_orders_temp.order_id, 
    COUNT(customer_orders_temp.pizza_id) AS num_pizzas_in_order,
    EXTRACT(EPOCH FROM (runner_orders_temp.pickup_time - customer_orders_temp.order_time)) / 60 AS time_to_pickup_mins
  FROM customer_orders_temp
  JOIN runner_orders_temp 
    ON customer_orders_temp.order_id = runner_orders_temp.order_id
  WHERE runner_orders_temp.cancellation IS NULL
  GROUP BY 
    customer_orders_temp.order_id, 
    runner_orders_temp.pickup_time, 
    customer_orders_temp.order_time
) sub
GROUP BY num_pizzas_in_order
ORDER BY num_pizzas_in_order;
```
• **Output:**  
| num_pizzas_in_order | avg_pickup_time |
| ------------------- | --------------- |
| 1                   | 12.36           |
| 2                   | 18.38           |
| 3                   | 29.28           |
  
• **Response:**
Yes, there is a positive relationship between the number of pizzas in an order and how long it takes to prepare.
</ul>

---

**4️⃣ What was the average distance travelled for each customer?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT customer_orders_temp.customer_id, AVG(runner_orders_temp.distance) AS avg_distance_kms
FROM customer_orders_temp
JOIN runner_orders_temp ON runner_orders_temp.order_id = customer_orders_temp.order_id
GROUP BY customer_orders_temp.customer_id
ORDER BY customer_orders_temp.customer_id;
```
• **Output:**
| customer_id | avg_distance_kms   |
| ----------- | ------------------ |
| 101         | 20                 |
| 102         | 16.733333333333334 |
| 103         | 23.399999999999995 |
| 104         | 10                 |
| 105         | 25                 |
  
• **Response:**  
The average distance travelled for customer 101 is 20km; for 102 it's 16.7km; for 103 it's 23.4km; for 104 it's 10km; and for 105 it's 25km.
</ul>

---

**5️⃣ What was the difference between the longest and shortest delivery times for all orders?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT (MAX(duration)-MIN(duration)) AS delivery_maxmin_difference
FROM runner_orders_temp
WHERE cancellation IS NULL;
```
• **Output:**  
| delivery_maxmin_difference |
| -------------------------- |
| 30                         |
  
• **Response:**
The difference between the longest and shortest delivery times for all orders is 30 minutes.
</ul>

---

**6️⃣ What was the average speed for each runner for each delivery and do you notice any trend for these values?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  runner_id, 
  order_id, 
  ROUND(((distance / duration::NUMERIC) * 60)::NUMERIC, 2) AS avg_speed_kmh
FROM runner_orders_temp
WHERE cancellation IS NULL
GROUP BY runner_id, order_id, avg_speed_kmh;
```
• **Output:**  
| runner_id | order_id | avg_speed_kmh |
| --------- | -------- | ------------- |
| 1         | 1        | 37.50         |
| 1         | 2        | 44.44         |
| 1         | 3        | 40.20         |
| 1         | 10       | 60.00         |
| 2         | 4        | 35.10         |
| 2         | 7        | 60.00         |
| 2         | 8        | 93.60         |
| 3         | 5        | 40.00         |
  
• **Response:**
For runner 1, average speed ranges from 37.5 km/h to 50km/h; for runner 2, average speed ranges from 35.1km/h to 93.6km/h; and runner 3 has an average speed of 40km/h. Runner 2 data should be further investigated as the speed ranges widely and could be a sign of data irregularity.
</ul>

---

**7️⃣ What is the successful delivery percentage for each runner?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  runner_id,
  ROUND(COUNT(CASE WHEN cancellation IS NULL THEN 1 END)::NUMERIC * 100 / COUNT(*)) AS successful_delivery_percentage
FROM runner_orders_temp
GROUP BY runner_id
ORDER BY runner_id;
```
• **Output:**  
| runner_id | successful_delivery_percentage |
| --------- | ------------------------------ |
| 1         | 100                            |
| 2         | 75                             |
| 3         | 50                             |
  
• **Response:**
Runner 1 has 100% successful delivery percentage; runner 2 has 75% success; and runner 3 has 50%.
</ul>

---

**➡️ Ingredient Optimization ⬅️**  
  
**1️⃣ What are the standard ingredients for each pizza?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  pn.pizza_name,
  pt.topping_name
FROM pizza_recipes pr
JOIN pizza_names pn 
  ON pr.pizza_id = pn.pizza_id
JOIN unnest(string_to_array(pr.toppings, ',')) AS topping_id_str(topping_id) 
  ON TRUE
JOIN pizza_toppings pt 
  ON pt.topping_id = topping_id_str.topping_id::INT
ORDER BY pn.pizza_name, pt.topping_id;
```
• **Output:**
| pizza_name | topping_name |
| ---------- | ------------ |
| Meatlovers | Bacon        |
| Meatlovers | BBQ Sauce    |
| Meatlovers | Beef         |
| Meatlovers | Cheese       |
| Meatlovers | Chicken      |
| Meatlovers | Mushrooms    |
| Meatlovers | Pepperoni    |
| Meatlovers | Salami       |
| Vegetarian | Cheese       |
| Vegetarian | Mushrooms    |
| Vegetarian | Onions       |
| Vegetarian | Peppers      |
| Vegetarian | Tomatoes     |
| Vegetarian | Tomato Sauce |

• **Response:**
For meatlovers pizza the toppings are: bacon, BBQ sauce, beef, cheese, chicken, mushrooms, pepperoni, and salami. For vegetarian pizza, ingredients are cheese, mushrooms, onions, peppers, tomatoes, and tomato sauce.
</ul>

---

**2️⃣ What was the most commonly added extra?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  pt.topping_name,
  COUNT(*) AS extra_count
FROM customer_orders_temp cot
JOIN LATERAL unnest(string_to_array(cot.extras, ',')) AS extras_str(extras) ON TRUE
JOIN pizza_toppings pt ON pt.topping_id = extras_str.extras::INT
WHERE cot.extras IS NOT NULL
GROUP BY pt.topping_name
ORDER BY extra_count DESC
LIMIT 1;
```
• **Output:**  
| topping_name | extra_count |
| ------------ | ----------- |
| Bacon        | 4           |
  
• **Response:**
The most commonly added extra was bacon.
</ul>

---

**3️⃣ What was the most common exclusion?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  pt.topping_name,
  COUNT(*) AS exclusion_count
FROM customer_orders_temp cot
JOIN LATERAL unnest(string_to_array(cot.exclusions, ',')) AS exclusions_str(exclusions) ON TRUE
JOIN pizza_toppings pt ON pt.topping_id = exclusions_str.exclusions::INT
WHERE cot.exclusions IS NOT NULL
GROUP BY pt.topping_name
ORDER BY exclusion_count DESC
LIMIT 1;
```
• **Output:**  
| topping_name | exclusion_count |
| ------------ | --------------- |
| Cheese       | 4               |
  
• **Response:**
The most common exclusion is cheese.
</ul>

---

**4️⃣ Generate an order item for each record in the customers_orders table in the format of one of the following:  
• Meat Lovers  
• Meat Lovers - Exclude Beef  
• Meat Lovers - Extra Bacon  
• Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  cto.order_id,
  pn.pizza_name ||
    COALESCE(' - Exclude ' || excl.exclusions, '') ||
    COALESCE(' - Extra ' || extr.extras, '') AS order_description
FROM customer_orders_temp cto
JOIN pizza_names pn ON cto.pizza_id = pn.pizza_id

LEFT JOIN (
  SELECT order_id, STRING_AGG(DISTINCT pt.topping_name, ', ') AS exclusions
  FROM customer_orders_temp
  JOIN LATERAL unnest(string_to_array(exclusions, ',')) AS e(eid) ON TRUE
  JOIN pizza_toppings pt ON pt.topping_id = e.eid::INT
  WHERE exclusions IS NOT NULL
  GROUP BY order_id
) excl ON excl.order_id = cto.order_id

LEFT JOIN (
  SELECT order_id, STRING_AGG(DISTINCT pt.topping_name, ', ') AS extras
  FROM customer_orders_temp
  JOIN LATERAL unnest(string_to_array(extras, ',')) AS e(eid) ON TRUE
  JOIN pizza_toppings pt ON pt.topping_id = e.eid::INT
  WHERE extras IS NOT NULL
  GROUP BY order_id
) extr ON extr.order_id = cto.order_id

ORDER BY order_id;
```
• **Output:**
| order_id | order_description                                               |
| -------- | --------------------------------------------------------------- |
| 1        | Meatlovers                                                      |
| 2        | Meatlovers                                                      |
| 3        | Vegetarian                                                      |
| 3        | Meatlovers                                                      |
| 4        | Meatlovers - Exclude Cheese                                     |
| 4        | Meatlovers - Exclude Cheese                                     |
| 4        | Vegetarian - Exclude Cheese                                     |
| 5        | Meatlovers - Extra Bacon                                        |
| 6        | Vegetarian                                                      |
| 7        | Vegetarian - Extra Bacon                                        |
| 8        | Meatlovers                                                      |
| 9        | Meatlovers - Exclude Cheese - Extra Bacon, Chicken              |
| 10       | Meatlovers - Exclude BBQ Sauce, Mushrooms - Extra Bacon, Cheese |
| 10       | Meatlovers - Exclude BBQ Sauce, Mushrooms - Extra Bacon, Cheese |
  
• **Response:**  
The above output represents a table where, for each order id, a description is displayed with the pizza type name plus any exclusions or extras for each.
</ul>

---

**5️⃣ Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients. For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"?**  
<ul>

• **SQL Query Solution:**
```sql
WITH numbered_orders AS (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY order_id, pizza_id ORDER BY order_time) AS pizza_instance
  FROM customer_orders_temp
)

SELECT
  no.order_id,
  pn.pizza_name || ': ' ||
    STRING_AGG(
      CASE
        WHEN topping_count = 2 THEN '2x' || pt.topping_name
        ELSE pt.topping_name
      END,
      ', ' ORDER BY pt.topping_name
    ) AS pizza_ingredients
FROM numbered_orders no
JOIN pizza_names pn ON no.pizza_id = pn.pizza_id
JOIN pizza_recipes pr ON no.pizza_id = pr.pizza_id

JOIN LATERAL (
  SELECT topping_id, COUNT(*) AS topping_count
  FROM (
    SELECT TRIM(tid) AS topping_id
    FROM UNNEST(string_to_array(pr.toppings, ',')) AS tid
    WHERE TRIM(tid) NOT IN (
      SELECT TRIM(ex_id)
      FROM UNNEST(string_to_array(COALESCE(no.exclusions, ''), ',')) AS ex_id
      WHERE TRIM(ex_id) <> ''
    )

    UNION ALL

    SELECT TRIM(extra_id) AS topping_id
    FROM UNNEST(string_to_array(COALESCE(no.extras, ''), ',')) AS extra_id
    WHERE TRIM(extra_id) <> ''
  ) all_toppings
  GROUP BY topping_id
) topping_data ON TRUE

JOIN pizza_toppings pt ON pt.topping_id = topping_data.topping_id::INT

GROUP BY no.order_id, no.pizza_id, no.pizza_instance, pn.pizza_name
ORDER BY no.order_id, no.pizza_instance;
```
• **Output:**  
| order_id | pizza_ingredients                                                                   |
| -------- | ----------------------------------------------------------------------------------- |
| 1        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami   |
| 2        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami   |
| 3        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami   |
| 3        | Vegetarian: Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes              |
| 4        | Meatlovers: BBQ Sauce, Bacon, Beef, Chicken, Mushrooms, Pepperoni, Salami           |
| 4        | Vegetarian: Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes                      |
| 4        | Meatlovers: BBQ Sauce, Bacon, Beef, Chicken, Mushrooms, Pepperoni, Salami           |
| 5        | Meatlovers: BBQ Sauce, 2xBacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami |
| 6        | Vegetarian: Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes              |
| 7        | Vegetarian: Bacon, Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes       |
| 8        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami   |
| 9        | Meatlovers: BBQ Sauce, 2xBacon, Beef, 2xChicken, Mushrooms, Pepperoni, Salami       |
| 10       | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami   |
| 10       | Meatlovers: 2xBacon, Beef, 2xCheese, Chicken, Pepperoni, Salami                     |

  
• **Response:**
The requested formatted output was generated.
</ul>

---

**6️⃣ What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?**  
<ul>

• **SQL Query Solution:**
```sql
WITH delivered_pizzas AS (
  SELECT cto.*
  FROM customer_orders_temp cto
  JOIN runner_orders_temp rot ON cto.order_id = rot.order_id
  WHERE rot.cancellation IS NULL
),
all_toppings AS (
  SELECT 
    dp.order_id,
    TRIM(tid) AS topping_id
  FROM delivered_pizzas dp
  JOIN pizza_recipes pr ON dp.pizza_id = pr.pizza_id
  JOIN LATERAL unnest(string_to_array(pr.toppings, ',')) AS tid ON TRUE
  WHERE TRIM(tid) NOT IN (
    SELECT TRIM(eid)
    FROM unnest(string_to_array(COALESCE(dp.exclusions, ''), ',')) AS eid
    WHERE TRIM(eid) <> ''
  )
  UNION ALL
  SELECT 
    dp.order_id,
    TRIM(eid) AS topping_id
  FROM delivered_pizzas dp
  JOIN LATERAL unnest(string_to_array(COALESCE(dp.extras, ''), ',')) AS eid ON TRUE
  WHERE TRIM(eid) <> ''
)

SELECT 
  pt.topping_name,
  COUNT(*) AS ingredient_qty
FROM all_toppings at
JOIN pizza_toppings pt ON pt.topping_id = at.topping_id::INT
GROUP BY pt.topping_name
ORDER BY ingredient_qty DESC;
```
• **Output:**  
| topping_name | ingredient_qty |
| ------------ | -------------- |
| Bacon        | 12             |
| Mushrooms    | 11             |
| Cheese       | 10             |
| Pepperoni    | 9              |
| Chicken      | 9              |
| Salami       | 9              |
| Beef         | 9              |
| BBQ Sauce    | 8              |
| Tomato Sauce | 3              |
| Onions       | 3              |
| Tomatoes     | 3              |
| Peppers      | 3              |
  
• **Response:**
The output table represents the requested results, with bacon being the most frequent ingredient in delivered pizzas.
</ul>

---

**➡️ Pricing and Ratings ⬅️**  
  
**1️⃣ If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  SUM(
    CASE 
      WHEN cot.pizza_id = 1 THEN 12
      WHEN cot.pizza_id = 2 THEN 10
      ELSE 0
    END
  ) AS total_revenue
FROM customer_orders_temp cot
JOIN runner_orders_temp rot ON cot.order_id = rot.order_id
WHERE rot.cancellation IS NULL;
```
• **Output:**
| total_revenue |
| ------------- |
| 138           |

• **Response:**
Pizza Runner made $138.
</ul>

---

**2️⃣ What if there was an additional $1 charge for any pizza extras?  
• Add cheese is $1 extra**  
<ul>

• **SQL Query Solution:**
```sql
SELECT 
  SUM(
    CASE 
      WHEN cot.pizza_id = 1 THEN 12
      WHEN cot.pizza_id = 2 THEN 10
      ELSE 0
    END
    +
    COALESCE(array_length(string_to_array(REPLACE(cot.extras, ' ', ''), ','), 1), 0)
  ) AS total_revenue
FROM customer_orders_temp cot
JOIN runner_orders_temp rot ON cot.order_id = rot.order_id
WHERE rot.cancellation IS NULL;
```
• **Output:**  
| total_revenue |
| ------------- |
| 142           |
  
• **Response:**
With additional $1 charges for any extras, total revenue is $142.
</ul>

---

**3️⃣ The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**4️⃣ Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?  
• customer_id  
• order_id  
• runner_id  
• rating  
• order_time  
• pickup_time  
• Time between order and pickup  
• Delivery duration  
• Average speed  
• Total number of pizzas?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**

  
• **Response:**  
A.
</ul>

---

**5️⃣ If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries??**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**  

  
• **Response:**
A.
</ul>


  ---
### Business Recommendations
Based on the conducted queries in response to the requests, there are a few observations that can be done from resulting outputs. For example, Danny's Diner can optimize operations by looking at customer spending patterns and tailoring its rewards program accordingly. Ramen stands out as the most popular item both by purchase frequency as well as preference among customers, making it a key product to promote and enhance brand image. Additionally, it appears that the rewards loyalty program provided by the Diner has the potential to influence customer behavior, especially during the first week of membership when purchases are incentivized by providing double points. This period could be leveraged to further drive customer engagement and encourage higher spending.  
  
To maximize the Diner’s profitability, some policies could be brainstormed for future implementation based on these insights. For example, offering bundle promotions on popular combinations like ramen paired with sushi or curry could encourage larger orders while catering to existing preferences. Also, targeted promotions for new members during their first week — such as offering extra points on purchases above a certain threshold — could drive higher initial spending and loyalty. These are just a few simple ideas gathered from the small dataset and resulting outputs that would naturally be expanded and better discussed were the dataset larger and more detailed over time.

  ---


</ul>

 
