


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
## Predicate Logic
ALL, ANY, BETWEEN, IN, LIKE, OR, SOME

## SET operator
union
union all (duplicates)
intersect (common)
except (data present in 1 st query not in second)

## Trigger
A trigger is a database object that runs automatically when an event occurs. There are three different types of events.

DML Events
DDL Events
LOGON Event – Logon trigger is fired when a LOGON event occurs i.e. when a user session is being established


##  Memory Optimized tables
Views and materialized views are two ways of creating virtual tables in SQL that can simplify queries and enhance performance.
 But what are the differences between them and how do you choose the best option for your database development project?
 In this article, you will learn the basics of views and materialized views, how they work, and when to use them.

 A view in SQL is a named query that returns a subset or a combination of data from one or more tables or views.
 You can think of a view as a virtual table that does not store any data, but only references the underlying tables or views.
 You can create a view using the CREATE VIEW statement, and then use it like a regular table in your queries. 


 How do views and materialized views differ?
The main difference between views and materialized views is that views are dynamic and materialized views are static.
This means that views always reflect the latest data from the underlying tables or views, 
while materialized views only show the data from the last refresh. Therefore, views are more suitable for queries that need real-time data, while materialized views are more suitable for queries that need precomputed data.

Another difference is that views are more lightweight and flexible, while materialized views are more resource-intensive and rigid. This means that views do not take up any storage space or require any maintenance, while materialized views do. However, views also depend on the availability and performance of the underlying tables or views, while materialized views do not. Therefore, views are more convenient for creating temporary or ad hoc queries, while materialized views are more reliable for creating permanent or recurring queries.


What are Memory Optimized Tables?

A Memory Optimized Table, starting in SQL Server 2014, is simply a table that has two copies one in active memory and one durable on disk whether that includes data or just Schema Only, which I will explain later. Since memory is flushed upon restart of SQL Services, SQL Server keeps a physical copy of the table that is recoverable. Even though there are two copies of the table, the memory copy is completely transparent and hidden to you.

What is the added benefit for using these in-memory tables?

That’s always something I ask when looking at SQL Server options or features. For in-memory tables, it’s the way SQL Server handles the latches and locks. According to Microsoft, the engine uses an optimistic approach for this, meaning it does not place locks or latches on any version of updated rows of data, which is very different than normal tables. It’s this mechanism that reduces contention and allows the transactions to process exponentially faster. Instead of locks In-Memory uses Row Versions, keeping the original row until after the transaction is committed. Much like Read Committed Snapshot Isolation (RCSI) this allows other transactions to read the original row, while updating the new row version. The In-Memory structured version is “pageless” and optimized for speed inside active memory,  giving a significant performance impact depending on workloads.

SQL Server also changes it’s logging for these tables. Instead of fully logging, this duality of both on disk and in memory versions (row versions) of the table allows less to be logged. SQL Server can use the before and after versions to gain information it would normally acquire from a log file. In SQL Server 2019, the same concept applies to the new Accelerated Data Recovery (ADR) approach to logging and recovery.

Finally, another added benefit is the DURABILITY option shown in the below example. The use of SCHEMA_ONLY can be a great way to get around the use of # TEMP tables and add a more efficient way to process temporary data especially with larger tables. You can read more on that here.

Things to Consider

Now this all sounds great, you would think everyone would add this to all their tables, however like all SQL Server options this is not for all environments. There are things you need to consider before implementing In Memory Tables. First and foremost, the amount of memory and the configuration of that memory before considering this. You MUST have that setup correctly in SQL Server as well adjust for the increased use of memory which may mean adding more memory to your server before starting. Secondly know that, like Columnstore indexes, these tables are not applicable for everything. These table are optimized for high volume WRITEs,  not a data warehouse which is mostly for reads for example.

Let’s see how we create one

The key to having a table “In-Memory” is the use of the key word “MEMORY-OPTIMIZED” on the create statement when you first create the table. Note there is no ability to ALTER a table to make an existing one memory optimized, you will need to recreate the table and load the data in order to take advantage of this option on an existing table.  There’s just a couple more setting you need to have configured to make this work as you can see from below.

First step is to make sure you are on compatibility level >=130
SELECT d.compatibility_level
    FROM sys.databases as d
    WHERE d.name = Db_Name();

If you are not you will need to change it.
ALTER DATABASE AdventureWorks2016CTP3; 
SET COMPATIBILITY_LEVEL = 130;

Next you must alter your database in order to take advantage of In- Memory you must alter your database and enable this setting.
ALTER DATABASE AdventureWorks2016CTP3; 
SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;

Lastly your database will need to have a memory optimized file group added.
ALTER DATABASE AdventureWorks2016CTP3 
ADD FILEGROUP AdventureWorks2016CTP3_mod CONTAINS MEMORY_OPTIMIZED_DATA;

The below actually creates the file into the new filegroup.
ALTER DATABASE AdventureWorks2016CTP3 
ADD FILE (name=' AdventureWorks2016CTP3_mod1', 
filename='c:\data\AdventureWorks2016CTP3) 
TO FILEGROUP AdventureWorks2016CTP3_mod

Now let’s create a table
USE AdventureWorks2016CTP3
CREATE TABLE dbo.InMemoryExample
    (
        OrderID   INTEGER   NOT NULL   IDENTITY
            PRIMARY KEY NONCLUSTERED,
        ItemNumber   INTEGER    NOT NULL,
        OrderDate    DATETIME   NOT NULL
    )
        WITH
            (MEMORY_OPTIMIZED = ON,
            DURABILITY = SCHEMA_AND_DATA)
## Constraint
check constraint
            CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
## rownumber vs dense_rank vs rank

![image](https://github.com/user-attachments/assets/d1c3fcf9-8daa-4c9e-bbb6-def29afd4e5d)

