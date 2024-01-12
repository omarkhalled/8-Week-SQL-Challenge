## 📚 Table of Contents
- [Business Task](#business-task)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- Solution
  - [Data Cleaning and Transformation](#-data-cleaning--transformation)
  - [A. Pizza Metrics](#a-pizza-metrics)
  - [B. Runner and Customer Experience](#b-runner-and-customer-experience)
  - [C. Ingredient Optimisation](#c-ingredient-optimisation)
  - [D. Pricing and Ratings](#d-pricing-and-ratings)
 
## Business Task
Danny is expanding his new Pizza Empire and at the same time, he wants to Uberize it, so Pizza Runner was launched!

Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers. 

## Entity Relationship Diagram
![image](https://github.com/omarkhalled/8-Week-SQL-Challenge/assets/90888020/46c510bd-f676-4152-a2ca-d7b0a3df7462)

## 🧼 Data Cleaning & Transformation

Looking at the `customer_orders` table below, we can see that there are
- In the `exclusions` column, there are missing/ blank spaces ' ' and null values. 
- In the `extras` column, there are missing/ blank spaces ' ' and null values.
![image](https://github.com/omarkhalled/8-Week-SQL-Challenge/assets/90888020/8b432b53-4756-4ed1-a968-b2c716fcd304)

````sql
Select order_id , customer_id , pizza_id ,
(case when exclusions is NULL or exclusions like 'null' then ' '
 else exclusions end )as exclusions,
 (case when extras is NULL or extras like 'null' then ' '
 else extras end )as extras,
 order_time
 INTO #tmp_customer_orders
from customer_orders
select *
from #tmp_customer_orders
`````

Looking at the `runner_orders` table below, we can see that there are
- In `pickup_time` column, remove nulls and replace with blank space ' '.
- In `distance` column, remove "km" and nulls and replace with blank space ' '.
- In `duration` column, remove "minutes", "minute" and nulls and replace with blank space ' '.
- In `cancellation` column, remove NULL and null and and replace with blank space ' '.
![image](https://github.com/omarkhalled/8-Week-SQL-Challenge/assets/90888020/2926b53f-c14d-40a0-b657-983799fb7462)

````sql

select order_id , runner_id ,
( case
  when pickup_time like 'null' then ''
  else pickup_time
  end
) as pickup_time ,
(
  case
  when distance like 'null' then ''
  when distance like '%km'then  REPLACE(distance,'km','')
  when distance like '%km'then  REPLACE(distance,' km','')
  else distance
  end

) as distance ,
(
  case
  when duration like 'null' then ''
  when duration like '%minutes'then  REPLACE(duration,'minutes','')
  when duration like '%mins'then  REPLACE(duration,'mins','')
  when duration like '%mins'then  REPLACE(duration,' mins','')
  when duration like '%minutes'then  REPLACE(duration,' minutes','')
  when duration like '%minute'then  REPLACE(duration,' minute','')
  else duration
  end
) as duration,
(
  case
  when cancellation like 'null' or cancellation is null then ''
  else cancellation
  end
) as cancellation
into #tmp_runner_orders
from runner_orders
`````

Then we will alter columns to correct data type

````sql

alter table #tmp_runner_orders
alter column pickup_time datetime
alter table #tmp_runner_orders
alter column distance float
alter table #tmp_runner_orders
alter column duration int

`````

## A. Pizza Metrics

