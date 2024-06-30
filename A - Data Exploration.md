


 # A1 Select & Sort Data
 
  ## 1 How To Query Data
  
  ### Select ALL Columns
  
  Use `*` to inspect all the columns in a table

**Example**
Show all records from the `language` table from the `dvd_rentals` schema
````sql
SELECT *
FROM dvd_rental. language;
````

  ### Select Specific Columns

 Separate columns with commas and make sure the spelling of each column is correct.
 
**Good to know** Use ctrl+F / cmd+F on Win / Mac to highlight all commas for an easier inspection

**Example**
Show only the `language_id` and `name` columns from the `language` table
````sql
SELECT
language_id,
name
FROM dvd_rentals.language;
````

  ### Limit Output Rows 

When developing and testing new exploratory queries and uncertain about what sort of data you are dealing with  always limit output. Not limiting outputs on big data set queries could crash the entire system causing serious backlog.

**Example**
Show the first 10 rows from the `actor` tables
````sql
SELECT
language_id,
name
FROM  dvd_rentals.language
LIMIT 10;
````

 

  ### Exercises
  

























 # A2 Record Counts & Distinct Values
 # A3 Identifying Duplicate Records
 # A4 Summary Statistics
 # A5 Distribution Functions
 # A6 Summary 
 # A7 Health Analytics Mini Case Study
 # A8 Case Study Quiz

 
 
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0OTAwMjkzODcsLTI2Mzg0MDcyMV19
-->