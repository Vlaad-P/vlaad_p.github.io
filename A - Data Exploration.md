


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

---
 
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
 
---

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

---

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

---

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
---

### Column Aliases
Used to specify a name for an expression in the select statement. Can also be used to name other SQL expressions such as database tables, subqueries and common table expressions (CTEs). They vastly improve SQL code readability, reducing the time it takes to write, debug, understand or quickly scan the code.

## 2 - DISTINCT For Unique Values
Identifies how many unique values there are in a specific column or table. Helps with looking at the counts or frequencies, (the number of times certain combinations occur within a dataset).

---

### Show Unique Column Values
Use the `DISTINCT` keyword to obtain unique values from a deduplicated target column.
**example :** What are the unique values for the `rating` column in the `film` table?
```
SELECT DISTINCT
  rating
FROM dvd_rentals.film_list
;
```
**output:**
 rating | 
 ---|
PG|
NC-17|
PG-13|
G|
R|
---

### Count of Unique Values
Use the `COUNT` function with the `DISTINCT` keyword to find the number of unique values of a specific column.
**example :** How many unique `category` values are there in the `film_list` table?
```
SELECT
  COUNT(DISTINCT category) AS unique_category_count
FROM dvd_rentals.film_list
```
**output:**
unique_category_count | 
 ---|
16|

## 3 - Group By Counts

### Single Column Value Counts
Use  `GROUP BY` to group elements. Only 1 row is returned for each group. The `GROUP BY` must be used after the `FROM` statement
**example :** What is the frequency of values in the `rating` column in the `film` table?
```
SELECT
  rating,
  COUNT(*) AS frequency
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY frequency DESC;
```

**output:**
 rating| frequency
 ---|---|
PG-13|223
NC-17|210
R|195
PG|194
G|178

### Adding a Percentage Column
Sometimes the frequency is just not enough to really understand the frequency at a quick glance, so we like to create an additional percentage column to our dataset.
```
SELECT
  rating,
  COUNT(*) AS frequency,
  COUNT(*)::NUMERIC / SUM(COUNT(*)) OVER () AS percentage
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY frequency DESC;
```
**output:**
rating| frequency | percentage
 ---|---|---
PG-13|223|0.22367101303911735206
NC-17|210|0.21063189568706118355
R|195|0.19458375125376128385 
PG|194|0.19358074222668004012
G|178|0.17753259779338014042
>**good to know**. `::NUMERIC` right after the `COUNT(*)` avoids integer floor division.  The code uses a modified window function combining both `SUM` and `COUNT` functions with an `OVER()` clause.

We can improve percentage readability by multiplying it by 100 and round it to 1 decimal place using a `ROUND` function:
```
SELECT
rating,
COUNT(*) AS frequency,
ROUND(
100*COUNT(*)::NUMERIC / SUM (COUNT(*)) OVER(),
2) AS percentage
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY frequency DESC
;
```
**output:**
rating| frequency | percentage
 ---|---|---
PG-13|223|22.37
NC-17|210|21.06
R|195|19.46
PG|194|19.36
G|178|17.75

## 4 - Counts For Multiple Column Combinations
How best to analyse combinations of 2+ columns: Apply `GROUP BY` clause and specify additional columns in the grouping element expressions. When we use `GROUP BY` on 2+ columns, the subsequent `COUNT` function will aggregate the records based off the unique combination of values in these columns instead of just a single 1.

**example:** What are the 5 most frequent `rating` and `category` combinations in the `film_list` table?
```
SELECT
rating,
category,
COUNT(*) AS frequency,
FROM dvd_rentals.film_list
GROUP BY rating,category
ORDER BY frequency DESC
LIMIT 5
;
```
**output:**
rating| category | frequency
 ---|---|---
PG-13|Drama|22
NC-17|Music|20
PG-13|Animation|19
PG-13|Foreign|19
NC-17|New|18

## 6 - Exercises
1. Which `actor_id` has the most number of unique `film_id` records in the `dvd_rentals.film_actor` table?
```
SELECT
actor_id,
COUNT(DISTINCT film_id) AS number_of_films
FROM dvd_rentals.film_actor
GROUP BY actor_id
ORDER BY number_of_films DESC
LIMIT 3
;
```
**output:**
actor_id| number_of_films
 ---|---
 107|42
 102|41
 198|40
 
2. How many distinct `fid` values are there for the 3rd most common `price` value in the `dvd_rentals.nicer_but_slower_film_list` table?
```
SELECT
price,
COUNT(DISTINCT fid) AS values
FROM dvd_rentals.nicer_but_slower_film_list
GROUP BY price
ORDER BY price
LIMIT 3
;
```
**output:**
price| value
 ---|---
 ***0.99***|***340***
 2.99|323
 4.99|334

3. How many unique `country_id` values exist in the `dvd_rentals.city` table?
```
SELECT
COUNT(DISTINCT country_id) AS unique_country_id
FROM dvd_rentals.city
;
```
**output:**
unique_country_ids| 
 ---|
 109
 
4. What percentage of overall `total_sales` does the Sports `category` make up in the `dvd_rentals.sales_by_film_category` table?
```
SELECT
category,
SUM(total_sales) as total_sales,
ROUND(
100*SUM(total_sales)::NUMERIC / SUM(total_sales) OVER (),
2) as percentage_of_total_sales
FROM dvd_rentals.sales_by_film_category
GROUP BY category, total_sales
ORDER BY percentage_of_total_sales DESC
;
```
**output:**
category|total_sales|percentage_of_total_sales 
 ---|---|---|
***Sports***|***5314.21***|***7.88***
Sci-Fi|4756.98|7.06
Animation|4656.30|6.91
Drama|4587.39|6.80
Comedy|4383.58|6.50

5. What percentage of unique `fid` values are in the Children `category` in the `dvd_rentals.film_list` table?
```
SELECT
category,
ROUND(
100*COUNT(DISTINCT fid)::NUMERIC / SUM(COUNT(DISTINCT fid)) OVER (),
2) as fid_unique_percentage
FROM dvd_rentals.film_list
GROUP BY category
ORDER BY category
;
```
**output:**
category|fid_unique_percentage
 ---|---|
Action|6.42
Animation|6.62
***Children***|***6.02***
Classics|5.72
Comedy|5.82

 # A3 Identifying Duplicate Records
## 1 - Dataset Inspection

View the first 10 rows from the `health.user_logs` table:
```
SELECT *
FROM health.user_logs
LIMIT 10;
```
**output:**
id| log date|measure|measure value|systolic|diastolic
 ---|---|---|---|---|---|
fa28f948a740320ad56b81a24744c8b81df119fa|2020-11-15|weight|46.03959|null|null
1a7366eef15512d8f38133e7ce9778bce5b4a21e|2020-10-10|blood_glucose|97|0|0|
bd7eece38fb4ec71b3282d60080d296c4cf6ad5e|2020-10-18|blood_glucose|120|0|0
0f7b13f3f0512e6546b8d2c0d56e564a2408536a|2020-10-17|blood_glucose|232|0|0|
d14df0c8c1a5f172476b2a1b1f53cf23c6992027|2020-10-15|blood_pressure|140|140|113
0f7b13f3f0512e6546b8d2c0d56e564a2408536a|2020-10-21|blood_glucose|166|0|0
0f7b13f3f0512e6546b8d2c0d56e564a2408536a|2020-10-22|blood_glucose|142|0|0
87be2f14a5550389cb2cba03b3329c54c993f7d2|2020-10-12|weight|129.060012817|0|0
0efe1f378aec122877e5f24f204ea70709b1f5f8|2020-10-07|blood_glucose|138|0|0|
054250c692e07a9fa9e62e345231df4b54ff435d|2020-10-04|blood_glucose|210|null|null

### Records count:

```
SELECT COUNT(*)
FROM health.user_logs;
```
**output:**
count|
 ---|
43891|

### Unique column count
Use the `COUNT DISTINCT` to take a look at how many unique `id` values there are in this dataset.
```
SELECT COUNT(DISTINCT id)
FROM health.user_logs;
```
**output:**
count|
 ---|
554|
This gives us a feel for how many unique users there are.

### Single Column Frequency Counts
What is the most frequent measurement taken?
Use a column (`measure`) using a `GROUP BY` and `ORDER BY DESC` combo + the additional percentage column.
```
SELECT
measure,
COUNT(*) as frequency,
ROUND(
100*COUNT(*)::NUMERIC / SUM(COUNT(*)) OVER (),
2) as percentage
FROM health.user_logs
GROUP BY measure
ORDER BY percentage DESC;
```
**output
measure|frequency|percentage|
---|---|---|
blood_glucose|38692|88.15
weight|2782|6.34|
blood_pressure|2417|5.51

 # A4 Summary Statistics
 # A5 Distribution Functions
 # A6 Summary 
 # A7 Health Analytics Mini Case Study
 # A8 Case Study Quiz

 
 
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxMjU5NDU0Nyw5MTM5NDg5MDEsLTEzMD
g1MzAzNDUsMTI4MTkwOTgwMiwxMTQ2MDI0NzE4LDk2OTUyNjQ2
LC01MzI5ODc2MiwtMTQyNDk3MTUzMCwxNDAwMzE0MTI3LC0xNT
A1NzQ5MjczLDU5MjA1MzMwNSwxMjEwMTg5NjA3LC0xMDE1OTIx
OTA4LDE4NzI4NjA3ODksLTYzNTY4Mjk3OSwtNzQ4MTAzMTgxLD
EwMTU0MDg4NjgsLTIwNzk5MDE3MzgsMTY2OTE5NjcxMiwtMTc4
MzU5NDg5M119
-->