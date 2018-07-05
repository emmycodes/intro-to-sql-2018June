x
Download Data: [Click here](https://github.com/emmycodes/intro-to-sql-2018June/blob/master/data/donorschoose.sqlite)


:note:

Speaker notes start with the line as above

Separate slides with a blank line, then 3 hyphens, then another blank line, as in the line below.

---

# Intro to SQL

#### Emily Supil [@dailysup](https://twitter.com/dailysup)

![GDI Circle Logo](./gdi/img/circle-gdi-logo.png "GDI Circle Logo")

Based on content by [Vicki Boykis](https://twitter.com/vboykis) and [a.e. Lavender](https://twitter.com/__metaclass__)

---

## Welcome!

Girl Develop It is here to provide affordable and accessible programs to learn software through mentorship and hands-on instruction.

[Our code of conduct](https://www.girldevelopit.com/code-of-conduct)

Some rules:
- We are here for you!
- Every question is important
- Help each other
- Have fun

---

## Tell us about yourself.
- Who are you?
- What's your experience level with SQL?
- What do you hope to get out of the class?
- How many other GDI classes have you taken?
- What is your favorite movie, and how many times have you watched it?

---

## About me
- Business Intelligence Developer & Analyst at [IBM](http://www.ibm.com)
- 5 years of experience with SQL
- Favourite movie is Elf. I've seen it at least 3x/year.

![Elf](./img/elf_pic.jpg "Elf")

---

## What to expect
- This is a complement to Sondre's database design class
  - You don't have to have taken that class in order to make use of SQL
  - Same terminology
  - Less theory, more practice

---

## What to expect
- Goals: By the end of this class you will be able to:
  - Query, filter, group, and aggregate data from a database table
  - Join multiple tables together to take advantage of relationships between tables
  - Add, update, and remove data from a database

---

## Plan for the day
- What's a database? What's SQL?
- Getting data out of a database (about 3/4 of the class):
  - `SELECT ... FROM ...`
  - Selecting distinct and counting elements
  - Filtering: `WHERE`
  - Ordering: `ORDER BY`
  - Aggregation functions
  - Grouping: `GROUP BY`
- Joining multiple tables together: `JOIN`

---

## What's a Relational Database?
- A relational database management system (RDBMS) is a way of storing data using the "relational data model"
  - Data is stored in tables
  - Tables have well-defined relationships between them
- The most common type of database today; when people just say "database," they almost always mean "relational database."
- Examples: MySQL, Postgres, SQLite, Microsoft SQL Server, IBM DB2 SQL, Oracle

---

## Terminology
- *Database System*: The hardware and software hosting the data
  - Typically runs on a server
  - A database system hosts one or more schemata.
- *Schema*: The layout of your data:
  - The names and structures of your tables and the relationships between them
  - A schema holds one or more tables.

---

## Terminology
- *Table*: A collection of records that all have the same structure
  - A table has many rows and many columns.
- *Column*: The structure of your table
  - Each column has a _name_ and a _type_
- *Row*: A single record of data: a singe "object" or "element"
  - Each row has one value for each column in the table.
- *Value*: A single piece of data (number, string, date, etc)

---

## Compared to Excel

| Database | Excel     |
| -------- | --------- |
| Schema   | Workbook  |
| Table    | Worksheet |
| Row      | Row       |
| Column   | Column    |
| Value    | Cell      |

![Russian Nesting Dolls](./img/small-nested-dolls.png "Russian Nesting Dolls")

---

## Differences from Excel
- Database columns have *names* (e.g. `UserName`) instead of letters (A, B, C, ...)
- Database columns have *types* (numbers, strings, dates)
- Database rows have *Primary Keys* instead of numbers (1, 2, 3, ...)
- Database tables hold just data (values), not formulas
  - Use SQL instead of formulas!
- Database tables can be *joined*
  - like `vlookup` but more powerful!

---

## What's SQL?
- Stands for _Structured Query Language_
- How to ask ("Query") a database for information
- A _declarative_ language: ask for what you want; the database figures out how to get it.
- Nobody pronounces it correctly
- Each database system uses a slightly different "flavour" of SQL

---

## The "Structure" in SQL:

Example of SQL Select statement:

```sql
SELECT UserName, SUM(PurchaseAmount) AS Total
FROM billing_database.purchases
WHERE PurchaseDate >= '2018-01-01'
  AND ProductCategory = 'books'
  AND UserName LIKE 'Ada%'
GROUP BY UserName
HAVING Total > 50.00
ORDER BY Total DESC
LIMIT 100;
```

What do you think this statement does?

---

## The Select Statement

#### Use these keywords, in this order:

- `SELECT`: What data (columns) do you want? (required)
- `FROM`: Which table(s) do you want it from? (required)
- `WHERE`: Filter which rows to return
- `GROUP BY`: Groups rows together
- `HAVING`: Filter which groups to return
- `ORDER BY`: Sort results
- `LIMIT`: How many results to return? (MySQL-specific!)

---

## Let's get connected (locally)!

:note:

Walk through connecting via DB Browser for SQLite.  

---

## About our data

This is **real data** about [DonorsChoose](http://donorschoose.org) from [Kaggle](https://kaggle.com)!

- DonorsChoose.org is an online platform to raise money for school classrooms!
- People donate to projects that teachers create to enhance the classroom experience.
- Requests include books, technology, supplies, field trips & more!
- Teachers at **79%** of US's publich schools have posted a project on the site.
- **3M+ people** have donated **$702M+** for classroom projects impacting **28M+** students.

---

## Our very first SQL statement!

```sql
SELECT * FROM PROJECTS;
```

:note:

At this point, switch to Workbench and do it interactively.

What does this mean?
 - `SELECT *` means "give me everything"
 - `FROM [donors_choose].Users` is where it's from.  Use a two-part name: the schema name and the table name.
- For SQLite, the schema is not typed out, but is typically included in enterprise systems.

Is SQL case sensitive?  It depends on what kinda SQL.
  - Keywords are usually NOT case sensitive
  - Table and column names usually ARE case sensitive
  - Data can go either way...

---

#### Let's try a few modifications...

```sql
SELECT * FROM PROJECTS
LIMIT 10;
```
<!-- .element: class="fragment" -->

```sql
SELECT Title, Grade_Level_Catg, Cost
FROM PROJECTS;
```
<!-- .element: class="fragment" -->

```sql
SELECT Title, Grade_Level_Catg, Cost
FROM PROJECTS
LIMIT 10;
```
<!-- .element: class="fragment" -->

```sql
SELECT COUNT(*) FROM PROJECTS;
```
<!-- .element: class="fragment" -->

---

#### Let's take some time to explore some of the other tables...
```sql
SELECT * FROM SCHOOLS LIMIT 10;

SELECT * FROM DONATIONS LIMIT 10;

SELECT * FROM RESOURCES LIMIT 10;
```
This is a great way to explore a new dataset and see what kind of data you've got.

---

#### We can already do some useful things!

```sql
-- How many teachers are listed?
SELECT COUNT(*) FROM TEACHERS;

-- How many schools have had a project?
SELECT COUNT(*) FROM SCHOOLS;

-- This is how you write a comment in SQL, btw...
```

---

## Stretch Break!

![Stretch!](./gdi/img/stretch_dog.jpg "Stretch!")

:note:

After the stretch break, briefly mention the following:
- Specifying the schema (stackoverflow) in front of every table name is not necessary but good practice
- The semi-colon is not necessary if you only have one statement but useful to separate multiple statements

---

## Filtering the `WHERE` clause: Numeric

What if we only want to see donations greater than $1,000?

```sql
SELECT * FROM DONATIONS
WHERE DONATE_AMT > 1000;
```

---

## Filtering the `WHERE` clause: Text (String)

What if we only want to see projects for grades 3 - 5?

```sql
SELECT * FROM PROJECTS 
WHERE GRADE_LEVEL_CATG = 'Grades 3-5';
```

---

#### Try answering these questions!

- Query a list of all the donors who are teachers 
- What is the zip code of 'Canyon Oaks Elementary School'?
- Get the list of donors in Philly
- Get a list of resources from vendor 'Lakeshore Learning Materials'?
- _How many_ schools have a free lunch % of 40%?  75%?
- _How many_ projects are 'Teacher-Led'?
- How many donors do NOT have a city listed?

**If you're not sure if something will work, try it and see! Don't worry if you can't get all of these!**

---

Query a list of all the donors who are teachers 
```sql
SELECT * FROM DONORS 
WHERE IS_TEACHER = 'Yes';
```

---

What is the zip code of `Canyon Oaks Elementary School`?
```sql
SELECT * FROM SCHOOLS
WHERE NAME = 'Canyon Oaks Elementary School';
```
Note that strings must be `'quoted like this'`

---

Get the list of donors in Philly
```sql
SELECT * FROM DONORS 
WHERE CITY = 'Philadelphia' AND STATE = 'Pennsylvania';
```
Teaser Feature :)

---

Get a list of resources from vendor 'Lakeshore Learning Materials'?
```sql
SELECT * FROM RESOURCES
WHERE VENDER_NAME = 'Lakeshore Learning Materials';
```

---

#### How many_schools have a free lunch % of 40%?

```sql
SELECT count(*) FROM SCHOOLS
WHERE FREE_LUNCH_PCT >= 40;
```

#### 75% or greater?

```sql
SELECT count(*) FROM SCHOOLS
WHERE FREE_LUNCH_PCT >= 75;
```

---

#### How many projects are 'Teacher-Led'?

```sql
SELECT count(*) FROM PROJECTS
WHERE TYPE = 'Teacher-Led';
```

---

#### How many donors do NOT have a city listed?

```sql
SELECT count(*) FROM DONORS
WHERE CITY IS NULL;
```

- `NULL` is a special value in SQL
- You can filter by `IS NULL` or `IS NOT NULL`

---

#### WHERE CLAUSE: Multiple Conditions (AND/OR)

How many donations were more than $100 AND did optional donation?

```sql
SELECT count(*) FROM DONATIONS
WHERE OPTIONAL_DONATE = 'Yes' AND DONATE_AMT > 100;
```

How many donations were more than $100 OR did optional donation?
```sql
SELECT count(*) FROM DONATIONS
WHERE OPTIONAL_DONATE = 'Yes' OR DONATE_AMT > 100;
```

---

#### INTRODUCING LIKE! 

What if we wanted to find out resources with books?

```sql
SELECT * FROM RESOURCES 
WHERE ITEM_NAME = 'Books';
```
```sql
SELECT * FROM RESOURCES 
WHERE ITEM_NAME = 'Books' OR ITEM_NAME = 'Book';
```
Both produce zero rows.

But what if we did this instead...
<!-- .element: class="fragment" data-fragment-index="1"-->


```sql
SELECT * FROM RESOURCES 
WHERE ITEM_NAME LIKE '%Book%';
```
<!-- .element: class="fragment" data-fragment-index="1"-->


<!-- .element: class="fragment"-->`LIKE` is a pattern-matching operator.  The `%` character is a wildcard character -- it matches anything.

---

## More with `LIKE`

Which projects have 'tech' in their need statement?

```sql
SELECT * FROM PROJECTS
WHERE NEED_STATEMENT LIKE '%tech%';
```
<!-- .element: class="fragment"-->

<!-- .element: class="fragment"-->The `%` can go anywhere in the string, and you can have more than one of them.

---

## Stretch Break!

![Stretch!](./gdi/img/stretch_dog.jpg "Stretch!")

---

## Putting things in `ORDER`:

```sql
SELECT * FROM PROJECTS
ORDER BY COST;
```

---

The 20 teachers with the earliest project create date: 

```sql
SELECT * FROM TEACHERS
ORDER BY FIRST_PROJ_CREATE_DATE
LIMIT 20;
```

The 10 latest posted projects:

```sql
SELECT * FROM PROJECTS
ORDER BY POST_DATE DESC
LIMIT 10;
```

---

You can `ORDER BY` multiple columns, in different directions:
```sql
SELECT * FROM PROJECTS
ORDER BY COST ASC, FUNDED_DATE DESC;
```

---

## Aggregation functions

Aggregation functions collapse and summarise your data.

You've already seen one aggregation function, `COUNT`:
```sql
SELECT COUNT(*) FROM DONATIONS;
```

If you give it a column name, it can do even more:
<!-- .element: class="fragment" -->

```sql
SELECT COUNT(ID) FROM DONATIONS;
```
<!-- .element: class="fragment" -->

```sql
SELECT COUNT(DISTINCT PROJECT_ID)
FROM DONATIONS;
```
<!-- .element: class="fragment" -->

You can combine those last two queries:
<!-- .element: class="fragment" -->

```sql
SELECT COUNT(ID), COUNT(DISTINCT PROJECT_ID)
FROM DONATIONS;
```
<!-- .element: class="fragment" -->

---

#### There are more aggregation functions:

Minumum, Maximum, Sum, Average

```sql
SELECT
MIN(DONATE_AMT),
MAX(DONATE_AMT),
SUM(DONATE_AMT),
AVG(DONATE_AMT)
FROM DONATIONS
```

And we can use `WHERE` as well:

```sql
SELECT
MIN(DONATE_AMT),
MAX(DONATE_AMT),
SUM(DONATE_AMT),
AVG(DONATE_AMT)
FROM DONATIONS
WHERE DONATE_DATE >= '9/9/15'
```

---

## Aggregations are more fun with `GROUP BY`

What's the difference between those who donate optionally vs those who do not?

```sql
SELECT OPTIONAL_DONATE, 
COUNT(*) as "DONATE_COUNT", 
SUM(DONATE_AMT) as "DONATE_TOTAL", 
AVG(DONATE_AMT) as "DONATE_AVG"
FROM DONATIONS
GROUP BY OPTIONAL_DONATE;
```
<!-- .element: class="fragment" -->

---

#### Try these:
- Based on the Project type, what is the total and avg costs of projects?
- What are the top 5 states with the great number of teacher donors?
- Find the total cost (qty * unit_price) of each vendor from resources?

---

Based on the Project type, what is the total and avg costs of projects?
```sql
SELECT TYPE, sum(cost), avg(cost) 
FROM PROJECTS
GROUP BY TYPE;
```

Group teacher donors by state and order the list by largest # of teacher donors to each state.
```sql
SELECT STATE, Count(*)
FROM DONORS
WHERE IS_TEACHER = 'Yes'
GROUP BY STATE
ORDER BY COUNT(*) DESC
LIMIT 5;
```

Find the total cost (qty * unit_price) of each vendor from resources?
```sql
SELECT VENDOR_NAME, sum(qty*unit_price)
FROM RESOURCES
GROUP BY VENDOR_NAME;
```

---

## Joins

Let's take a peak at the `Projects` table.

```sql
SELECT * FROM PROJECTS limit 10;
```

What do you suppose the `SCHOOLID` column means?

<div class="fragment">
This is a _reference_, also known as a _foreign key_.  It "points at" another table (in this case, the `Users` table).

Wouldn't it be nice if we could combine them?
</div>

---

#### Our First Join:  Projects with Schools
```sql
SELECT *
FROM PROJECTS AS P
INNER JOIN SCHOOLS AS S
ON P.SCHOOLID = S.ID;
```

:note:

A ton of new things here, so let's walk through it slowly.

- `SELECT *` looks familiar, but since we're getting data from two tables, it gives us all of the columns from both tables
- Using aliases for the tables. This is not necessary -- I could have written out the names every time -- but very useful.
- `ON`: This is the "join condition". We're gluing two tables together, and this specifies how to match things up. In the case, we want `SCHOOLID` from the PROJECTS table to match with `ID` from the SCHOOLS table. Let's look at the results and see that this is indeed the case.
- `INNER JOIN`: The default and most common kind of join.
  - If you had just said `JOIN` then SQL assumes you mean `INNER JOIN`. I like to write it out in full for clarity.
  - It means find every case where the join condition matches, and if a row in either table doesn't match then don't include it. (We'll talk about other types of joins in a bit.)
- Let's look at the results. A few things to note:
  - Multiple columns called ID, and they are different. SELECT Id FROM ... won't work; need to use the alias.
  - Rows from the Users table can appear more than once. Why?
  - Do any rows from the Comments table appear more than once? How do we know?

---

#### `WHERE`, `GROUP BY`, and `ORDER BY` still work

But you have to be careful when specifying columns. I like to _always_ include the table alias for clarity.

What does this do?

```sql
SELECT P.TITLE, P.SUBJECT_CATG, 
P.ID as "PROJECT_ID", S.ID as "SCHOOL_ID", S.NAME, S.STATE
FROM PROJECTS AS P
INNER JOIN SCHOOLS AS S
ON P.SCHOOLID = S.ID
WHERE S.STATE = 'Pennsylvania'
```

:note:

Get Comment text and name of commenter for all comments made after 2017-01-01 from users with reputation > 1000 with the word 'mysql' in them, sorted newest comments first.

---

#### Try answering these!

- Choose a Project ID and join it with ID in Resources - How many resources was needed for that project?
- Join Projects with 'Technology' resource code and Subject subcategory like Math. Order descending by Project cost.
- What state has the highest donor amount?

--

Choose a Project ID and join it with ID in Resources - How many resources was needed for that project?
```sql
SELECT * FROM PROJECTS P INNER JOIN RESOURCES R
on P.ID = R.PROJECT_ID
WHERE P.ID = '0006c59e2e24920011fbfd274ee7e377'
```

--

Join Projects with 'Technology' resource code and Subject subcategory like Math. Order descending by Resource Unit Prices.
```sql
SELECT * FROM PROJECTS P INNER JOIN RESOURCES R 
on P.ID = R.PROJECT_ID
WHERE P.RESOURCE_CATG = 'Technology' 
and SUBJECT_SUBCATG like '%Math%'
ORDER BY R.UNIT_PRICE desc
```

--

What 5 states has the highest donor amount?
```
SELECT D.STATE, sum(DN.DONATE_AMT)
FROM DONORS D INNER JOIN DONATIONS DN
on (D.ID = DN.DONOR_ID)
GROUP BY D.STATE 
ORDER BY sum(DN.DONATE_AMT) desc
LIMIT 5;
```

:note:

- This is called an Entity Relationship Diagram (ERD)
- The arrows represent foreign key relations -- where a column in one table references the id of another table.
- In this schema, Primary Keys are always called 'ID'. This is a convention (and a very common one) but it won't always be the case.
- Whats with the 'Types' tables?
- What's the deal with the Posts table pointing to itself?
  - Posts contains both questions and answers.
  - Answers have a ParentID which points to the question they are answering
  - Questions have an AcceptedAnswerId which points to the Answer that the questioner chose as best
  - (Work through an example -- number of answers given for each question)
- What's with the PostTags table? Why is this necessary?
  - It's called a "Through Table" and is necessary to represent a many-to-many relation: a Post can have many Tags, and a Tag can be applied to many Posts

---

#### Try these!

- What top 3 Subject Category and Grade level's are schools with 60% or more free lunch percentage creating projects for? (multiple group bys)
- What are the top 10 most popular vendors and metro types? (multiple joins)
- How many donors does each grade level category have?

--

What top 3 Subject Category and Grade level's are schools with 60% or more free lunch percentage creating projects for?
```sql
SELECT SUBJECT_CATG, GRADE_LEVEL_CATG, count(*)
FROM SCHOOLS S INNER JOIN PROJECTS P
on P.SCHOOLID = S.ID
WHERE S.FREE_LUNCH_PCT > 60
GROUP BY SUBJECT_CATG, GRADE_LEVEL_CATG
ORDER BY count(*) DESC
LIMIT 3;
```

--

Which users have answered the most questions?
```sql
SELECT S.metro_type, r.vendor_name, count(*) 
FROM SCHOOLS S 
inner join PROJECTS P on S.ID = P.SCHOOLID
inner join RESOURCES R on R.PROJECT_ID = P.ID 
group by S.metro_type, r.vendor_name
ORDER BY count(*) DESC
LIMIT 10
```

--

How many donors does each grade level category have?
```sql
SELECT P.GRADE_LEVEL_CATG, count(DN.ID) as "# of donations" FROM PROJECTS P 
INNER JOIN DONATIONS DN on P.ID = DN.PROJECT_ID
INNER JOIN DONORS D on D.ID = DN.DONOR_ID
group by P.GRADE_LEVEL_CATG
```

---

## Stretch Break!

![Stretch!](./gdi/img/stretch_dog.jpg "Stretch!")

:note:

Leave an hour for insert/update/delete, summary, and wrap-up. Omit or shorten sections on subqueries, outer joins, and HAVING if necessary to make sure students understand inner joins.

TODO: Start writing Intermediate SQL course :-)

---

## OUTER JOIN:
#### What if there are no matches?


```sql
SELECT S.CITY, R.vendor_name, count(*)
FROM SCHOOLS S 
inner join PROJECTS P on S.ID = P.SCHOOLID
inner join RESOURCES R on R.PROJECT_ID = P.ID 
where S.STATE = 'Pennsylvania'
group by s.CITY, r.vendor_name;
```

```sql
SELECT S.CITY, R.vendor_name, count(*)
FROM SCHOOLS S 
left outer join PROJECTS P on S.ID = P.SCHOOLID
left outer join RESOURCES R on R.PROJECT_ID = P.ID 
where S.STATE = 'Pennsylvania'
group by s.CITY, r.vendor_name;
```

<!-- .element: class="fragment" -->

Note: This is an advanced topic. Don't worry if you don't understand it yet!
<!-- .element: class="fragment" -->

:note:

Start with answer to previous question. Why doesn't it work to just change `HAVING COUNT(*) = 1` to `= 0`?

---

#### Try these!
- How many donations don't have donor data?
- How many users have not made a post yet?

--

- How many donations don't have donor data?
```sql
SELECT * FROM DONATIONS DN
LEFT OUTER JOIN DONORS D on DN.DONOR_ID = D.ID 
WHERE D.ID is null;
```

--

How many users have not made a post yet?
```sql
SELECT count(*) 
FROM PROJECTS P 
LEFT OUTER JOIN RESOURCES R on P.ID = R.PROJECT_ID
where R.PROJECT_ID is null
```

:note:

The second one is complicated and takes some lateral thinking.  Motivate it as follows:
- We know how to list all users:
  `SELECT * FROM users;`
- Now let's join in posts:
  `SELECT * FROM users INNER JOIN posts ON u.Id = p.OwnerUserId;`
- But we know that inner join will filter out users who have not made any posts (which happens to be exactly who we want to see).  So let's change to left join instead:
  `SELECT * FROM users LEFT JOIN posts ON u.Id = p.OwnerUserId;`
- How do users who have no posts appear in the above result?  They have all NULLs for the post.  So we can use a WHERE to filter them out.
  `SELECT * FROM users LEFT JOIN posts ON u.Id = p.OwnerUserId WHERE p.Id IS NLLL;`
- Cool, now we've just got a list of users who have made no posts.  I wanted a count, so change to COUNT(*)
  `SELECT COUNT(*) FROM users LEFT JOIN posts ON u.Id = p.OwnerUserId WHERE p.Id IS NLLL;`

---

## HAVING:
#### Filtering on a group

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING group condition
ORDER BY column_name(s);
```

---

Resource Categories and Grade Level Groupings with more than 1K projects:
```sql
SELECT P.RESOURCE_CATG, P.GRADE_LEVEL_CATG, COUNT(P.ID)
FROM PROJECTS P
GROUP BY P.RESOURCE_CATG, P.GRADE_LEVEL_CATG
HAVING COUNT(P.ID) >= 1000
ORDER BY COUNT(P.ID) DESC;
```

---

#### Try these!
- How many items have more than 50 records in resources?
- How many districts have more than 100 schools listed?

--

- How many items have more than 50 records in resoures?
```sql
SELECT item_name, count(*) 
FROM RESOURCES
GROUP BY item_name
HAVING count(*) > 50
ORDER BY count(*) desc
```

--

How many users have not made a post yet?
```sql
SELECT DISTRICT, count(*) 
FROM SCHOOLS
GROUP BY DISTRICT
HAVING count(*) > 100
ORDER BY count(*) desc
```

---

## How does the data get there in the first place?

:note:

Often you won't have permission to modify data, and usually you won't want to.

---

## CRUD:
### Not as gross as it sounds

- **C**reate -- adding new data
- **R**etrieve -- we've learned this part!
- **U**pdate -- modifying data that already exists
- **D**elete -- removing data

---

## `INSERT`ing new data

Let's edit the DonorsChoose data!

```sql
select * from RESOURCES;
```
<!-- .element: class="fragment" -->

```sql
INSERT INTO RESOURCES
  (ID, ITEM_NAME, QTY, UNIT_PRICE, VENDOR_NAME)
VALUES
('TESTGDI1', 'GDI Workshop' , 1, 60, 'GDI Philly');
```
<!-- .element: class="fragment" -->

:note:

Have student's insert their own data, then select to verify that it made it in there.

How did ID get there? It's an "auto-incrementing primary key"

Have each student figure out what their id number is.

---


## Now let's `UPDATE` your row!

Let's say the price increased. Now my table is out of date. Let's fix it.

```sql
UPDATE RESOURCES
SET UNIT_PRICE = 65
WHERE ID = 'TESTGDI1';
```
<!-- .element: class="fragment" -->

Why didn't I do it this way?<!-- .element: class="fragment" -->

```sql
UPDATE RESOURCES
SET UNIT_PRICE = 65
WHERE VENDOR_NAME = 'GDI Philly';
```
<!-- .element: class="fragment" -->

:note:

Have students update their row similarly.

---

## `DELETE` your data

What if you don't want to be in my table at all?

```sql
DELETE FROM RESOURCES
WHERE Id = 'TESTGDI1';
```
<!-- .element: class="fragment" -->

---

## How did the _table_ get there in the first place?

```sql
CREATE TABLE [name of table] (
    ID INTEGER NOT NULL PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) DEFAULT NULL,
    Cost INTEGER DEFAULT NULL,
    Type VARCHAR(255) DEFAULT NULL
);
```

:note:

- Filtering after grouping: `HAVING`
- Sub-queries
- Updating data in a database:
- Adding a row to a table: `INSERT`
- Modifying rows: `UPDATE`
- Removing rows: `DELETE`
- Updating the structure of the database itself
- Creating tables

What do the various data types mean?  Discuss Primary Key, Auto Increment

Fun open-ended exercise/discussion (if there's time): Say that GDI were a University. You work for the dean of GDI U and you need to track who has taken which classes so that you know when they are ready to graduate.  What tables would you need?  Draw out your Entity Relationship Diagram.  Think about what information the dean might ask you and what queries you'll have to write to get those answers.

Have students do this in small groups.

Sample answer:

Users (Id, Name)
Classes (Id, Name, Description)
ClassPrerequisites (Id, ClassId, PrerequisiteClassId)
Events (Id, ClassId FK, Date, Location, Price)
EventParticipants (Id, ClassId, ParticipantUserId, ParticipantTypeId, Grade) -- Grade non null for students only
ParticipantTypes (Id, Name) -- Name={Student, TA, Teacher}

---

## SQL and the modern world

- Relational databases have been around since the 1970s and is still the most widely used type of database
  - Virtually all billing, banking, and e-commerce systems use SQL
  - Salesforce, Tableau, WordPress, Wikipedia
- More recently (within the last 10 years or so), "Big Data" and "NoSQL" have become popular
  - _Sometimes_ better for specific uses

---

## Summary

#### Here's what we learned!

- Getting data out of your tables with `SELECT`
  - Counting rows with `COUNT`
  - filtering with `WHERE`
  - ordering with `ORDER BY`
  - Grouping with `GROUP BY`
  - Aggregating with `MIN`, `MAX`, `AVG`
  - Combining tables with various types of `JOIN`
  - Subqueries: queries within queries
- Adding data with `INSERT`
- Modifying existing data with `UPDATE`
- Getting rid of data with `DELETE`
- Creating new tables with `CREATE TABLE`

---

## What's next?
- More things you can do with joins and subqueries
- More SQL functions and expressions
- Structuring your data efficiently (Sondra's Database Design class)
- Modifying data safely: Transactions, constraints, ACID compliance
- Saving and sharing your SQL: Views, Triggers, and Stored Procedures
- Combining SQL with other programming languages (Object Relational Mappings)

---

## Resources
- GDI Philly's Slack has a `#sql` channel!
- [Head First SQL](https://www.amazon.com/Head-First-SQL-Brain-Learners/dp/0596526849/) Good visual book
- [Learn SQL The Hard Way](http://sql.learncodethehardway.org/d-Way.html) Not as hard as it sounds
- [DataPhilly Meetup](https://www.meetup.com/DataPhilly/)

Online resources like Udacity, DataCamp, Coursera, Kaggle, Codecademy, etc. have more SQL practice!

---

## Thanks for coming!

.

ejsupil@gmail.com

Twitter: @dailysup
