## Basic syntax: SELECT and FROM

There are two required ingredients in any SQL query: `SELECT` and `FROM`—and they have to be in that order. `SELECT` indicates which columns you'd like to view, and `FROM` identifies the table that they live in.

Let's start by looking at a couple columns from the [housing unit table](https://mode.com/sql-tutorial/sql-in-mode#about-this-dataset):

```
SELECT year,
       month,
       west
  FROM tutorial.us_housing_units
```

To see the results yourself, copy and paste this query into [Mode's Query Editor](https://mode.com/sql-tutorial/sql-in-mode) and run the code. If you already have SQL code in the Query Editor, you'll need to paste over or delete the query that was there previously. If you simply copy and paste this query below the previous one, you'll get an error—you can only run one `SELECT` statement at a time.

So what's happening in the above query? In this case, the query is telling the database to return the `year`, `month`, and `west` columns from the table `tutorial.us_housing_units`. (Remember that when referencing tables, the table names have to be preceded by [the name of user who uploaded it](https://mode.com/sql-tutorial/introduction-to-sql).) When you run this query, you'll get back a set of results that shows values in each of these columns.

[![](https://mode.com/resources/images/the-basics/prelim-results.png)](https://mode.com/resources/images/the-basics/prelim-results.png "The results of your first query")
Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of the column names:

```
SELECT *
  FROM tutorial.us_housing_units
```

[![](https://mode.com/resources/images/the-basics/results.png)](https://mode.com/resources/images/the-basics/results.png "The results of a query using SELECT *")
## Practice Problem

Write a query to select all of the columns in the `tutorial.us_housing_units` table without using `*`.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT year, month, month_name, south, west, midwest, northeast
  FROM tutorial.us_housing_units
```

When you've completed the above practice problem, check your answer by clicking "See the answer." Following the link will show you our solution SQL query. To see the results produced by this solution query, click "Data" in the left sidebar:

This will show you a table of query results that should be the same as your query results (if your answer is correct):

[![](https://mode.com/resources/images/the-basics/query-results.png)](https://mode.com/resources/images/the-basics/query-results.png "View Query Results in Mode")  

To compare your query or results with our solution, jump back to the window where you're editing your practice solutions. There's lots to explore in the Editor (see ["How to use the query editor"](https://mode.com/help/articles/querying-data/#query-editor) to learn more), but to start you might want to experiment with creating a chart using our drag-and-drop chart builder--just click on the green plus button next to the "Display Table" tab:

[![](https://mode.com/resources/images/the-basics/add-chart.png)](https://mode.com/resources/images/the-basics/view-details.png "Add Chart in Mode")  

This will take you to Mode's drag-and-drop chart builder. For more about building charts in Mode, check out ["How to build charts."](https://mode.com/help/articles/visualizations/#quick-charts)

If you're feeling particularly proud of your work, you might want to explore how it looks in Mode's Report View—a cleaned-up view meant for sharing queries and results. Just click on "View" in the header:

[![](https://mode.com/resources/images/the-basics/view-report.png)](https://mode.com/resources/images/the-basics/view-details.png "How to View Reports in Mode")  

Now you'll be looking at a cleaned-up version of your report fit for sharing. You can learn more about viewing and building reports on [Mode's help site](https://mode.com/help/articles/organizing-reports/). For now, the most important thing to know is that you can share this report with anyone by clicking the "Share" menu in the Query Editor and selecting the channel you'd like to use for sharing:

[![](https://mode.com/resources/images/the-basics/how-to-share.png)](https://mode.com/resources/images/the-basics/how-to-share.png "Sharing in Mode")[![](https://mode.com/resources/images/the-basics/how-to-share-2.png)](https://mode.com/resources/images/the-basics/how-to-share-2.png "Sharing in Mode")

_Send to all your friends by email or Slack!_

You can also share your work in progress from the Editor view, where you've been writing your queries. To get back to editing your query, click on "Edit" in the header bar:

[![](https://mode.com/resources/images/the-basics/to-editor.png)](https://mode.com/resources/images/the-basics/title-description.png "How to get from Report View to Editor")

You'll land back in the Query Editor, where you can edit your SQL, your charts, or your reports.

## [](https://mode.com/sql-tutorial/sql-select-statement#what-actually-happens-when-you-run-a-query)What actually happens when you run a query?

Let's get back to it! When you run a query, what do you get back? As you can see from running the queries above, you get a table. But that table isn't stored permanently in the database. It also doesn't change any tables in the database—`tutorial.us_housing_units` will contain the same data every time you query it, and the data will never change no matter how many times you query it. Mode does store all of your results for future access, but `SELECT` statements don't change anything in the underlying tables.

## [](https://mode.com/sql-tutorial/sql-select-statement#formatting-convention)Formatting convention

You might have noticed that the `SELECT` and `FROM' commands are capitalized. This isn't actually necessary, SQL will understand these commands if you type them in lowercase. Capitalizing commands is simply a convention that makes queries easier to read. Similarly, SQL treats one space, multiple spaces, or a line break as being the same thing. For example, SQL treats this the same way it does the previous query:

```
SELECT *        FROM tutorial.us_housing_units
```

It also treats this the same way:

```
SELECT *
  FROM tutorial.us_housing_units
```

While most capitalization conventions are the same, there are several conventions for formatting line breaks. You'll pick up on several of these in this tutorial and in other people's work on Mode. It's up to you to determine what formatting method is easiest for you to read and understand.

## [](https://mode.com/sql-tutorial/sql-select-statement#column-names)Column names

While we're on the topic of formatting, it's worth noting the format of column names. All of the columns in the `tutorial.us_housing_units` table are named in lower case, and use underscores instead of spaces. The table name itself also uses underscores instead of spaces. Most people avoid putting spaces in column names because it's annoying to deal with spaces in SQL—if you want to have spaces in column names, you need to always refer to those columns in double quotes.

If you'd like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the `west` column to appear as `West Region` in the results, you would have to type:

```
SELECT west AS "West Region"
  FROM tutorial.us_housing_units
```

Without the double quotes, that query would read 'West' and 'Region' as separate objects and would return an error. Note that the results will only return capital letters if you put column names in double quotes. The following query, for example, will return results with lower-case column names.

```
SELECT west AS West_Region,
       south AS South_Region
  FROM tutorial.us_housing_units
```


## Practice Problem

Write a query to select all of the columns in `tutorial.us_housing_units` and rename them so that their first letters are capitalized.

[Try it out](https://app.mode.com/editor/reports/new) 

ANSWER
```
SELECT  year as "Year", 
		month as "Month", 
		month_name as "Month Name", 
		south as "South", 
		west as "West", 
		midwest as "Midwest", 
		northeast as "Northeast"
  FROM tutorial.us_housing_units
```

### [](https://mode.com/sql-tutorial/sql-select-statement#find-the-report-but-not-the-sql)Find the report but not the SQL?

Once you've got the report open, see the SQL powering it by clicking on "View Details" at the top, then clicking "SQL" in the sidebar on the left.