# Sql-for-data-analysis
To complete your Task 4: SQL for Data Analysis, here's a complete solution based on the guidelines in the image. The example will use a sample Ecommerce_SQL_Database. If you don’t have a database yet, I can give you a sample schema and dummy data too.


---

✅ README 

# Task 4: SQL for Data Analysis

## 🎯 Objective:
Use SQL queries to extract and analyze data from a database.

## 🛠 Tools Used:
- MySQL (You can also use PostgreSQL or SQLite)

## 📂 Dataset:
- Ecommerce_SQL_Database (Sample database with tables: Customers, Orders, Products, OrderDetails)

## 📄 Deliverables:
- SQL queries written in a `.sql` file
- Screenshots of output results

## 🧠 Skills Practiced:
- SELECT, WHERE, ORDER BY, GROUP BY
- JOINs (INNER, LEFT)
- Subqueries
- Aggregate Functions (SUM, AVG)
- Views
- Indexing (theoretical explanation)

## 🧪 Output:
Learned to manipulate and query structured data using SQL effectively.


---

📊 Sample Database Schema Used

Tables:

customers(customer_id, name, email)

products(product_id, name, price)

orders(order_id, customer_id, order_date)

order_details(order_detail_id, order_id, product_id, quantity)



---

📜 Sample SQL Queries 

-- 1. Total number of customers
SELECT COUNT(*) AS total_customers FROM customers;

-- 2. Top 5 most expensive products
SELECT name, price FROM products ORDER BY price DESC LIMIT 5;

-- 3. Total revenue generated
SELECT SUM(p.price * od.quantity) AS total_revenue
FROM order_details od
JOIN products p ON od.product_id = p.product_id;

-- 4. Orders placed by each customer (JOIN + GROUP BY)
SELECT c.name, COUNT(o.order_id) AS total_orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name;

-- 5. Average order value
SELECT AVG(order_total) AS average_order_value FROM (
    SELECT o.order_id, SUM(p.price * od.quantity) AS order_total
    FROM orders o
    JOIN order_details od ON o.order_id = od.order_id
    JOIN products p ON od.product_id = p.product_id
    GROUP BY o.order_id
) AS subquery;

-- 6. Create a view for customer order summary
CREATE VIEW customer_order_summary AS
SELECT c.customer_id, c.name, COUNT(o.order_id) AS order_count, 
       SUM(p.price * od.quantity) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
GROUP BY c.customer_id;

-- 7. Subquery: Customers who spent more than average
SELECT name FROM customers
WHERE customer_id IN (
    SELECT customer_id FROM customer_order_summary
    WHERE total_spent > (SELECT AVG(total_spent) FROM customer_order_summary)
);

-- 8. Indexing (theoretical)
-- CREATE INDEX idx_order_customer ON orders(customer_id);
-- CREATE INDEX idx_orderdetail_product ON order_details(product_id);


---


