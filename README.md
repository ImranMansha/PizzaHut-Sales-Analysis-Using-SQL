
# Pizza Sales Analysis with MySQL

## Project Overview

This project involves analyzing pizza sales data using MySQL to uncover valuable insights and drive data-driven decisions. The analysis focuses on understanding top-performing pizza types, revenue distribution across categories, and sales trends over time.

## Objectives

- **Identify High-Performing Pizza Types:** Determine which pizza types generate the most revenue and are most frequently ordered.
- **Analyze Revenue Distribution by Category:** Examine how different pizza categories contribute to total revenue.
- **Visualize Sales Trends Over Time:** Track and analyze cumulative revenue trends over time.

## Key Findings

- **Top-Performing Pizza Types:** Ranked list of the top 3 most ordered pizza types based on revenue.
- **Revenue Contribution Breakdown:** Percentage contribution of each pizza category to total revenue.
- **Sales Trend Visualization:** Visualization of cumulative revenue over time, revealing key sales patterns.

## Getting Started

To run the analysis, follow these steps:

1. **Clone the Repository:**

 
    git clone https://github.com/ImranMansha/PizzaHut-Sales-Analysis-Using-SQL


2. **Set Up the Database:**

    - Import the provided SQL schema and sample data into your MySQL database.
    - Ensure that the database schema matches the structure described in the project.

3. **Run Queries:**

    - Execute the SQL queries provided in the project to perform various analyses.

4. **Review Results:**

    - Review the outputs of the queries to gain insights into pizza sales data.

## SQL Queries

### 1. Identify the Highest-Priced Pizza

SELECT pizza_types.name, pizzas.price 
FROM pizzas 
JOIN pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;

### 2. List the Top 5 Most Ordered Pizza Types
```sql
WITH PizzaRevenue AS (
    SELECT pt.name, SUM(od.quantity * p.price) AS pizza_revenue
    FROM pizzas p
    JOIN order_details od ON p.pizza_id = od.pizza_id
    JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
    GROUP BY pt.name
)
SELECT name, pizza_revenue
FROM PizzaRevenue
ORDER BY pizza_revenue DESC
LIMIT 5;
```
### 3. Determine the Distribution of Orders by Hour of the Day
```sql
SELECT HOUR(order_time) AS hour, COUNT(*) AS order_count
FROM orders
GROUP BY hour
ORDER BY hour;
```
### 4. Calculate the Percentage Contribution of Each Category to Total Revenue

```sql
WITH TotalRevenue AS (
    SELECT SUM(od.quantity * p.price) AS total_revenue
    FROM pizzas p
    JOIN order_details od ON p.pizza_id = od.pizza_id
),
CategoryRevenue AS (
    SELECT pt.category, SUM(od.quantity * p.price) AS category_revenue
    FROM pizzas p
    JOIN order_details od ON p.pizza_id = od.pizza_id
    JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
    GROUP BY pt.category
)
SELECT category,
       ROUND((category_revenue / (SELECT total_revenue FROM TotalRevenue) * 100), 2) AS percentage_contribution
FROM CategoryRevenue
ORDER BY percentage_contribution DESC;
```
## Contributing

Feel free to fork the repository, make changes, and submit pull requests. Contributions and suggestions are welcome!

## Contact

For questions or feedback, please contact Imran Mansha at imansha752@gmail.com.
