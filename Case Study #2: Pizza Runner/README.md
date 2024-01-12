![image](https://github.com/omarkhalled/8-Week-SQL-Challenge/assets/90888020/4cd8f152-d005-40ef-93b1-1f6cd96ff8dd)

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
