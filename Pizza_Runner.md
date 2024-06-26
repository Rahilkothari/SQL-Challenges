<h1>Case Study #2 Pizza Runner🍕</h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/8fbb6bcb-31bd-4b7e-8083-07596fdd911d">
<h1>Contents</h1>
<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#entityrelationshipdiagram">Entity Relationship Diagram</a></li>
  <li><a href="#datacleaninganddatatransformation">Data Cleaning & Data Transformation</a></li>
  <li><a href="#casestudyquestionsandsolutions">Case Study Questions & Solutions</a></li>
  
  <ul>
  <li><a href="#a.pizzametrics">A. Pizza Metrics</a></li>
  <li><a href="#b.runnerandcustomerexperience">B. Runner And Customer Experience</a></li>
  <li><a href="#c.ingredientoptimisation">C. Ingredient Optimisation</a></li>
  </ul>
  
  <li><a href="#keyinsights">Key Insights</a></li>
</ul>

<h1><a name="introduction">Introduction</a></h1>
<p>Welcome to the Pizza Runner Case Study! Follow Danny's journey as he combines the irresistible allure of "80s Retro Styling and Pizza Is The Future" to launch Pizza Runner, an innovative venture in the pizza delivery industry. With his background in data science, Danny understands the significance of data collection for business growth. Now, he seeks assistance in cleaning and analyzing the data to optimize Pizza Runner's operations and guide his runners more efficiently. Join us as we explore how data-driven decisions propel Pizza Runner towards success and elevate the pizza delivery experience to new heights.</p>

<h1><a name="entityrelationshipdiagram"></a>Entity Relationship Diagram</h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/38cb5eb7-522d-4081-adb5-0b47ac2ef0a8">

<h1><a name="datacleaninganddatatransformation"></a>Data Cleaning & Data Transformation</h1>

<ul>
<li><h5>customer_orders table Before</h5></li>
  
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/2ba5364c-d9b5-48f1-bb31-b010a28ad9b6">
<ul>
  <li>The customer_orders table consists of individual pizza orders, with each row representing a unique pizza.</li>
  <li>Key columns in the table are pizza_id, exclusions, and extras.</li>
  <li>Before utilizing the data for queries, the exclusions and extras columns require a data cleaning process to ensure accuracy and consistency.</li>
  <li>Data cleaning involves handling missing or null values in the exclusions and extras columns.</li>
  <li>The ingredient_id values in the exclusions and extras columns need to be standardized for uniformity.</li>
  <li>Inconsistencies and duplicates in the exclusions and extras data should be resolved to eliminate ambiguities.</li>
  <li>By performing thorough data cleaning, the customer_orders table will be optimized for effective analysis.</li>
  <li>The cleaned data will provide valuable insights into customer preferences, enabling better decision-making for Pizza Runner's operations.</li>
  <li>With accurate data, Pizza Runner can efficiently meet customer demands and deliver an enhanced pizza ordering experience.</li>
</ul>

```sql
DROP TABLE IF EXISTS customer_orders_tempp;
CREATE TABLE IF NOT EXISTS customer_orders_tempp AS
SELECT 
  order_id,
  customer_id,
  pizza_id,
  CASE 
    WHEN exclusions IS NULL OR exclusions LIKE 'null' THEN ''
    ELSE exclusions
  END AS exclusions,
  CASE 
    WHEN extras IS NULL OR extras LIKE 'null' THEN ''
    ELSE extras
  END AS extras,
  order_time
FROM customer_orders;

SELECT*
FROM customer_orders_temp
```
<li><h5>customer_orders table After AS customer_order_tempp</h5></li>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/2d344d6f-695f-4128-befe-09d692b7a115">
<li><h5>runner_orders table Before</h5></li>

<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/086a9085-3f8b-4f49-8ef0-101e198f91eb">
 <ul>
  <p>The data in the orders table of Pizza Runner contains valuable information regarding the assignment of orders to runners, including pickup times, distances, and durations. However, it is crucial to note that the table may have some known data issues that require careful handling during data cleaning.</p><br>

<p>Here are the key points to consider when cleaning the data in the orders table:</p>

<li>Verify Data Types: Before proceeding with data cleaning, it is essential to check the data types for each column in the schema SQL. Ensuring accurate data types will prevent potential data type mismatches and errors in subsequent queries.</li>

<li>Handle Incomplete Orders: Some orders may not be fully completed and can be canceled by either the restaurant or the customer. It is necessary to identify and properly handle these incomplete orders during the data cleaning process.</li>

<li>Address Null Values: The table may contain null values in certain columns, such as pickup_time, distance, and duration. Properly handling these null values is crucial to avoid inaccuracies in the analysis.</li>

<li>Validate Timestamps: The pickup_time column represents the timestamp when the runner arrives at Pizza Runner headquarters to pick up the pizzas. Validating and ensuring the consistency of these timestamps will be essential to maintain data integrity.</li>

<li>Clean Distance and Duration: The distance and duration fields provide information about the runner's travel to deliver the order. Cleaning these fields involves checking for any outliers or inconsistencies that may affect analysis results.</li>

<li>Address Known Data Issues: As there are known data issues in the table, special attention must be given to resolving these issues during the data cleaning process. Identifying and rectifying data discrepancies will enhance the accuracy and reliability of the dataset.</li>
</ul>

```sql
DROP TABLE IF EXISTS runner_orders_temp;

CREATE TABLE runner_orders_temp AS(
	SELECT order_id
	   , runner_id
	   , CASE 
	   	   WHEN pickup_time IS null OR pickup_time LIKE 'null' THEN null
	       ELSE pickup_time
	     END pickup_time
	   , CASE 
	   	   WHEN distance IS null OR distance LIKE 'null' THEN null
	       WHEN distance LIKE '%km' THEN TRIM('km' from distance)
	       ELSE distance
	     END distance
	   , CASE 
	   	  WHEN duration IS null OR duration LIKE 'null' THEN null
	      WHEN duration LIKE '%mins' THEN TRIM('mins' from duration)
	      WHEN duration LIKE '%minute' THEN TRIM('minute' from duration)
	      WHEN duration LIKE '%minutes' THEN TRIM('minutes' from duration)
	      ELSE duration 
	     END duration
	   , CASE 
	   	   WHEN cancellation IS null OR cancellation LIKE 'null'
		   THEN ''
	       ELSE cancellation
	     END cancellation
	FROM runner_orders
	);

ALTER TABLE runner_orders_temp
	ALTER COLUMN pickup_time TYPE timestamp without time zone
	USING pickup_time::timestamp,
	ALTER COLUMN distance TYPE NUMERIC
	USING distance::numeric,
	ALTER COLUMN duration TYPE INT
	USING duration::integer;
		
SELECT*
FROM runner_orders_temp
```


<li><h5>runner_orders table After AS runner_orders_temp</h5></li>
<img width="500" alt="Coding" src='https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/16580860-979b-450c-9cd4-c726659995a5'>
</ul>


<h1><a name="casestudyquestionsandsolutions"></a>Case Study Questions & Solutions</h1>

<h4><a name="a.pizzametrics"></a>A. Pizza Metrics🍕🍕</h4>

<ol> 
  <li><h5>How many pizzas were ordered?</h5></li>
 
```sql
Select count(pizza_id) as pizza_orders
From customer_orders


```

<h6>Answer:</h6>
<img width="150" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/7f304383-b85d-4cb9-80ac-863f641647a3">




   <li><h5>How many unique customer orders were made?</h5></li>
   
```sql
Select count(distinct order_id) as unique_orders
From customer_orders
```

   <h6>Answer:</h6>
<img width="150" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/015a6c20-99d0-4342-967f-ef3f4c12a69d">




   <li><h5>How many successful orders were delivered by each runner?</h5></li>

   ```sql
Select runner_id, Count(distance) as orders_delivered 
From runner_orders
where distance != 'null'
Group by runner_id
Order by runner_id
```

  <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/315359f2-94ff-4db7-af2f-a1043259178f">



   <li><h5>How many of each type of pizza was delivered?</h5></li>

  ```sql
SELECT pizza_name,COUNT(C.pizza_id)AS delivered_order_count
FROM customer_orders_tempp C
JOIN runner_orders_temp R ON C.order_id=R.order_id
JOIN pizza_names PN ON C.pizza_id=PN.pizza_id
WHERE cancellation=''
GROUP BY pizza_name

```

  <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/9278e4ca-3b7c-4bf4-807a-3ea93e21bd90">



   <li><h5>How many Vegetarian and Meatlovers were ordered by each customer?</h5></li>

   ```sql
Select c.customer_id,p.pizza_name, count(p.pizza_name) as count_ord
From customer_orders c
Join pizza_names p
On c.pizza_id = p.pizza_id

Group by c.customer_id,p.pizza_name
Order by c.customer_id
```  

   <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/83617d3c-0120-44e9-a1a0-144c96e37ec2">



   <li><h5>What was the maximum number of pizzas delivered in a single order?</h5></li>

   ```sql
with cte as
(Select c.customer_id, count(c.pizza_id) as count_ord
 From customer_orders c
 Join runner_orders r
 On c.order_id = r.order_id
 where r.distance != 'null'
 Group by c.customer_id)
 
Select max(count_ord) as max_order_delivered
From cte

```

   <h6>Answer:</h6>
<img width="150" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/aded37c9-9df5-4da3-9708-b5061cbb16c4">



   <li><h5>For each customer, how many delivered pizzas had at least 1 change and how many had no changes?</h5></li>

   ```sql
with cte as
(Select c.customer_id,
sum(case when c.exclusions != '' or c.extras != '' then 1 else 0 end) as atleast_1_change,  
sum(case when c.exclusions='' and c.extras='' then 1 else 0 end) as no_change
from customer_orders c
Join runner_orders r
On c.order_id = r.order_id
 
where r.distance != 'null'
group by c.customer_id)
 
Select sum(atleast_1_change) as has_atleast_1_changes,
       sum(no_change) as no_changes
From cte

```

   <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/02068c6b-aff7-4a87-997a-ff02f56d3cf3">



   <li><h5>How many pizzas were delivered that had both exclusions and extras?</h5></li>

   ```sql
Select sum(case when c.exclusions != 'null' and c.extras != 'null' then 1 else 0 end) as pizza_with_exclusions_extras
from customer_orders c
Join runner_orders r
On c.order_id = r.order_id
where r.distance != 'null'

```

   <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/d2596131-2fdd-444b-9f52-aa17bf8b4e0b">



   <li><h5>What was the total volume of pizzas ordered for each hour of the day?</h5></li>

   ```sql
Select
Extract (Hours from order_time) as hours,
Count(pizza_id) as "pizza ordered"
From customer_orders

Group by hours
Order by hours



```

   <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/571af1d6-8293-4332-a466-5d5c7c15728f">



   <li><h5>What was the volume of orders for each day of the week?</h5></li>

   ```sql
Select
to_char(order_time, 'Day') as day,
Count(pizza_id) as ordered_pizza
from customer_orders
Group by day

```

   <h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/55255c08-49f8-4b59-bd18-62c37c99fcf7">

