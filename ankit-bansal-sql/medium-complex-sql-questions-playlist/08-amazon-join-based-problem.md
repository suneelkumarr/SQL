### Problem Statement

There are 2 tables, first table has 5 records and second table has 10 records. You can assume any values in each of the tables. 

How many maximum and minimum records possible in case of inner join, left join, right join and full outer join ??



### Solution

```sql
-- Basically, a theoritical question: 

-- so for max records possible, all the records of table 1 and table 2 should be the same. 
-- and so for minimum records possible, all the records of table 1 and table 2 should be different.
```








To understand how many maximum and minimum records are possible with different types of joins, let's break down each case for an INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL OUTER JOIN. We'll assume the following:

Table 1 has 5 records.
Table 2 has 10 records.
The records in each table have a common key (like id) on which the join is performed.

### INNER JOIN
An INNER JOIN returns only the records where there is a match in both tables based on the join condition.

Minimum Records:
Minimum records: The minimum number of records in the result set is 0. If there is no match between the two tables (i.e., no common values in the joining column), then no rows will be returned.
Maximum Records:
Maximum records: The maximum number of records occurs when each record in Table 1 has a matching record in Table 2. If each record in Table 1 matches multiple records in Table 2, the result will have the maximum records. For instance, if every record in Table 1 matches all 10 records in Table 2, the result will have:

Maximum records
=
5
(
Table 1 records
)
×
10
(
matching Table 2 records
)
=
50
 records
Maximum records=5(Table 1 records)×10(matching Table 2 records)=50 records
So:

Minimum records: 0
Maximum records: 50
### LEFT JOIN (LEFT OUTER JOIN)
A LEFT JOIN returns all the records from the left table (Table 1) and the matched records from the right table (Table 2). If there is no match, NULL values will be returned for columns from Table 2.

Minimum Records:
Minimum records: The minimum number of records is the number of records in Table 1, which is 5. This happens if none of the records in Table 1 have a match in Table 2. In this case, each row from Table 1 will still appear with NULL values for the columns from Table 2.
Maximum Records:
Maximum records: The maximum number of records happens when each record in Table 1 matches all 10 records in Table 2. Thus, the result set will have:

Maximum records
=
5
(
Table 1 records
)
×
10
(
matching Table 2 records
)
=
50
 records
Maximum records=5(Table 1 records)×10(matching Table 2 records)=50 records
So:

Minimum records: 5
Maximum records: 50
### RIGHT JOIN (RIGHT OUTER JOIN)
A RIGHT JOIN returns all records from the right table (Table 2) and the matched records from the left table (Table 1). If there is no match, NULL values will be returned for columns from Table 1.

Minimum Records:
Minimum records: The minimum number of records is the number of records in Table 2, which is 10. This happens if none of the records in Table 2 have a match in Table 1. In this case, each row from Table 2 will still appear with NULL values for the columns from Table 1.
Maximum Records:
Maximum records: The maximum number of records happens when each record in Table 2 matches all 5 records in Table 1. Thus, the result set will have:

Maximum records
=
10
(
Table 2 records
)
×
5
(
matching Table 1 records
)
=
50
 records
Maximum records=10(Table 2 records)×5(matching Table 1 records)=50 records
So:

Minimum records: 10
Maximum records: 50

### FULL OUTER JOIN
A FULL OUTER JOIN returns all records when there is a match in either left (Table 1) or right (Table 2) table. It returns NULL for unmatched rows in both tables.

Minimum Records:
Minimum records: The minimum number of records occurs when there are no matching rows between the two tables. In this case, all 5 records from Table 1 and all 10 records from Table 2 will appear, but with NULL values where no match exists. The minimum number of records will be the total number of records from both tables:

Minimum records
=
5
(
Table 1 records
)
+
10
(
Table 2 records
)
=
15
 records
Minimum records=5(Table 1 records)+10(Table 2 records)=15 records
Maximum Records:
Maximum records: The maximum number of records happens when each record in Table 1 matches all 10 records in Table 2, and each record in Table 2 matches all 5 records in Table 1. This results in:

Maximum records
=
5
(
Table 1 records
)
×
10
(
matching Table 2 records
)
+
10
(
Table 2 records
)
×
5
(
matching Table 1 records
)
=
50
 records
Maximum records=5(Table 1 records)×10(matching Table 2 records)+10(Table 2 records)×5(matching Table 1 records)=50 records
But, we need to consider only the total unique records, so in the worst case, this would still be 50.

So:

Minimum records: 15
Maximum records: 50
Summary of Maximum and Minimum Records:
Join Type	Minimum Records	Maximum Records
INNER JOIN	0	50
LEFT JOIN	5	50
RIGHT JOIN	10	50
FULL OUTER JOIN	15	50
In each case, the maximum number of records is based on the assumption that every record from one table matches every record from the other table. The minimum number of records is determined by the conditions of the join type (such as unmatched rows in INNER JOIN or unmatched rows in LEFT/RIGHT/FULL OUTER JOIN).