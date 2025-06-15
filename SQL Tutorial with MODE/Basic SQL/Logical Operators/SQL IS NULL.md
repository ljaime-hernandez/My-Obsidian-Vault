`IS NULL` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to exclude rows with missing data from your results.

Some tables contain null values—cells with no data in them at all. This can be confusing for heavy Excel users, because the difference between a cell having no data and a cell containing a space isn't meaningful in Excel. In SQL, the implications can be pretty serious. This is covered in greater detail in the [intermediate tutorial](https://mode.com/sql-tutorial/sql-aggregate-functions), but for now, here's what you need to know:

You can select rows that contain no data in a given column by using `IS NULL`. Let's try it out using a [dataset from the Billboard Music Charts](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset).

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist IS NULL
```

`WHERE artist = NULL` will **not** work—you can't perform arithmetic on null values.

### [](https://mode.com/sql-tutorial/sql-is-null#sharpen-your-sql-skills)Sharpen your SQL skills




## Practice Problem

Write a query that shows all of the rows for which `song_name` is null.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE song_name IS NULL
```