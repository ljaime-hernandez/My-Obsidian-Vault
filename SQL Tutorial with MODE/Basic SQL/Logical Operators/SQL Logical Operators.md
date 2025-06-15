In the [previous lesson](https://mode.com/sql-tutorial/sql-operators), you played with some comparison operators to filter data. You’ll likely also want to filter data using several conditions—possibly more often than you'll want to filter by only one condition. Logical operators allow you to use multiple comparison operators in one query.

Each logical operator is a special snowflake, so we'll go through them individually in the following lessons. Here's a quick preview:

- [`LIKE`](https://mode.com/sql-tutorial/sql-like) allows you to match similar values, instead of exact values.
- [`IN`](https://mode.com/sql-tutorial/sql-in-operator) allows you to specify a list of values you'd like to include.
- [`BETWEEN`](https://mode.com/sql-tutorial/sql-between) allows you to select only rows within a certain range.
- [`IS NULL`](https://mode.com/sql-tutorial/sql-is-null) allows you to select rows that contain no data in a given column.
- [`AND`](https://mode.com/sql-tutorial/sql-and-operator) allows you to select only rows that satisfy two conditions.
- [`OR`](https://mode.com/sql-tutorial/sql-or-operator) allows you to select rows that satisfy either of two conditions.
- [`NOT`](https://mode.com/sql-tutorial/sql-not-operator) allows you to select rows that do not match a certain condition.

## [](https://mode.com/sql-tutorial/sql-logical-operators#about-this-dataset)About this dataset

To practice logical operators in SQL, you'll be using data from [Billboard Music Charts](http://www.billboard.com/charts). It was collected in January 2014 and contains data from 1956 through 2013. The results in this table are the _year-end_ results—the top 100 songs at the end of each year.

To access the dataset, use the following query:

```
SELECT * FROM tutorial.billboard_top_100_year_end
```

- `year_rank` is the rank of that song at the end of the listed year.
- `group_name` is the name of the entire group that won (this could be multiple artists if there was a collaboration).
- `artist` is an individual artist. This is a little complicated, as an artist can be an individual or group.

You can get a better sense of some of the nuances of this dataset by running the query below. It uses the [`ORDER BY`](https://mode.com/sql-tutorial/sql-order-by) clause, which you'll learn about in a later lesson. Don't worry about it for now:

```
SELECT *
  FROM tutorial.billboard_top_100_year_end
 ORDER BY year DESC, year_rank
```

You'll notice that Macklemore does a lot of collaborations. Since his songs are listed as featuring other artists like Ryan Lewis, there are multiple lines in the dataset for Ryan Lewis. Daft Punk and Pharrell Williams are also listed as two artists. Daft Punk is actually a duo, but since the album lists them together under the name Daft Punk, that's how Billboard treats them.