


 # A1 - Select & Sort Data
 
  ## 1 - How To Query Data
Simple `SELECT` statements and `ORDER BY` clause to sort the output to answer basic questions
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
***Travel***|***16***
Sports|15
  Sci-Fi|14
  
2. For the films with the longest `length`, what is the `title` of the “R” rated film with the lowest `replacement_cost` in `dvd_rentals.film` table?
 ```
SELECT 
title,
length,
rating,
replacement_cost
FROM dvd_rentals.film
ORDER BY 2 DESC,4
LIMIT 10
;
```
**output:**
title | length | rating | replacement_cost
 ---|---| ---|---|
CONTROL ANTHEM | 185 | G | 9.99
CHICAGO NORTH | 185 | PG-13 | 11.99
DARN FORRESTER|185|G|14.99|
***HOME PITY***|***185***|***R***|***15.99***
MUSCLE BRIGHT|185|G|23.99
POND SEATTLE|185|PG-13|25.99
WORST BANGER|185|PG|26.99
SWEET BROTHERHOOD|185|R|27.99
GANGS PRIDE|185|PG-13|27.99
SOLDIERS EVOLUTION|185|R|27.99

3. Who was the `manager` of the store with the highest `total_sales` in the `dvd_rentals.sales_by_store` table?
 ```
SELECT 
manager,
total_sales
FROM dvd_rentals.sales_by_store
ORDER BY 2 DESC
LIMIT 3
;
```
**output:**
 name | category_id
 ---|---|
***Jon Stephens***|***33726.77***
Mike Hillyer|33689.74

4. What is the `postal_code` of the city with the 5th highest `city_id` in the `dvd_rentals.address` table?
 ```
SELECT 
postal_code,
city_id
FROM dvd_rentals.address
ORDER BY 2 DESC
LIMIT 5
;
```
**output:**
 postal_code | city_id
 ---|---|
75559|600
39976|599
95093|598
40792|597
***31390***|***596***

 # A2 Record Counts & Distinct Values

`COUNT` function | `DISTINCT` keyword | `GROUP BY` clauses and how they work under the hood | `ORDER BY` with `GROUP BY` to selectively sort the output |  Column aliases to rename `SELECT` expressions in the final output |    Frequency/counts for a single column and multiple column combinations | Calculate the counts percentage for groups using window functions

## 1 - How Many Records?
It is important to know how many records there are in a table prior to running further analytical queries on the dataset as the data size will have significant impact on performance.
**example :** How many rows are there in the `film_list` table?
```
SELECT
  COUNT(*) AS row_count
FROM dvd_rentals.film_list;
```
### Column Aliases
Used to specify a name for an expression in the select statement. Can also be used to name other SQL expressions such as database tables, subqueries and common table expressions (CTEs). They vastly improve SQL code readability, reducing the time it takes to write, debug, understand or quickly scan the code.

## 2 - DISTINCT For Unique Values
Identifies how many unique values there are in a specific column or table. More commonly we will try to look at the counts or frequencies, the number of times certain combinations occur within a dataset.
### Show Unique Column Values
### Count of Unique Values
## 3 - Group By Counts
#### Dividing Rows
#### Apply Aggregate Count Function
#### Combining Condensed Outputs
#### Single Column Value Counts
#### Adding a Percentage Column
## 4 - Counts For Multiple Column Combinations
#### Using Positional Numbers Instead of Column Names
## 5 - Integer floor division
## 6 - Exercises
1. Which `actor_id` has the most number of unique `film_id` records in the `dvd_rentals.film_actor` table?
2. How many distinct `fid` values are there for the 3rd most common `price` value in the `dvd_rentals.nicer_but_slower_film_list` table?
3. How many unique `country_id` values exist in the `dvd_rentals.city` table?
4. What percentage of overall `total_sales` does the Sports `category` make up in the `dvd_rentals.sales_by_film_category` table?
5. What percentage of unique `fid` values are in the Children `category` in the `dvd_rentals.film_list` table?
6. 
























 # A3 Identifying Duplicate Records
 # A4 Summary Statistics
 # A5 Distribution Functions
 # A6 Summary 
 # A7 Health Analytics Mini Case Study
 # A8 Case Study Quiz

 
 
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzk3MDEyMTQ3LDE4NzI4NjA3ODksLTYzNT
Y4Mjk3OSwtNzQ4MTAzMTgxLDEwMTU0MDg4NjgsLTIwNzk5MDE3
MzgsMTY2OTE5NjcxMiwtMTc4MzU5NDg5MywyMDI1NTA3ODkzLC
0xOTQyMDQ1MzczLC02NTc2MTQwNywtODY3NzMzODMxLDE1MDAx
MDcwNTYsLTE1NjczMjQ3NjEsLTE0OTAwMjkzODcsLTI2Mzg0MD
cyMV19
-->