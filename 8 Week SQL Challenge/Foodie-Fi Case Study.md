# 🥑 Case Study: [Foodie-Fi](https://8weeksqlchallenge.com/case-study-3/)


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
    <img width="70%" src="https://github.com/user-attachments/assets/fd9721f2-b48c-4d42-a963-059421205fd3">
</p>

  ---
  
### Dataset Schema SQL Code

**Schema (PostgreSQL v13)**
<ul> 
  
 ```sql
CREATE SCHEMA foodie_fi;
SET search_path = foodie_fi;

CREATE TABLE plans (
  plan_id INTEGER,
  plan_name VARCHAR(13),
  price DECIMAL(5,2)
);

INSERT INTO plans
  (plan_id, plan_name, price)
VALUES
  ('0', 'trial', '0'),
  ('1', 'basic monthly', '9.90'),
  ('2', 'pro monthly', '19.90'),
  ('3', 'pro annual', '199'),
  ('4', 'churn', null);



CREATE TABLE subscriptions (
  customer_id INTEGER,
  plan_id INTEGER,
  start_date DATE
);

INSERT INTO subscriptions
  (customer_id, plan_id, start_date)
VALUES
  ('1', '0', '2020-08-01'),
  ('1', '1', '2020-08-08'),
  ('2', '0', '2020-09-20'),
  ('2', '3', '2020-09-27'),
  ('3', '0', '2020-01-13'),
  ('3', '1', '2020-01-20'),
  ('4', '0', '2020-01-17'),
  ('4', '1', '2020-01-24'),
  ('4', '4', '2020-04-21'),
  ('5', '0', '2020-08-03'),
  ('5', '1', '2020-08-10'),
  ('6', '0', '2020-12-23'),
  ('6', '1', '2020-12-30'),
  ('6', '4', '2021-02-26'),
  ('7', '0', '2020-02-05'),
  ('7', '1', '2020-02-12'),
  ('7', '2', '2020-05-22'),
  ('8', '0', '2020-06-11'),
  ('8', '1', '2020-06-18'),
  ('8', '2', '2020-08-03'),
  ('9', '0', '2020-12-07'),
  ('9', '3', '2020-12-14'),
  ('10', '0', '2020-09-19'),
  ('10', '2', '2020-09-26'),
  ('11', '0', '2020-11-19'),
  ('11', '4', '2020-11-26'),
  ('12', '0', '2020-09-22'),
  ('12', '1', '2020-09-29'),
  ('13', '0', '2020-12-15'),
  ('13', '1', '2020-12-22'),
  ('13', '2', '2021-03-29'),
  ('14', '0', '2020-09-22'),
  ('14', '1', '2020-09-29'),
  ('15', '0', '2020-03-17'),
  ('15', '2', '2020-03-24'),
  ('15', '4', '2020-04-29'),
  ('16', '0', '2020-05-31'),
  ('16', '1', '2020-06-07'),
  ('16', '3', '2020-10-21'),
  ('17', '0', '2020-07-27'),
  ('17', '1', '2020-08-03'),
  ('17', '3', '2020-12-11'),
  ('18', '0', '2020-07-06'),
  ('18', '2', '2020-07-13'),
  ('19', '0', '2020-06-22'),
  ('19', '2', '2020-06-29'),
  ('19', '3', '2020-08-29'),
  ('20', '0', '2020-04-08'),
  ('20', '1', '2020-04-15'),
  ('20', '3', '2020-06-05'),...
```
  [Full Schema SQL Code found here.](https://www.db-fiddle.com/f/rHJhRrXy5hbVBNJ6F6b9gJ/16)
</ul>

---

### Questions & Solutions

For each question, I have included a SQL Query Solution, its output, and a brief response to the information requested.    
  
**➡️ Customer Journey ⬅️**  

**Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.**
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---
  
**➡️ Data Analysis Questions ⬅️**  
  
**1️⃣ How many customers has Foodie-Fi ever had?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**2️⃣ What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**3️⃣ What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**4️⃣ What is the customer count and percentage of customers who have churned rounded to 1 decimal place?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**5️⃣ How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**6️⃣ What is the number and percentage of customer plans after their initial free trial?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**7️⃣ What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**8️⃣ How many customers have upgraded to an annual plan in 2020?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**9️⃣ How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**🔟 Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**1️⃣1️⃣ How many customers downgraded from a pro monthly to a basic monthly plan in 2020?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**➡️ Challenge Payment Question ⬅️**  
**The Foodie-Fi team wants you to create a new payments table for the year 2020 that includes amounts paid by each customer in the subscriptions table with the following requirements:  
  • Monthly payments always occur on the same day of month as the original start_date of any monthly paid plan;  
  • Upgrades from basic to monthly or pro plans are reduced by the current paid amount in that month and start immediately;  
  • Upgrades from pro monthly to pro annual are paid at the end of the current billing period and also starts at the end of the month period;  
  • Once a customer churns they will no longer make payments.**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**➡️ Outside The Box Questions ⬅️**  

**1️⃣ How would you calculate the rate of growth for Foodie-Fi?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**2️⃣ What key metrics would you recommend Foodie-Fi management to track over time to assess performance of their overall business?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**3️⃣ What are some key customer journeys or experiences that you would analyse further to improve customer retention?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**4️⃣ If the Foodie-Fi team were to create an exit survey shown to customers who wish to cancel their subscription, what questions would you include in the survey?**  
<ul>

• **SQL Query Solution:**
```sql

```
• **Output:**


• **Response:**
A.
</ul>

---

**5️⃣ What business levers could the Foodie-Fi team use to reduce the customer churn rate? How would you validate the effectiveness of your ideas?**  
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

 
