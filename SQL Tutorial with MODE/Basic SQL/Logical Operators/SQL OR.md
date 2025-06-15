`OR` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to select rows that satisfy either of two conditions. It works the same way as `AND`, which selects the rows that satisfy both of two conditions. Try `OR` out by running this query against [data from the Billboard Music Charts](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset):

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year_rank = 5 OR artist = 'Gotye'
```

You'll notice that each row will satisfy one of the two conditions. You can combine `AND` with `OR` using parenthesis. The following query will return rows that satisfy **both** of the following conditions:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE year = 2013
   AND ("group_name" ILIKE '%macklemore%' OR "group_name" ILIKE '%timberlake%')
```

You will notice that the conditional statement `year = 2013` will be fulfilled for every row returned. In this case, `OR` is treated like one separate conditional statement because it's in parentheses, so it must be satisfied in addition to the first statement of `year = 2013`. You can think of the rows selected as being either of the following:

- Rows where `year = 2013` is true and `"group_name" ILIKE '%macklemore%'` is true
- Rows where `year = 2013` is true and `"group_name" ILIKE '%timberlake%'` is true
- Rows where `year = 2013` is true and `"group_name" ILIKE '%macklemore%'` is true and `"group_name" ILIKE '%timberlake%'` is true

### [](https://mode.com/sql-tutorial/sql-or-operator#sharpen-your-sql-skills)Sharpen your SQL skills

## Practice Problem

Write a query that returns all rows for top-10 songs that featured either Katy Perry or Bon Jovi.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year_rank <= 10
       AND ("group" ILIKE '%katy perry%' OR "group" ILIKE '%bon jovi%')
```

## Practice Problem

Write a query that returns all songs with titles that contain the word "California" in either the 1970s or 1990s.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE song_name LIKE '%California%'
   AND (year BETWEEN 1970 AND 1979 OR year BETWEEN 1990 AND 1999)
```

## Practice Problem

Write a query that lists all top-100 recordings that feature Dr. Dre before 2001 or after 2009.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "group" ILIKE '%dr. dre%'
   AND (year <= 2000 OR year >= 2010)
```