![GitHub last commit](https://img.shields.io/github/last-commit/Dharmika66/ecommerce-sales-insights?style=flat-square)
![Repo size](https://img.shields.io/github/repo-size/Dharmika66/ecommerce-sales-insights?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&style=flat-square)
![SQL](https://img.shields.io/badge/SQL-SQLite-lightgrey?logo=sqlite&style=flat-square)
# E-commerce Sales Insights

A data exploration project built with SQL and a synthetic dataset to simulate real-world e-commerce analytics. This project showcases skills in data querying, aggregation, joins, and business analysis using a rich and realistic dataset.

---

## **Dataset Overview**

The dataset includes:

- **customers.csv**: 5,000 customers with sign-up dates and country.
- **products.csv**: 1,000 products across 6 categories with price and cost.
- **orders.csv**: 30,000+ orders from customers over 2 years.
- **order_items.csv**: Line-level order details with quantity and price.
- **returns.csv**: Returned items with reasons and dates.

---

## **Schema Diagram**

```
customers      orders           order_items        products          returns
-----------    -----------      --------------     -------------     -------------
customer_id -> customer_id      order_id -> order_id     product_id -> product_id
                                product_id -> product_id                order_id -> order_id
```

---

## **Business Questions & SQL Queries**

### 1. Top 10 customers by total spending
```sql
SELECT c.customer_id, c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_spent DESC
LIMIT 10;
```

### 2. Monthly revenue trend
```sql
SELECT DATE_TRUNC('month', order_date) AS month, SUM(total_amount) AS revenue
FROM orders
WHERE status IN ('Shipped', 'Delivered')
GROUP BY month
ORDER BY month;
```

### 3. Most returned products
```sql
SELECT p.product_name, COUNT(*) AS return_count
FROM returns r
JOIN products p ON r.product_id = p.product_id
GROUP BY p.product_name
ORDER BY return_count DESC
LIMIT 10;
```

### 4. Repeat customer percentage
```sql
WITH order_counts AS (
  SELECT customer_id, COUNT(*) AS order_count
  FROM orders
  GROUP BY customer_id
)
SELECT
  ROUND(100.0 * SUM(CASE WHEN order_count > 1 THEN 1 ELSE 0 END) / COUNT(*), 2) AS repeat_customer_percentage
FROM order_counts;
```

### 5. Profit per product
```sql
SELECT 
  p.product_id, 
  p.product_name, 
  SUM(oi.quantity * (oi.item_price - p.cost)) AS total_profit
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_profit DESC;
```

---

## **How to Use**

1. Load the CSVs into a relational database (e.g., PostgreSQL, MySQL, SQLite).
2. Use the SQL queries above to answer business questions.
3. Modify or extend the queries for deeper insights (e.g., per-country revenue, churn trends).

---

## **Author**

Dharmika Aman — Data Explorer & Developer  
Project part of personal portfolio showcasing real-world data skills.

