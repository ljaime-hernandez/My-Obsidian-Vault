lik
`LIKE` is a [logical operator](https://mode.com/sql-tutorial/sql-logical-operators) in SQL that allows you to match on similar values rather than exact ones.

In this example, the results from the [Billboard Music Charts dataset](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset) will include rows for which `"group_name"` starts with "Snoop" and is followed by any number and selection of characters.

Run the code to see which results are returned.

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "group_name" LIKE 'Snoop%'
```

## [](https://mode.com/sql-tutorial/sql-like#wildcards-and-ilike)Wildcards and ILIKE

The `%` used above represents any character or set of characters. In this case, `%` is referred to as a "wildcard." In the type of SQL that Mode uses, `LIKE` is case-sensitive, meaning that the above query will only capture matches that start with a capital "S" and lower-case "noop." To ignore case when you're matching values, you can use the `ILIKE` command:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "group_name" ILIKE 'snoop%'
```

You can also use `_` (a single underscore) to substitute for an individual character:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE artist ILIKE 'dr_ke'
```

## Practice Problem

Write a query that returns all rows for which Ludacris was a member of the group.

ANSWER
```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 WHERE "group" ilike '%ludacris%'
```

## Practice Problem

Write a query that returns all rows for which the first artist listed in the group has a name that begins with "DJ".

ANSWER
```
SELECT * 
FROM tutorial.billboard_top_100_year_end
WHERE group_name ILIKE 'DJ%'
```
