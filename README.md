# SQL_Subqueries_ALL_TYPES_OF_SUBQUERIES
📚 Project Overview  All types of SQL subqueries using a sample Customer (master) and Orders (child) table structure. The project covers basic and advanced subquery concepts, including:  Simple Subqueries  Nested Subqueries  Correlated Subqueries  Scalar Subqueries  Inline Views (subquery in FROM clause)  Subqueries with IN, EXISTS, =
SELECT * FROM CUSTOMER;

SELECT * FROM ORDERS;

--SIMPLE SUBQUERY (IN WHERE clause)

SELECT customer_id, customer_name
FROM customer
WHERE customer_id IN (
SELECT customer_id FROM orders);


--NESTED SUBQUERY
SELECT customer_id, customer_name
FROM customer
WHERE customer_id IN (SELECT customer_id FROM orders
WHERE order_amount > (SELECT AVG(order_amount)FROM orders));

--SCALAR SUBQUERY (In SELECT)
SELECT order_id,customer_id,order_amount,
(SELECT SUM(order_amount) FROM orders) AS total_sales
FROM orders;

--INLINE VIEW
SELECT *
FROM (SELECT order_id, customer_id, order_amount
FROM orders
ORDER BY order_amount DESC
) top_orders
WHERE ROWNUM <= 5;

--CORRELATED SUBQUERY
SELECT o.order_id,o.customer_id,o.order_amount
FROM orders o
WHERE o.order_amount > (
        SELECT AVG(order_amount)
        FROM orders
        WHERE customer_id = o.customer_id);

--USING EXISTS 
SELECT customer_id, customer_name
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);

--USING = (Scalar comparison)
SELECT customer_name
FROM customer
WHERE customer_id = (
SELECT customer_id FROM orders
WHERE order_amount = (
SELECT MAX(order_amount) FROM orders));

--USING NOT EXISTS
SELECT customer_id, customer_name
FROM customer c
WHERE NOT EXISTS (SELECT 1 FROM orders o
WHERE o.customer_id = c.customer_id);

--INLINE VIEW + GROUPING
SELECT * FROM (SELECT customer_id,
SUM(order_amount) AS total_revenue
FROM orders
GROUP BY customer_id
ORDER BY total_revenue DESC
) revenue_table
WHERE ROWNUM = 1;
