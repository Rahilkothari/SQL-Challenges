
<h1>Case Study #1 - Danny's Diner👨🏻‍🍳</h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/c69d49a0-ffd6-4cf1-b66f-1d1eb14f8549">
<h1>Contents</h1>
<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#problemstatement">Problem Statement</a></li>
  <li><a href="#entityrelationshipdiagram">Entity Relationship Diagram</a></li>
  <li><a href="#casestudyquestionsandsolutions">Case Study Questions & Solutions</a></li>
  <li><a href="#bonusquestionsandsolutions">Bonus Questions & Solutions</a></li>
  <li><a href="#keyinsights">Key Insights</a></li>
</ul>

<h1><a name="introduction">Introduction</a></h1>
<p>In early 2021, Danny follows his passion for Japanese food and opens "Danny's Diner," a charming restaurant offering sushi, curry, and ramen. However, lacking data analysis expertise, the restaurant struggles to leverage the basic data collected during its initial months to make informed business decisions. Danny's Diner seeks assistance in using this data effectively to keep the restaurant thriving.</p>

<h1><a name="problemstatement">Problem Statement</a></h1>
<p>Danny aims to utilize customer data to gain valuable insights into their visiting patterns, spending habits, and favorite menu items. By establishing a deeper connection with his customers, he can provide a more personalized experience for his loyal patrons.

He plans to use these insights to make informed decisions about expanding the existing customer loyalty program. Additionally, Danny seeks assistance in generating basic datasets for his team to inspect the data conveniently, without requiring SQL expertise.

Due to privacy concerns, he has shared a sample of his overall customer data, hoping it will be sufficient for you to create fully functional SQL queries to address his questions.

The case study revolves around three key datasets:

- Sales
- Menu
- Members</p>

<h1><a name="entityrelationshipdiagram">Entity Relationship Diagram</a></h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/4bc1a02f-6fac-47f5-82c7-d9be65de1700">


<h1><a name="casestudyquestionsandsolutions">Case Study Questions & Solutions</a></h1>

<ol>

  <li><h5>What is the total amount each customer spent at the restaurant?</h5></li>
	
```sql
Select s.customer_id, sum(m.price) as total_amnt
From sales s
Join menu m
On s.product_id = m.product_id
Group by s.customer_id
Order by s.customer_id
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/15c40926-3faf-43a6-91de-5e01f12955fc">



  <li><h5>How many days has each customer visited the restaurant?</h5></li>

```sql
Select customer_id, count(distinct order_date) as No_Days
from sales
group by customer_id
```
<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/8e92fa69-0d80-4713-899d-001ee361f30f">




  <li><h5>What was the first item from the menu purchased by each customer?</h5></li>

```sql
with cte as(
  Select s.customer_id, dense_rank() over(partition by customer_id order by order_date) as rnk, m.product_name
  from sales s
  Join menu m
  On s.product_id  = m.product_id)
  
  Select customer_id, product_name
  From cte 
  where rnk = 1
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/09694137-3bf0-48a0-8cc6-f9fff7f631d7)">



  <li><h5>What is the most purchased item on the menu and how many times was it purchased by all customers?</h5></li>

```sql
Select m.product_name, Count(s.product_id) as most_ordered
From sales s
Join menu m
On s.product_id = m.product_id
Group by m.product_name
Order by most_ordered desc
Limit 1
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/ceb3fa1b-d1f1-4c88-b3f1-6e642a5e8c70)">




  <li><h5>Which item was the most popular for each customer?</h5></li>

```sql
with cte as 
(Select s.customer_id, m.product_name, Count(s.product_id) as order_count,
Dense_rank() over(partition by s.customer_id order by count(s.product_id)desc) as rnk
From sales s
Join menu m
On s.product_id = m.product_id
Group by s.customer_id, m.product_name)

Select customer_id, product_name, order_count
from cte
where rnk = 1
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/9b194b28-afed-43b7-bfbc-d396c80877c0">



  <li><h5>Which item was purchased first by the customer after they became a member?</h5></li>

  ```sql
Select distinct on(s.customer_id)
  s.customer_id, 
  m.product_name
from sales s
Join members mbr 
On s.customer_id  = mbr.customer_id
Join menu m 
On s.product_id = m.product_id


where s.order_date > mbr.join_date
Order by s.customer_id
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/ca782906-d2a5-4f85-ae09-879818790462">



  <li><h5>Which item was purchased just before the customer became a member?</h5></li>

  ```sql
Select distinct on(s.customer_id)
  s.customer_id, 
  m.product_name
from sales s
Join members mbr 
On s.customer_id  = mbr.customer_id
Join menu m 
On s.product_id = m.product_id


where s.order_date < mbr.join_date
Order by s.customer_id
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/76aef691-9fed-4b92-9ead-721187fbf711">



  <li><h5>What is the total items and amount spent for each member before they became a member?</h5></li>

  ```sql
Select s.customer_id, count(s.product_id) as total_item,sum(m.price) as total_amont
from sales s
Join members mbr 
On s.customer_id  = mbr.customer_id
Join menu m 
On s.product_id = m.product_id


where s.order_date < mbr.join_date
Group by s.customer_id
Order by s.customer_id
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/4806449e-5d44-47eb-8a4b-6d75c226a5c0">



  <li><h5>If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?</h5></li>

  ```sql
Select s.customer_id,
	sum(case
        when m.product_name = 'sushi' then m.price * 2 * 10
        else m.price * 10
        end) as total_points
From sales s
Join menu m
On s.product_id = m.product_id
Group by s.customer_id
Order by s.customer_id
```

<h6>Answer:</h6>
<img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/fce539f7-78cd-47fd-9aac-bee87e6974ee">



 










