# 🥢 Case Study: [Danny's Diner](https://8weeksqlchallenge.com/case-study-1/)


### 🗂️ Table of Contents  
• <i> [Business Use Case](#Business_Use_Case) </i>  
• <i> [ERD](#ERD) </i>  
• <i> [Dataset Schema SQL Code](#Dataset_Schema_SQL_Code) </i>  
• <i> [Questions & Solutions](#Questions_&_Solutions) </i>   
• <i> [Business Recommendations](#Business_Recommendations) </i> 
  
  ---
### Business Use Case
Danny’s Diner is a small restaurant serving sushi, curry, and ramen. Danny wants to use customer data to understand their buying patterns, how much they spend, and which menu items are most popular. This will help improve the customer experience and decide whether to expand the loyalty program. Danny has provided sample data on sales, menu, and members and needs SQL queries to analyze this data and create simple datasets for his team to review.

  ---
### ERD
<p align="center" width="100%">
    <img width="70%" src="https://github.com/user-attachments/assets/bd3d31b1-965b-479e-bf8c-b322643e951f">
</p>

  ---
  
### Dataset Schema SQL Code

**Schema (PostgreSQL v13)**
<ul>
	
 ```sql
CREATE SCHEMA dannys_diner;
SET search_path = dannys_diner;

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
```
</ul>

---

### Questions & Solutions

For each question, I have included a SQL Query Solution, its output, and a brief response to the information requested.    

**1️⃣ What is the total amount each customer spent at the restaurant?**  
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
| customer_id | total_sales |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

• **Response:**
The total amount spent by customer A is $76, customer B is $74 and customer C is $36.
</ul>

---

**2️⃣ How many days has each customer visited the restaurant?**  
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
| customer_id | total_visits |
| ----------- | ------------ |
| A           | 4            |
| B           | 6            |
| C           | 2            |
  
• **Response:**
Customer A has visited the restaurant 4 times, customer B has visited 6 times, and customer C has visited 2 times.
</ul>

---

**3️⃣ What was the first item from the menu purchased by each customer?**  
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
| customer_id | first_purchase |
| ----------- | -------------- |
| A           | curry          |
| A           | sushi          |
| B           | curry          |
| C           | ramen          |
  
• **Response:**
Customer A purchased 2 items for their first visit, curry and sushi, while customer B purchased curry and customer C purchased ramen for their first visits.
</ul>

---

**4️⃣ What is the most purchased item on the menu and how many times was it purchased by all customers?**  
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
| most_purchased_product | total_purchases |
| ---------------------- | --------------- |
| ramen                  | 8               |
  
• **Response:**  
The most popular item by purchase amount is ramen, being purchased 8 times by all customers combined.
</ul>

---

**5️⃣ Which item was the most popular for each customer?**  
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
| customer_id | customer_favorite |
| ----------- | ----------------- |
| A           | ramen             |
| B           | ramen             |
| C           | ramen             |
  
• **Response:**
The most popular item for every distinct customer (A, B, and C) is ramen.
</ul>

---

**6️⃣ Which item was purchased first by the customer after they became a member?**  
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
| customer_id | product_name |
| ----------- | ------------ |
| A           | ramen        |
| B           | sushi        |
  
• **Response:**
After they became members, customer A's first purchase was ramen, while customer B's first purchase was sushi. Customer C is not yet a member.
</ul>

---

**7️⃣ Which item was purchased just before the customer became a member?**  
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
| customer_id | product_name |
| ----------- | ------------ |
| A           | sushi        |
| A           | curry        |
| B           | sushi        |
  
• **Response:**
Just before becoming members, customer A purchased sushi and curry on the same visit, and customer B purchased sushi.
</ul>

---

**8️⃣ What is the total items and amount spent for each member before they became a member?**  
<ul>
	
• **SQL Query Solution:**
```sql
    SELECT
    	sales.customer_id,
        	COUNT(sales.product_id) AS total_items,
        	SUM(menu.price) AS total_amount_spent
    FROM 
    	sales
    JOIN 
    	menu
    ON 
    	sales.product_id = menu.product_id
    JOIN
    	members
    ON 
    	sales.customer_id = members.customer_id
    WHERE 
    	sales.order_date < members.join_date   
    GROUP BY 
    	sales.customer_id
    ORDER BY 
    	sales.customer_id;
```
• **Output:**  
| customer_id | total_items | total_amount_spent |
| ----------- | ----------- | ------------------ |
| A           | 2           | 25                 |
| B           | 3           | 40                 |
  
• **Response:**
Before becoming members, customer A had purchased 2 items for a total amount spent of $25, while customer B had purchased 3 items for a total amount spent of $40.
</ul>
	
---

**9️⃣ If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**  
<ul>
	
• **SQL Query Solution:**
```sql
    SELECT 
        sales.customer_id,
        SUM(
            CASE 
                WHEN menu.product_name = 'sushi' THEN menu.price * 10 * 2
                ELSE menu.price * 10
            END
        ) AS total_points
    FROM 
        sales
    JOIN 
        menu
    ON 
        sales.product_id = menu.product_id
    GROUP BY 
        sales.customer_id
    ORDER BY 
        sales.customer_id;
```
• **Output:**  

| customer_id | total_points |
| ----------- | ------------ |
| A           | 860          |
| B           | 940          |
| C           | 360          |


• **Response:**
If each $1 spent equates to 10 points and sushi has a 2x point multiplier, customer A would have 860 points, customer B would have 940 points, and customer C would have 360 points.
</ul>

---

**🔟 In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**  
<ul>

• **SQL Query Solution:**
```sql
    SELECT 
        sales.customer_id,
        SUM(
        CASE
            WHEN menu.product_name = 'sushi' THEN menu.price * 20
            WHEN sales.order_date BETWEEN DATE(members.join_date) AND DATE(members.join_date) + INTERVAL '6 days' THEN menu.price * 20
            ELSE menu.price * 10
        END) AS jan_points
    FROM 
        sales
    JOIN 
        menu ON sales.product_id = menu.product_id
    JOIN
        members ON sales.customer_id = members.customer_id
    WHERE 
        sales.order_date <= '2021-01-31'
        AND sales.order_date >= members.join_date
    GROUP BY
    	sales.customer_id
    ORDER BY 
        sales.customer_id;
```
• **Output:**  
| customer_id | jan_points |
| ----------- | ---------- |
| A           | 1020       |
| B           | 320        |  
  
• **Response:**
If in the first week after joining the rewards program all purchases earn the 2x point multiplier, but afterward each $1 spent equates to 10 points and only sushi retains the 2x multiplier, customer A would have accumulated 1,020 points for January whereas customer B would have accumulated 320 points for the month.

  ---
### Business Recommendations
Based on the conducted queries in response to the requests, there are a few observations that can be done from resulting outputs. For example, Danny's Diner can optimize operations by looking at customer spending patterns and tailoring its rewards program accordingly. Ramen stands out as the most popular item both by purchase frequency as well as preference among customers, making it a key product to promote and enhance brand image. Additionally, it appears that the rewards loyalty program provided by the Diner has the potential to influence customer behavior, especially during the first week of membership when purchases are incentivized by providing double points. This period could be leveraged to further drive customer engagement and encourage higher spending.  
  
To maximize the Diner’s profitability, some policies could be brainstormed for future implementation based on these insights. For example, offering bundle promotions on popular combinations like ramen paired with sushi or curry could encourage larger orders while catering to existing preferences. Also, targeted promotions for new members during their first week — such as offering extra points on purchases above a certain threshold — could drive higher initial spending and loyalty. These are just a few simple ideas gathered from the small dataset and resulting outputs that would naturally be expanded and better discussed were the dataset larger and more detailed over time.

  ---


</ul>

 
