


 # A1 - Select & Sort Data
 
  ## 1 - How To Query Data
  
  ### Select ALL Columns
 Use `*` to inspect all the columns in a table.
**example:** Show all records from the `language` table from the `dvd_rentals` schema.
```
SELECT *
FROM dvd_rental. language;
```

---

  ### Select Specific Columns
 Separate columns with commas and make sure the spelling of each column is correct.
  **example:** Show only the `language_id` and `name` columns from the `language` table.
```
SELECT
language_id,
name
FROM dvd_rentals.language;
```
>**good to know.** Use ctrl+F / cmd+F on Win / Mac to highlight all commas for an easier inspection.

---

  ### Limit Output Rows 
When developing and testing new exploratory queries and uncertain about what sort of data we
 are dealing with  always limit output. Not limiting outputs on big data set queries could crash the entire system causing serious backlog.
**example :** Show the first 10 rows from the `actor` tables.
```
SELECT
language_id,
name
FROM  dvd_rentals.language
LIMIT 10;
```
>**good to know.** Some SQL flavours like SQL Server or Teradata use **TOP** instead of **LIMIT** and it goes in the front of the **SELECT** statement, like this: 
```
SELECT
Top 10*
name
FROM dvd_rentals.language
;
```

 ## 2 - Sorting Query Results
 
 ### Sort by Text Columns
 By using **ORDER BY** automatically sorts text columns in alphabetical order. 
```
SELECT country
FROM dvd_rentals.country
ORDER BY country
LIMIT 10;
```

---

 ### Sort by Numeric / Date Columns
 Sorting any numerical / date / timestamp columns is done from lowest to highest or latest to newest.  

```
SELECT total_sales
FROM dvd_rentals.sales_by_film_category
ORDER BY 1
LIMIT 10;
```
>**good to know.** We can refer to the **ORDER BY** column by it's  position in the final resulting output (in this case 1).

 ### Sort by Descending
 To reverse the sort order use **DESC** 
```
SELECT country
FROM dvd_rentals.country
ORDER BY country DESC
LIMIT 5;
```

---

 ### Sort by Multiple Columns
 We can also perform a multi-level sort by specifying 2 or more columns with the ORDER BY clause. Will use the following sample table for further deep dive:
 id | column_a | column_b
 ---|---|---|
 1|0|A
 2|0|B
 3|1|C
 4|1|D
 5|2|D
 6|3|D

Creating a TEMP table with above data
```
DROP TABLE IF EXISTS sample_table;
CREATE TEMP TABLE sample_table AS
WITH raw_data (id, column_a, column_b) AS (VALUES)
 (1, 0, 'A'),
 (2, 0, 'B'),
 (3, 1, 'C'),
 (4, 1, 'D'),
 (5, 2, 'D'),
 (6, 3, 'D')
)
SELECT * FROM raw_data;
```
---
#### Sorting both ascending
```
SELECT * FROM sample_table
ORDER BY column_a, column_b
```
**output:**
 id | column_a | column_b
 ---|---|---|
 1|0|A
 2|0|B
 3|1|C
 4|1|D
 5|2|D
 6|3|D
 
#### Sorting ascending & descending
```
SELECT * FROM sample_table
ORDER BY column_a DESC, column_b
```
**output:**
 id | column_a | column_b
 ---|---|---|
 6|3|D
 5|2|D
 3|1|C
 4|1|D
 1|0|A
 2|0|B
#### Sorting both descending 
```
SELECT * FROM sample_table
ORDER BY column_a DESC, column_b DESC
```
**output:**
 id | column_a | column_b
 ---|---|---|
 6|3|D
 5|2|D
 4|1|D
 3|1|C
 2|0|B
 1|0|A
#### Sorting with different column order
Order by `colum_b DESC` first instead of `column_a`
```
SELECT * FROM sample_table
ORDER BY column_b DESC, column_a;
```
**output:**
 id | column_a | column_b
 ---|---|---|
 4|1|D
 5|2|D
 6|3|D
 3|1|C
 2|0|B
 1|0|A
Order by `column_b` and `column_a DESC`
```
SELECT * FROM sample_table
ORDER BY column_b, column_a DESC;
```
**output:**
 id | column_a | column_b
 ---|---|---|
 1|0|A
 2|0|B
 3|1|C
 6|3|D
 5|2|D
 4|1|D

 ### Example Sorting Questions
 1. Which customer_id had the latest rental_date for inventory_id = 1 and 2?
```
SELECT 
customer_id,
inventory_id,
last_update
FROM dvd_rentals.rental
ORDER BY inventory_id, last_update DESC
LIMIT 10
;
```
**output:**
 customer_id | inventory_id | last_update
 ---|---|---|
 ***431***|***1***|***2006-02-15 21:30:53.000***
 518|1|2006-02-15 21:30:53.000
 279|1|2006-02-15 21:30:53.000
 ***411***|***2***|***2006-02-15 21:30:53.000***
 170|2|2006-02-15 21:30:53.000
 161|2|2006-02-15 21:30:53.000

 2. In the `dvd_rentals.sales_by_film_category` table, which category has the highest total_sales?
 ```
SELECT 
category,
total_sales
FROM dvd_rentals.sales_by_film_category
ORDER BY total_sales DESC
LIMIT 3
;
```
**output:**
 category | total_sales
 ---|---|
***Sports***|***5314.21***
 Sci-Fi|4756.98
 Animation|4656.30

 
 ## 3 - Exercises
1. What is the `name` of the category with the highest `category_id` in the `dvd_rentals.category` table?
 ```
SELECT 
name,
category_id
FROM dvd_rentals.category
ORDER BY category_id DESC
LIMIT 3
;
```
**output:**
 name | category_id
 ---|---|
***Sports***|***5314.21***
 Sci-Fi|4756.98
 Animation|4656.30
3. For the films with the longest `length`, what is the `title` of the “R” rated film with the lowest `replacement_cost` in `dvd_rentals.film` table?
4. Who was the `manager` of the store with the highest `total_sales` in the `dvd_rentals.sales_by_store` table?
5. What is the `postal_code` of the city with the 5th highest `city_id` in the `dvd_rentals.address` table?






  

























 # A2 Record Counts & Distinct Values
 # A3 Identifying Duplicate Records
 # A4 Summary Statistics
 # A5 Distribution Functions
 # A6 Summary 
 # A7 Health Analytics Mini Case Study
 # A8 Case Study Quiz

 
 
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY3NTMxNTA2LC0yMDc5OTAxNzM4LDE2Nj
kxOTY3MTIsLTE3ODM1OTQ4OTMsMjAyNTUwNzg5MywtMTk0MjA0
NTM3MywtNjU3NjE0MDcsLTg2NzczMzgzMSwxNTAwMTA3MDU2LC
0xNTY3MzI0NzYxLC0xNDkwMDI5Mzg3LC0yNjM4NDA3MjFdfQ==

-->