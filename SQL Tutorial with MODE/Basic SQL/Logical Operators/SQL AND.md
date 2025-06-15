`AND` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to select only rows that satisfy two conditions. Using [data from the Billboard Music Charts](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset), the following query will return all rows for top-10 recordings in 2012.

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 2012 AND year_rank <= 10
```

You can use SQL's `AND` operator with additional `AND` statements or any other comparison operator, as many times as you want. If you run this query, you'll notice that all of the requirements are satisfied.

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 2012
   AND year_rank <= 10
   AND "group_name" ILIKE '%feat%'
```

You can see that this example is spaced out onto multiple lines—a good way to make long `WHERE` clauses more readable.




### [](https://mode.com/sql-tutorial/sql-and-operator#sharpen-your-sql-skills)Sharpen your SQL skills

## Practice Problem

Write a query that surfaces all rows for top-10 hits for which Ludacris is part of the Group.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank <= 10
   AND "group_name" ILIKE '%ludacris%'
```
## Practice Problem

Write a query that surfaces the top-ranked records in 1990, 2000, and 2010.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank = 1
   AND "year" IN ('1990', '2000', '2010')
```
## Practice Problem

Write a query that lists all songs from the 1960s with "love" in the title.

[Try it out](https://app.mode.com/editor/reports/new) [See the answer](https://app.mode.com/tutorial/reports/11c78511873a/queries/521b92fbdd85)

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "year" BETWEEN 1960 AND 1969
   AND "song_name" ILIKE ('%love%')
```