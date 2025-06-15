## Automatic LIMIT in Mode

In [Mode's Query Editor](https://mode.com/sql-tutorial/sql-in-mode), you may have noticed the checkbox next to "Run" that says "Limit 100".

[![](https://mode.com/resources/images/the-basics/limit-box.png)](https://mode.com/resources/images/the-basics/limit-box.png "Be sure to un-check the Limit box if you want to return more than 100 rows")
As you might expect, the limit restricts how many rows the SQL query returns. The default value is 100; when this box is checked, it's telling the database to only return the first 100 rows of the query. Because [the dataset](https://mode.com/sql-tutorial/sql-in-mode#about-this-dataset) `tutorial.us_housing_units` has more than 100 rows, the queries thus far haven't been returning the full result sets. Try turning the `LIMIT` off (by clicking the check mark next to it) and running this query.

```
SELECT *
  FROM tutorial.us_housing_units
```

You'll notice many more rows get returned. You can tell by checking the header of the results table:

[![](https://mode.com/resources/images/the-basics/rows-returned.png)](https://mode.com/resources/images/the-basics/rows-returned.png "Checking the number of rows returned can help you get a sense of whether or not the query you wrote is correct")
## [](https://mode.com/sql-tutorial/sql-limit#why-should-you-limit-your-results)Why should you limit your results?

Many analysts use limits as a simple way to keep their queries from taking too long to return. The aim of many of your queries will simply be to see what a particular table looks like—you'll want to scan the first few rows of data to get an idea of which fields you care about and how you want to manipulate them. If you query a very large table (such as one with hundreds of thousands or millions of rows) and don't use a limit, you could end up waiting a long time for all of your results to be displayed, which doesn't make sense if you only care about the first few.

## [](https://mode.com/sql-tutorial/sql-limit#using-the-sql-limit-command)Using the SQL LIMIT command

The limiting functionality is built into Mode to prevent you from accidentally returning millions of rows without meaning to (we've all done it). However, if you're ever using SQL outside of Mode, you can manually add a limit with a SQL command. The following syntax does the same thing as having the box checked with a value of 100:

```
SELECT *
  FROM tutorial.us_housing_units
 LIMIT 100
```
## Practice Problem

Write a query that uses the `LIMIT` command to restrict the result set to only 15 rows.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.us_housing_units
 LIMIT 15
```