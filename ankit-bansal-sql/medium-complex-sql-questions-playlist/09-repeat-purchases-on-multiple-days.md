### Problem Statement

Write a query to obtain the number of users who purchased the same product on two or more different days. Output the number of unique users.

**Link**: https://datalemur.com/questions/sql-repeat-purchases



### Solution

```sql
WITH purchases_by_users AS (
    SELECT  *, 
            DENSE_RANK() OVER(PARTITION BY user_id, product_id ORDER BY DATE(purchase_date)) AS num_purchases
    FROM    purchases
)
SELECT COUNT(DISTINCT user_id) AS users_num
FROM   purchases_by_users
WHERE  num_purchases > 1














-- repeat purchases on multiple days --

WITH purchases_by_users AS
(
    SELECT
        user_id,
        product_id, DATE(purchase_date),
        DENSE_RANK() OVER(PARTITION BY user_id, product_id ORDER BY DATE(purchase_date) ASC) AS purchase_num
    FROM
        purchases
)

SELECT
    COUNT(DISTINCT user_id) AS users_num
FROM
    purchases_by_users
WHERE
    purchase_num > 1;
```



