


### Understanding the Logical Order of Operations in SELECT statements
1. FROM
        Logically, it makes sense to fetch the data first before actually filtering or calculating anything.
         You may also notice some SQL tools attempt to auto-complete the field name when you type them, but only after you write FROM clause.
2. JOIN
        Join is actually an operator of FROM, and it makes sense to create the full table before working on it.
3. WHERE
        This works as a high level rows filter by the condition you specify, which can reduce the amount of rows that needs to be processed (and hence less memory usage).
4. GROUP BY
        Now this is where it’s starting to get spicy. As the name implies, GROUP BY virtually divides the rows into groups/buckets, which then allows for aggregation by those groups. In plain English, it reads like “For each group, what is the aggregation of their values?” which brings us to the next point…
5. Aggregation
      The magic happens here specifically. This is why we cannot have aggregation in the WHERE clause, because WHERE happened 2 steps above! And this is also why we can only filter using aggregation in our next step; HAVING
6. HAVING
      As mentioned above, we can now start filtering the dataset using the aggregation, because technically all the aggregation already happened above!
7. SELECT
      Now after handling all the calculations and filtering, the engine finally produces the results. We have access to all the grouping and aggregations, hence creating a new field with aggregation (such as AVG([Profit]) and Count) works here.
      Note that you cannot create an alias here then refer to the same alias in the same SELECT query (e.g., SELECT column1 as ‘Alias’, SUM([Alias]) ….) because SELECT runs as a batch, so the alias has not been created yet in this run. In fact, you can only use the alias created from SELECT query in ORDER BY (2 steps below).
      On the other hand, aliases created for table names from the FROM and JOIN queries can work here because they occurred at the beginning of the whole process. For instance, FROM Table1 AS ‘Alias2’ can be used in SELECT Alias2.ColumnName.
8. DISTINCT
      Now that the engine knows the full rows and columns of interest, it can start dropping duplicates.
9. ORDER BY
      And after dropping the duplicates and having the final table, the engine can finally start the ranking.
10. LIMIT or TOP
