# 🍕 SQL Data Analysis – Pizza Sales Project

This repository contains SQL queries used to analyze a pizza restaurant's sales data. The project demonstrates how structured query language can uncover valuable business insights from raw transactional data.

---

## 📁 Dataset Overview

The dataset includes the following tables:

- `orders`: Contains order timestamps.
- `order_details`: Contains order IDs, pizza IDs, and quantities.
- `pizzas`: Lists each pizza with size and price.
- `pizza_types`: Provides pizza names, categories, and ingredients.

---

## 🎯 Objectives

- Calculate total and average revenue.
- Identify best- and worst-selling pizzas.
- Analyze order volume by day and hour.
- Track cumulative revenue growth over time.
- Support data-driven business decisions.

---

## 📄 Files in This Repo

- `pizza_sales_analysis.sql` → contains all SQL queries used in the project.
- `README.md` → this documentation file.

---

## 🔧 Tools Used

- SQL (MySQL / PostgreSQL / SQLite)
- DB Browser / DBeaver / PgAdmin (any SQL editor)

---

## ✅ Solved Queries:-
1. Analyze the cumulative revenue generated over time.
```sql
select order_date, 
sum(revenue) 
over(order by order_date) as cum_revenue 
from
(select order_date, sum(price*quantity) as revenue
from pizzas join order_details
on pizzas.pizza_id = order_details.pizza_id
join orders
on orders.order_id = order_details.order_id
group by order_date) as sales;
```
2. Calculate the percentage contribution of each pizza type to total revenue.

```sql
SELECT 
    category,
    ROUND((SUM(quantity * price) / (SELECT 
                    SUM(pizzas.price * order_details.quantity)
                FROM
                    pizzas
                        JOIN
                    order_details ON pizzas.pizza_id = order_details.pizza_id)) * 100,
            2) AS revenue_in_percent
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY category;
```
3. Determine the top 3 most ordered pizza types based on revenue.
```sql
SELECT 
    pizza_types.name Pizza_Type,
    SUM(pizzas.price * order_details.quantity) AS revenue
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
        JOIN
    pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
GROUP BY Pizza_Type
ORDER BY revenue DESC
LIMIT 3;
```
4. Group the orders by date and calculate the average number of pizzas ordered per day.
```sql
select round(avg(sum_quantity),0) as avg_qunatity_per_day from 
(select order_date, sum(order_details.quantity) as sum_quantity
from orders join order_details
on order_details.order_id = orders.order_id
group by  order_date) order_quantity;
```
5. Join relevant tables to find the category-wise distribution of pizzas.
```sql
SELECT 
    category, COUNT(name)
FROM
    pizza_types
GROUP BY category;
```
6. Determine the distribution of orders by hour of the day.
```sql
SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY hour;
```
7. Join the necessary tables to find the total quantity of each pizza category ordered.
```sql
SELECT 
    pizza_types.category AS Category,
    SUM(order_details.quantity) AS Quantity
FROM
    pizzas
        JOIN
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY Category
ORDER BY Quantity DESC;
```
8. List the top 5 most ordered pizza types along with their quantities.
```sql
select pizza_types.name, sum(order_details.quantity) as total_orders
from pizzas join pizza_types
on pizzas.pizza_type_id = pizza_types.pizza_type_id
join order_details
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.name
order by total_orders desc
limit 5;
```
9. Identify the most common pizza size ordered.
```sql
select pizzas.size, count(order_details.order_details_id) as No_of_Orders
from pizzas join order_details
on pizzas.pizza_id = order_details.pizza_id
group by pizzas.size
order by No_of_Orders Desc
limit 1;
```
10. Identify the highest-priced pizza
```sql
select pizza_types.name, pizzas.price 
from pizza_types join pizzas
on pizzas.pizza_type_id = pizza_types.pizza_type_id
order by pizzas.price desc
limit 1;
```
11. Calculate the total revenue generated from pizza sales.
```sql
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_revenue
FROM
    order_details
        JOIN
    pizzas ON order_details.pizza_id = pizzas.pizza_id
```
12. Retrieve the total number of orders placed.
```sql
select count(*) as total_orders from orders;
```
---

## 🧠 Insights Gained

- Which pizzas generate the most revenue
- When the business receives the most orders
- Revenue trends over time
- Opportunities for promotions during low-sale periods

---

## 📬 Contact

**Sahil Alam**  
📧 official.sahilalam@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/sahilalam01)  
💻 [GitHub](https://github.com/sahilstechz)

---

## ⭐ Star this repo if you find it helpful!
