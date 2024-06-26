<h1>Case Study #3 - Foodie-Fi🫕🥑</h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/abd6c8a0-371d-4c17-8e36-4a22fd894c20">
<h1>Contents</h1>

<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#entityrelationshipdiagram">Entity Relationship Diagram</a></li>
  <li><a href="#casestudyquestionsandsolutions">Case Study Questions & Solutions</a></li>

<ul>
  <li><a href="#a.customerjourney">A. Customer Journey</a></li>
  <li><a href="#b.dataanalysisquestions">B. Data Analysis Questions</a></li>
  <li><a href="#c.challengepaymentquestion">C. Challenge Payment Question</a></li>
  </ul>
  
  
</ul>

<h1><a name="introduction">Introduction</a></h1>
<p>In response to the growing popularity of subscription-based businesses, Danny recognized a significant market gap. He saw an opportunity to introduce a distinctive streaming service, one that exclusively featured culinary content—a culinary counterpart to platforms like Netflix. With this vision in mind, Danny collaborated with a group of astute friends to establish Foodie-Fi, a startup launched in 2020. The company quickly began offering monthly and annual subscription plans, granting subscribers unrestricted access to an array of exclusive culinary videos sourced from across the globe.<br>

At the heart of Foodie-Fi's foundation lies Danny's commitment to data-driven decision-making. He was resolute in leveraging data to inform all future investments and innovative features. This case study delves into the realm of subscription-style digital data, where insights derived from data analysis are pivotal in addressing critical business inquiries and steering the company toward sustained growth and success.</p>

<h1><a name="entityrelationshipdiagram"></a>Entity Relationship Diagram</h1>
<img width="500" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/82dd908e-38b9-4d7f-a17e-743bc745469d">

<h1><a name="casestudyquestionsandsolutions"></a>Case Study Questions & Solutions</h1>

<h4><a name="a.customerjourney"></a>A. Customer Journey🫕🥑</h4>

<ol> 
  <li><h5>Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.</h5></li>

  ```sql
SELECT
    s.customer_id,
    p.plan_name,
    s.start_date
FROM
    subscriptions s
JOIN
    plans p ON s.plan_id = p.plan_id
ORDER BY
    s.customer_id, s.start_date;
```

<h6>Answer:</h6>
<img width="250" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/eb8df25b-8606-4bea-9008-e67b15c27a77">
</ol>

<ol>
  <li>
    <strong>Customer 1:</strong>
    <ul>
      <li>Started with a trial plan on 2020-08-01.</li>
      <li>Upgraded to the basic monthly plan on 2020-08-08.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 2:</strong>
    <ul>
      <li>Started with a trial plan on 2020-09-20.</li>
      <li>Upgraded to the pro annual plan on 2020-09-27.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 11:</strong>
    <ul>
      <li>Started with a trial plan on 2020-11-19.</li>
      <li>Downgraded to the churn plan on 2020-11-26.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 13:</strong>
    <ul>
      <li>Started with a trial plan on 2020-12-15.</li>
      <li>Upgraded to the basic monthly plan on 2020-12-22.</li>
      <li>Upgraded to the pro monthly plan on 2021-03-29.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 15:</strong>
    <ul>
      <li>Started with a trial plan on 2020-03-17.</li>
      <li>Upgraded to the pro monthly plan on 2020-03-24.</li>
      <li>Downgraded to the churn plan on 2020-04-29.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 16:</strong>
    <ul>
      <li>Started with a trial plan on 2020-05-31.</li>
      <li>Upgraded to the basic monthly plan on 2020-06-07.</li>
      <li>Upgraded to the pro annual plan on 2020-10-21.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 18:</strong>
    <ul>
      <li>Started with a trial plan on 2020-07-06.</li>
      <li>Upgraded to the pro monthly plan on 2020-07-13.</li>
    </ul>
  </li>
  <li>
    <strong>Customer 19:</strong>
    <ul>
      <li>Started with a trial plan on 2020-06-22.</li>
      <li>Upgraded to the pro monthly plan on 2020-06-29.</li>
      <li>Upgraded to the pro annual plan on 2020-08-29.</li>
    </ul>
  </li>
</ol>
</ol>

<h4><a name="b.dataanalysisquestions"></a>B. Data Analysis Questions📊</h4>
<ol>
  <li><h5>How many customers has Foodie-Fi ever had?</h5></li>

  ```sql
Select count(distinct customer_id) as customer_count
from subscriptions
```
   <h6>Answer:</h6>
  <img width="150" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/c5d03554-ff58-44c0-96cf-dd13ed9483b4">


  
  <li><h5>What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value</h5></li>

  ```sql
Select 
date_trunc('month', start_date) as month_start,
count(*) as trial_starts
From subscriptions
where plan_id = 0
Group by month_start
```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/8d1a6bef-80c1-4392-9b8f-5ee08b249a10">

  
  <li><h5>What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name</h5></li>

  ```sql
Select p.plan_name, p.plan_id,
COUNT(*) as event_count

From plans p
Join subscriptions s
On p.plan_id = s.plan_id
where s.start_date > '2020-12-31'

Group by p.plan_name , p.plan_id
Order by p.plan_name
```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/ba0305f6-a4c7-4100-8cb0-0b96cf5f58f1">


  
  <li><h5>What is the customer count and percentage of customers who have churned rounded to 1 decimal place?</h5></li>

  ```sql
Select
Count(distinct case when s.plan_id = 4 and s.start_date <= current_date then s.customer_id end) as churned_customers,

Count(distinct case when s.plan_id = 4 and s.start_date <= current_date then s.customer_id end)*100/Count(distinct s.customer_id) as churned_percentage

From subscriptions s

```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/e3e6ab3e-bfe8-43b2-aa0f-157f29b32a1c">


  
  <li><h5>How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?</h5></li>

  ```sql
with cte as 
(Select s.customer_id, p.plan_name,
 Lead(p.plan_name) Over(Partition by s.customer_id Order by s.start_date) as next_plan
 From subscriptions s
 Join plans p
 On s.plan_id  = p.plan_id)
 
 Select Count(customer_id) as customers,
 Count(customer_id) * 100/ (Select Count(Distinct customer_id) 
 from subscriptions) as churn_percentage
 
 from cte
 where plan_name = 'trial'
 And next_plan = 'churn'

```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/80d4fcdc-7aff-4c12-8b8b-8c8c7154d204">


  
  <li><h5>What is the number and percentage of customer plans after their initial free trial?</h5></li>

  ```sql
with cte as 
(Select s.customer_id, p.plan_id,
 Lead(p.plan_id) Over(Partition by s.customer_id Order by p.plan_id) as next_plan_id
 From subscriptions s
 Join plans p
 On s.plan_id  = p.plan_id)
 
 Select next_plan_id as plan_id,
 Count(customer_id) as after_free_customers,
 Count(customer_id) * 100/ (Select Count(Distinct customer_id) 
 from subscriptions) as after_percentage
 
 from cte
 where plan_id is not null AND plan_id = 0
 Group by next_plan_id
 Order by next_plan_id
```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/658fc25d-ce6e-4617-b16a-2f59e7d254ac">

  
  <li><h5>What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?</h5></li>

  ```sql
Select p.plan_name,
Count(distinct s.customer_id) as customer_count,
Round(100.0 * Count(Distinct s.customer_id) / (Select Count(Distinct customer_id) 
From subscriptions), 1) AS percentage  

From subscriptions s
Join plans p
On s.plan_id = p.plan_id
where s.start_date <= '2020-12-31'
Group by p.plan_name, p.plan_id
ORDER BY p.plan_id

````
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/a7865e1f-d503-42d7-b27f-8795fbee0a4f">
 

  
  <li><h5>How many customers have upgraded to an annual plan in 2020?</h5></li>

  ```sql
Select count(distinct customer_id) as upgraded_customers_count
From subscriptions

where start_date >= '2020-01-01'
and start_date <= '2020-12-31'
And plan_id = 3
```
  <h6>Answer:</h6>
  <img width="200" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/9941e762-8415-4b50-b9db-012650cba5d1">


  
  <li><h5>How many customers downgraded from a pro monthly to a basic monthly plan in 2020?</h5></li>

  ```sql
Select count(*) as num_downgrades 
from subscriptions prev
Join subscriptions current
On prev.plan_id = current.plan_id
And prev.plan_id = 2
And current.plan_id = 1
And prev.start_date <= current.start_date

where extract(year from prev.start_date)  = '2020
```
  <h6>Answer:</h6>
  <img width="150" alt="Coding" src="https://github.com/Mariyajoseph24/8_Week_SQL_challenge/assets/91487663/8e50e760-b694-4a90-832a-1338e13e2115">


  

