## The SQL IN operator

`IN` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to specify a list of values that you'd like to include in the results. For example, the following query of data from the [Billboard Music Charts](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset) will return results for which the `year_rank` column is equal to one of the values in the list:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank IN (1, 2, 3)
```

As with [comparison operators](https://mode.com/sql-tutorial/sql-operators#comparison-operators-on-non-numerical-data), you can use non-numerical values, but they need to go inside single quotes. Regardless of the data type, the values in the list must be separated by commas. Here's another example:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist IN ('Taylor Swift', 'Usher', 'Ludacris')
```

### [](https://mode.com/sql-tutorial/sql-in-operator#sharpen-your-sql-skills)Sharpen your SQL skills

## Practice Problem

Write a query that shows all of the entries for Elvis and M.C. Hammer.  
  
**Hint:** M.C. Hammer is actually on the list under multiple names, so you may need to first write a query to figure out exactly how M.C. Hammer is listed. You're likely to face similar problems that require some exploration in many real-life scenarios.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "group_name" IN ('M.C. Hammer', 'Hammer', 'Elvis Presley')
```
