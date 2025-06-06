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
    WHEN exclusions IS NULL OR exclusions ILIKE 'null' OR TRIM(exclusions) = '' THEN NULL
    ELSE exclusions
  END AS exclusions,

  CASE
    WHEN extras IS NULL OR extras ILIKE 'null' OR TRIM(extras) = '' THEN NULL
    ELSE extras
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
    ELSE pickup_time
  END AS pickup_time,

  CASE
    WHEN distance ILIKE 'null' OR TRIM(distance) = '' THEN NULL
    WHEN distance LIKE '%km%' THEN TRIM(REPLACE(distance, 'km', ''))
    ELSE distance
  END AS distance,

  CASE
    WHEN duration ILIKE 'null' OR TRIM(duration) = '' THEN NULL
    WHEN duration LIKE '%minutes%' THEN TRIM(REPLACE(duration, 'minutes', ''))
    WHEN duration LIKE '%minute' THEN TRIM(REPLACE(duration, 'minute', ''))
    WHEN duration LIKE '%mins' THEN TRIM(REPLACE(duration, 'mins', ''))
    ELSE duration
  END AS duration,

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
SELECT runner_orders_temp.runner_id, AVG(EXTRACT(EPOCH FROM (CAST(runner_orders_temp.pickup_time AS TIMESTAMP) - customer_orders_temp.order_time)) / 60) AS avg_time_to_pickup_mins
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
SELECT num_pizzas_in_order, ROUND(AVG(time_to_pickup_mins)::NUMERIC, 2) AS avg_pickup_time
FROM
	(SELECT customer_orders_temp.order_id, COUNT(customer_orders_temp.pizza_id) AS num_pizzas_in_order,EXTRACT(EPOCH FROM (CAST(runner_orders_temp.pickup_time AS TIMESTAMP) - customer_orders_temp.order_time)) / 60 AS time_to_pickup_mins
	FROM customer_orders_temp
	JOIN runner_orders_temp ON customer_orders_temp.order_id = runner_orders_temp.order_id
	WHERE runner_orders_temp.cancellation IS NULL
	GROUP BY customer_orders_temp.order_id, runner_orders_temp.pickup_time, customer_orders_temp.order_time
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
    SELECT
    	menu.product_name AS most_purchased_product,
    	COUNT (sales.order_date) AS total_purchases
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	menu.product_name
    ORDER BY
    	total_purchases DESC
    LIMIT 1;
```
• **Output:**

  
• **Response:**  
A.
</ul>

---

**5️⃣ What was the difference between the longest and shortest delivery times for all orders?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT DISTINCT ON (sales.customer_id)
    	sales.customer_id,
    	menu.product_name AS customer_favorite
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	sales.customer_id, menu.product_name
    ORDER BY
    	sales.customer_id, COUNT(sales.order_date) DESC;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**6️⃣ What was the average speed for each runner for each delivery and do you notice any trend for these values?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
    	menu.product_name
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    WHERE
    	sales.order_date = (
    		SELECT 
    			s.order_date
            	FROM 
                		sales s
            	JOIN 
                		members m
            	ON 
                		s.customer_id = m.customer_id
            	WHERE 
                		s.customer_id = sales.customer_id  -- Outer query's "sales" vs. subquery's "s"
                		AND s.order_date > m.join_date
            	ORDER BY 
               		 s.order_date ASC
            	LIMIT 1
        )
    ORDER BY
	sales.customer_id;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**7️⃣ What is the successful delivery percentage for each runner?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
    	menu.product_name
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    WHERE
    	sales.order_date = (
    		SELECT 
    			MAX(s.order_date)
            	FROM 
                		sales s
            	JOIN 
                		members m
            	ON 
                		s.customer_id = m.customer_id
            	WHERE 
                		s.customer_id = sales.customer_id
                		AND s.order_date < m.join_date
        )
    ORDER BY
    	customer_id;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**➡️ Ingredient Optimization ⬅️**  
  
**1️⃣ What are the standard ingredients for each pizza?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
	SUM(menu.price) AS total_sales
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	sales.customer_id
    ORDER BY
    	sales.customer_id ASC;
```
• **Output:**


• **Response:**
A.
</ul>

---

**2️⃣ What was the most commonly added extra?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	customer_id, COUNT (DISTINCT order_date) as total_visits
    FROM
    	sales
    GROUP BY
    	customer_id
    ORDER BY
    	customer_id ASC;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**3️⃣ What was the most common exclusion?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
        sales.customer_id, 
        menu.product_name AS first_purchase
    FROM
        sales
    JOIN
        menu
    ON
        sales.product_id = menu.product_id
    WHERE
        sales.order_date = (
            SELECT
                MIN(order_date)
            FROM
                sales s
            WHERE
                s.customer_id = sales.customer_id
        )
    GROUP BY
        sales.customer_id, menu.product_name
    ORDER BY
        sales.customer_id;
```
• **Output:**  

  
• **Response:**
A.
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
    	menu.product_name AS most_purchased_product,
    	COUNT (sales.order_date) AS total_purchases
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	menu.product_name
    ORDER BY
    	total_purchases DESC
    LIMIT 1;
```
• **Output:**

  
• **Response:**  
A.
</ul>

---

**5️⃣ Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients. For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT DISTINCT ON (sales.customer_id)
    	sales.customer_id,
    	menu.product_name AS customer_favorite
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	sales.customer_id, menu.product_name
    ORDER BY
    	sales.customer_id, COUNT(sales.order_date) DESC;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**6️⃣ What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
    	menu.product_name
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    WHERE
    	sales.order_date = (
    		SELECT 
    			s.order_date
            	FROM 
                		sales s
            	JOIN 
                		members m
            	ON 
                		s.customer_id = m.customer_id
            	WHERE 
                		s.customer_id = sales.customer_id  -- Outer query's "sales" vs. subquery's "s"
                		AND s.order_date > m.join_date
            	ORDER BY 
               		 s.order_date ASC
            	LIMIT 1
        )
    ORDER BY
	sales.customer_id;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**➡️ Pricing and Ratings ⬅️**  
  
**1️⃣ If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
	SUM(menu.price) AS total_sales
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	sales.customer_id
    ORDER BY
    	sales.customer_id ASC;
```
• **Output:**


• **Response:**
A.
</ul>

---

**2️⃣ What if there was an additional $1 charge for any pizza extras?  
• Add cheese is $1 extra**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
    	customer_id, COUNT (DISTINCT order_date) as total_visits
    FROM
    	sales
    GROUP BY
    	customer_id
    ORDER BY
    	customer_id ASC;
```
• **Output:**  

  
• **Response:**
A.
</ul>

---

**3️⃣ The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT
        sales.customer_id, 
        menu.product_name AS first_purchase
    FROM
        sales
    JOIN
        menu
    ON
        sales.product_id = menu.product_id
    WHERE
        sales.order_date = (
            SELECT
                MIN(order_date)
            FROM
                sales s
            WHERE
                s.customer_id = sales.customer_id
        )
    GROUP BY
        sales.customer_id, menu.product_name
    ORDER BY
        sales.customer_id;
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
    SELECT
    	menu.product_name AS most_purchased_product,
    	COUNT (sales.order_date) AS total_purchases
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	menu.product_name
    ORDER BY
    	total_purchases DESC
    LIMIT 1;
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
    SELECT DISTINCT ON (sales.customer_id)
    	sales.customer_id,
    	menu.product_name AS customer_favorite
    FROM
    	sales
    JOIN
    	menu
    ON
    	sales.product_id = menu.product_id
    GROUP BY
    	sales.customer_id, menu.product_name
    ORDER BY
    	sales.customer_id, COUNT(sales.order_date) DESC;
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

 
