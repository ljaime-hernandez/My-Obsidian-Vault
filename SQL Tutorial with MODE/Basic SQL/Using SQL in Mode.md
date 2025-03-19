## Opening Mode and getting started

Open a second browser window to [Mode](https://mode.com/) and log in. If you don't already have a Mode account, [create one here](https://app.mode.com/signup?utm_source=community&utm_medium=tutorial&utm_campaign=sqltutorial). You'll arrive at your homepage. You can use the homepage to quickly access a project you were working on previously. Since you probably haven't used Mode yet, click New Report to get started.

[![](https://mode.com/resources/images/the-basics/new-query.png)](https://mode.com/resources/images/the-basics/new-query.png "Start a session by creating a new query")

This will take you to the Query Editor. This is the bread and butter of Mode—it's where you'll be able to use all the skills you learn in this tutorial. From the Query Editor, you can run queries against all of the data in Mode.

For this tutorial, SQL queries will be shown in light blue boxes like the one below. To run a query, simply copy the text from the box into the Query Editor and click the "Run" button. Alternatively, you can run a query by pressing `⌘` + `return` on a Mac or `ctrl` + `return` on a PC.

```
SELECT *
  FROM tutorial.us_housing_units
```

Give this a shot, then keep reading to learn what the query is doing.

[![](https://mode.com/resources/images/the-basics/run-button.png)](https://mode.com/resources/images/the-basics/run-button.png "Run your first query")

It takes a few seconds for the query to run. When it's done, you'll see the results show up in a table below the query window.

## [](https://mode.com/sql-tutorial/sql-in-mode#about-this-dataset)About this dataset

To start out, you'll be working with real data from the U.S. Census. This dataset shows the number of completed housing units in major regions of the United States. The table you'll be working with has a column for each region. The values in each row represent the number of housing units completed in thousands during the corresponding month. The data was collected in August 2014 and can be accessed at [the U.S. Census website](http://www.census.gov/econ/currentdata/).