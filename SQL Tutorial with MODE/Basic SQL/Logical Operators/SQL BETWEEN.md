`BETWEEN` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to select only rows that are within a specific range. It has to be paired with the [`AND`](https://mode.com/sql-tutorial/sql-logical-operators) operator, which you'll learn about in a later lesson. Here's what `BETWEEN` looks like on a [Billboard Music Chart Dataset](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset):

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank BETWEEN 5 AND 10
```

`BETWEEN` includes the range bounds (in this case, 5 and 10) that you specify in the query, in addition to the values between them. So the above query will return the exact same results as the following query:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank >= 5 AND year_rank <= 10
```

Some people prefer the latter example because it more explicitly shows what the query is doing (it's easy to forget whether or not `BETWEEN` includes the range bounds).

### [](https://mode.com/sql-tutorial/sql-between#sharpen-your-sql-skills)Sharpen your SQL skills




## Practice Problem

Write a query that shows all top 100 songs from January 1, 1985 through December 31, 1990.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year >= 1985 AND year <= 1990
```