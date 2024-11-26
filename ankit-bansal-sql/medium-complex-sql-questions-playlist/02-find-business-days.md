### Problem Statement

Write a SQL query to find business days between created date and resolved date, excluding the weekends and public holidays.


### Schema Setup

```sql
CREATE TABLE tickets (
    ticket_id VARCHAR(10),
    create_date DATE,
    resolved_date DATE
);

DELETE FROM tickets;

INSERT INTO tickets VALUES
(1, '2022-08-01', '2022-08-03'),
(2, '2022-08-01', '2022-08-12'),
(3, '2022-08-01', '2022-08-16');


CREATE TABLE holidays (
    holiday_date DATE,
    reason VARCHAR(100)
);

DELETE FROM holidays;

INSERT INTO holidays VALUES
('2022-08-11', 'Rakhi'),
('2022-08-15', 'Independence day');
```

### Expected Output

ticket_id |	business_days |
--|--|
1 |	3 |
2 |	9 |
3 |	10 |


### Solution

```sql
-- my approach


WITH date_range AS (
    SELECT ticket_id, create_date AS curr_date, resolved_date
    FROM tickets
    WHERE create_date <= resolved_date
    UNION ALL
    SELECT dr.ticket_id, DATEADD(DAY, 1, dr.curr_date) AS curr_date, dr.resolved_date
    FROM date_range dr
    WHERE DATEADD(DAY, 1, dr.curr_date) <= dr.resolved_date
),
business_days AS (
    SELECT ticket_id, curr_date
    FROM date_range
    WHERE DATEPART(WEEKDAY, curr_date) NOT IN (1, 7) -- Exclude Sunday (1) and Saturday (7)
),
valid_business_days AS (
    SELECT bd.ticket_id, bd.curr_date
    FROM business_days bd
    LEFT JOIN holidays h ON bd.curr_date = h.holiday_date
    WHERE h.holiday_date IS NULL
)
SELECT ticket_id, COUNT(*) AS business_days
FROM valid_business_days
GROUP BY ticket_id
ORDER BY ticket_id;


-- method 2
WITH numbers AS (
    SELECT ROW_NUMBER() OVER (ORDER BY (SELECT NULL)) - 1 AS n
    FROM master.dbo.spt_values -- Built-in table with many rows
),
date_range AS (
    SELECT 
        t.ticket_id,
        DATEADD(DAY, n, t.create_date) AS curr_date
    FROM tickets t
    JOIN numbers n ON n.n <= DATEDIFF(DAY, t.create_date, t.resolved_date)
),
business_days AS (
    SELECT ticket_id, curr_date
    FROM date_range
    WHERE DATEPART(WEEKDAY, curr_date) NOT IN (1, 7) -- Exclude Sunday (1) and Saturday (7)
),
valid_business_days AS (
    SELECT bd.ticket_id, bd.curr_date
    FROM business_days bd
    LEFT JOIN holidays h ON bd.curr_date = h.holiday_date
    WHERE h.holiday_date IS NULL
)
SELECT ticket_id, COUNT(*) AS business_days
FROM valid_business_days
GROUP BY ticket_id
ORDER BY ticket_id;







-- NOTE: 

-- in the video he has used different approach, much shorter. 

-- also my approach is giving a slight different answer; like instead of 2, 8, 9 it is giving 3, 9, 10 in the business days column.

-- but I cant seem to figure out the mistake.
```

