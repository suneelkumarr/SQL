### Problem Statement

For every customer that bought Photoshop, return a list of customers, and the total spent on all the products except for Photoshop products.

**Link**: https://datalemur.com/questions/photoshop-revenue-analysis 


### Schema Setup

```sql
CREATE TABLE adobe_transactions (
    customer_id INT,
    product VARCHAR(50),
    revenue INT
);

INSERT INTO adobe_transactions (customer_id, product, revenue) VALUES
(123, 'Photoshop', 50),
(123, 'Premier Pro', 100),
(123, 'After Effects', 50),
(234, 'Illustrator', 200),
(234, 'Premier Pro', 100);
```


### Solution

```sql
-- Solution 1: using IN


SELECT   at.customer_id, SUM(at.revenue) AS revenue
FROM     adobe_transactions at
JOIN     (SELECT customer_id 
          FROM   adobe_transactions 
          WHERE  product = 'Photoshop'
          GROUP BY customer_id) subq
ON       at.customer_id = subq.customer_id
WHERE    at.product != 'Photoshop'
GROUP BY at.customer_id;


-- Solution 2: using EXISTS (better practice to use instead of IN)


SELECT   a.customer_id, SUM(a.revenue) AS revenue
FROM     adobe_transactions a
WHERE    EXISTS (
              SELECT 1
              FROM   adobe_transactions b
              WHERE  b.product = 'Photoshop' AND a.customer_id = b.customer_id
          )
         AND a.product != 'Photoshop'  -- Explicitly refer to 'product' from the 'a' alias
GROUP BY a.customer_id;            -- Explicitly refer to 'customer_id' from the 'a' alias




-- NOTE: 

-- `EXISTS` is generally faster than `IN` when dealing with large datasets because it can short-circuit and stop searching as soon as it finds a match, rather than generating and searching through a full list of values like `IN`.


-- Why `EXISTS` is preferred in this case:

-- - `EXISTS` only needs to verify the existence of a match, which is usually quicker, especially if the customer_id column is indexed.

-- - `IN` requires building a full list of customer_ids from the subquery and then searching through this list for each row in the outer query, which can be less efficient with larger datasets.
```