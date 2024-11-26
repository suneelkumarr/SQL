### Problem Statement

Write a SQL query to find the total number of people inside the hospital.


### Schema Setup

```sql
CREATE TABLE hospital (
    emp_id INT,
    action VARCHAR(10),
    time DATETIME
);

INSERT INTO hospital VALUES 
(1, 'in', '2019-12-22 09:00:00'),
(1, 'out', '2019-12-22 09:15:00'),
(2, 'in', '2019-12-22 09:00:00'),
(2, 'out', '2019-12-22 09:15:00'),
(2, 'in', '2019-12-22 09:30:00'),
(3, 'out', '2019-12-22 09:00:00'),
(3, 'in', '2019-12-22 09:15:00'),
(3, 'out', '2019-12-22 09:30:00'),
(3, 'in', '2019-12-22 09:45:00'),
(4, 'in', '2019-12-22 09:45:00'),
(5, 'out', '2019-12-22 09:40:00');
```

### Expected Output

people_inside | 
--| 
3 |


### Solution

```sql
-- Solution 1: usnig ROW_NUMBER()  // also my approach: 

WITH cte AS (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY emp_id ORDER BY [time] DESC) AS rn
    FROM hospital
)
SELECT COUNT(*) AS people_inside
FROM cte
WHERE rn = 1 AND action = 'in';


-- Solution 2: using intime > outtime logic

WITH CTE AS (
    SELECT 
        emp_id, 
        MAX(CASE WHEN action = 'in' THEN time END) AS intime,
        MAX(CASE WHEN action = 'out' THEN time END) AS outtime
    FROM hospital
    GROUP BY emp_id
)
SELECT COUNT(*) AS people_inside
FROM CTE
WHERE (intime > outtime OR outtime IS NULL) AND intime IS NOT NULL;
```

