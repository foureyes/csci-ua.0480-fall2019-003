---
layout: slides
title: "Postgres and SQL Basics"
---

<section markdown="block" class="intro-slide">
# {{ page.title }}


### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>


<section markdown="block">
## PostgreSQL Background

__Database category:__

* relational
* __AND__ "object oriented"
	* allows overloaded functions
	* table inheritance 
	* extensible type system!

(We'll only be using the relational features, though)

</section>

<section markdown="block">
## Installation

__For all platforms, you can use the [installer](https://www.postgresql.org/download/)__

* it _should_ prompt you for a username and password
* make sure you remember it (username is postgres)

__If you're on MacOS:__

* preferred installation is to use [homebrew](https://brew.sh/)
	* `brew install postgresql`
	* watch output of install, and copy command given
	* (`pg_ctl -D /usr/local/var/postgres start` # start server!)
	* super user is your username, no password if connect locally
* Linux or Windows 10 - WSL 
	* Linux - apt / pacman / etc. (super user is `postgres`)


</section>

<section markdown="block">
## Server / Client

__Installation installs both server and client:__

* server must be running in order for it to be queried
	* server will run in background when using `pg_ctl`
	* (you can close tab, and it'll still be running)
* client can be commandline or graphical (rn only use commandline)
	* `psql` - commandline client
	* pgAdmin - graphical
	* DataGrip - graphical, not gree
* currently both server and client local (on same machine)
* eventually, server on remote machine!


</section>

<section markdown="block">
## Start, Stop, Restart

* `pg_ctl start` 
	* `-D datadir` - directory where postgres stores data
	* `-l filename` - where to log output
	* (on MacOS) `pg_ctl -D /usr/local/var/postgres start`
	* add `-l /usr/local/var/postgres/server.log`
* `pg_ctl stop`
* `pg_ctl restart` - stop and then start
* `pg_ctl reload` - reread config, but no need to completely stop and start again

Want to stop specifying the `-D` when starting?

* {:.fragment} `export PGDATA=/foo/bar`, add export to`.bashrc` or add export to `bash_profile` ... or modify `postgresql.conf`


</section>

<section markdown="block">
## Configuration

* `pg_hba.conf`
	* who can access / where they can access from
	* more about permissions, roles, security in next few classes
* `postgreql.conf` (logging, data dir, etc.)
* locations:
	* `/usr/local/var/postgres/pg_hba.conf`
	* `/usr/local/var/postgres/postgresql.conf`

Or... when connected to any database through `psql`:

`SELECT name, setting FROM pg_settings WHERE category = 'File Locations';`


</section>

<section markdown="block">
## Postgres Object Hierarchy

* __templates__ - base template(s) to copy database from
* __databases__ - multiple databases allowed in one postgres instance / "cluster" 
* __schemas__ - (ANSI SQL standard) next level of organization in database
	* database -> schema -> table
	* name spacing - useful for organizing many tables
	* default schema is `public`
	* we will stick to the public schema
* __tables__
* __views__ - abstraction of table / multiple tables: merge tables and perform calculations and present data as if it were a table; typically read only
* other - languages, functions, triggers, types, sequences

</section>

<section markdown="block">
## psql

__Commandline client: `psql` (should be installed when postgres is installed)__ &rarr;

Usage `psql dbName`: where `dbName` is name of database to connect to.

Optional flags include:

* `-U`  / `--username`
* `-W` / `--password`  prompt for password
* no dbName uses user name as db name



</section>

<section markdown="block">
## psql Commands 

__Change database__

* `\c db_name` - connect to different database

__Informational__ (append an `S` to most of these to show system objects)

* `\l` - list databases
* `\dt` - list tables
* `\dv` list views
* `\d` - list tables and views
* `\dn` - list schemas
* `\d table_name` - describe table
* `\du` - list users


</section>

<section markdown="block">
## HALP PLZ

__The `psql` client has a lot of built-in help!__

Use `\?` to show available `psql` commands

`\h` shows help for SQL statements

* `\h STATMENT` will show documentation for _that_ statement
* for example: `\h ALTER TABLE` shows help for the `ALTER TABLE` statement
* syntax and options will be shown
* type `q` to leave help / space to go to next page

</section>

<section markdown="block">
## Naming Conventions

* quoted names are case sensitive
* unquoted normalizes to lowercase (maybe bad depending on your table names)
* ...so make table names and column names:
	* lowercase
	* words separated by underscore
	* very descriptive
	* underscore id (foo_id) for foreign keys (more on this later)
	* be consistent with pluralization (always either use singular or always use plural)


</section>

<section markdown="block">
## Types

__PostgreSQL has _a lot_ of types (you can even create your own!)__

These are some high level categories where these types can fit in:

* {:.fragment} numeric
* {:.fragment} strings
* {:.fragment} date and time
* {:.fragment} _other_


</section>

<section markdown="block">
## Number Types 

__Check the docs on [numeric data types](https://www.postgresql.org/docs/current/static/datatype-numeric.html)__ &rarr;

* `serial` (auto incrementing, pk if no "natural pk" apparent, called artificial / surrogate)
* `integer` - typical choice for integer, 4 bytes
* `smallint` - 2 bytes, signed
* `bigint` - 8 bytes, signed
* `decimal` / `numeric` - arbitrary precision numbers

</section>

<section markdown="block">
## Strings

__See docs on [character data types](https://www.postgresql.org/docs/current/static/datatype-character.html)__ &rarr;

* `text` - unlimited length
* `varchar(n)` - where `n` is num of characters (character varying)

⚠️If casting to lesser length, string will be truncated to fit!

</section>

<section markdown="block">
## Dates and Times

See [docs on Date/Time types](https://www.postgresql.org/docs/current/static/datatype-datetime.html)

* `timestamptz` (timestamp __with timezon__, __use this__!)
	* stored as UTC, queried, shown in local time zone
* `timestamp` (no timezone)
* `date`
* `time`


</section>

<section markdown="block">
## Many Others!

__Check out this [table of all data types](https://www.postgresql.org/docs/10/static/datatype.html#DATATYPE-TABLE)!__ &rarr;

* {:.fragment} currency (`money` 💰)
	* will use the currency based on os-level localization settings
	* will format appropriately (commas, currency symbol, dots, etc.)
* {:.fragment} shapes (`circle`, `polygon`)
* {:.fragment} documents (`xml`, `json` / `jsonb`)
* {:.fragment} networking (`inet` for ipv4 and ipv6, `cidr` for ip ranges)


</section>

<section markdown="block">
## About SQL Syntax

__Whitespace (newlines, tabs) is ok within a statement, so formatting code with line breaks and indentation for readibility is encouraged!__

* {:.fragment} end statements with `;`
* {:.fragment} comments start with `--`
* {:.fragment} SQL keywords can be written in upper or lower case
	* but it's common practice to uppercase keywords 
	* (or at least remain consistent)

Note that in postgres:
{:.fragment}

* when writing SQL, names of objects are lowered
* (double quote if you don't want this behavior) 
* so - in postgres - __avoid uppercase letters in names of objects so that quoting isn't required__
{:.fragment}


</section>

<section markdown="block">
## Strings, Numbers, NULL 

__Delimit strings with single quotes `'`__ &rarr;

* escape with extra `'` (`'` --> `''`)
* or use double $ as quotes `$$what's this$$`
* `E'\t'` - prefix with E to use \ as escape character

__Numbers are just numeric literals: `5`, `1.23`__

__`NULL` means no value or missing value__

</section>
<section markdown="block">
## Creating a Database

__A database can have the following attributes__

* character encoding
* collation (sort order of characters)
* clones template1 (also a template 0)

To create a table:

<pre><code data-trim contenteditable>
CREATE DATABASE someDatabaseName;
--uses same encoding and collation as template1
</code></pre>

See [CREATE DATABASE](https://www.postgresql.org/docs/10/static/sql-createdatabase.html) docs


</section>

<section markdown="block">
## Creating a Table Background Info

See [CREATE TABLE](https://www.postgresql.org/docs/10/static/sql-createtable.html) doc

* `CREATE TABLE`
* followed by table name
* in parens...
	* comma separated list of column names and their type separated by space:
	* `first_name` varchar(255)
* can specify some constraints after type, such as:
	* `NOT NULL`
	* `UNIQUE`
	* `PRIMARY KEY`
* default value specified with:
	* `DEFAULT value_to_default_to`
</section>

<section markdown="block">
## Creating a Table Example

__Create a student table with 5 fields: netid first, last, midterm and registered__ &rarr;

<pre><code data-trim contenteditable>
CREATE TABLE student(
	 netid varchar(20) PRIMARY KEY,
	 first varchar(255) NOT NULL,
	 last varchar(255) NOT NULL,
	 midterm numeric,
	 registered timestamptz DEFAULT NOW()
);
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## Create (INSERT)

__To add a new row to a table, use an <span class="hl">INSERT</span> statement__ &rarr;

<pre><code data-trim contenteditable>
-- values in order of field names (registered
-- left out, as it has a default value)
INSERT INTO student 
	VALUES ('fb123', 'foo', 'bar', 90);
</code></pre>
{:.fragment}

<pre><code data-trim contenteditable>
-- specify columns and matching values (does not have 
-- to follow same order of columns in CREATE TABLE)
INSERT INTO student 
	(first, last, midterm, netid) 
	VALUES ('baz', 'qux', 70, 'bq789');
</code></pre>
{:.fragment}

* {:.fragment} insert multiple rows: add commas to after each "tuple" of values
* {:.fragment} [See docs on INSERT](https://www.postgresql.org/docs/current/static/sql-insert.html)

</section>

<section markdown="block">
## Read (SELECT)

__Use a <span class="hl">SELECT</span> statement to _read_ data__ &rarr;

* start with `SELECT`
* followed by a comma separated list of columns or calculated values you'd like to see
	* you can use `AS some_alias` to alias column names or name calculations
	* `*` means all columns
	* arithmetic operators and functions / expressions can be used here (see next slide)!
* then keyword `FROM tablename` ...to specify which table to query

</section>

<section markdown="block">
## Operators and Functions

__Operators__ &rarr;

* arithmetic: `+`, `-`, `*`, `/`
* string concatenation: `||` (`'HI' || 'THERE'`)
* logical operators: `AND`, `OR`, `NOT`
* check for NULL: `IS NULL` and `IS NOT NULL`
* pattern matching: `LIKE`

__Functions__ &rarr;

* `NOW()` (current date / time), `ROUND(val)`, etc.

See [documentation on operators and functions](https://www.postgresql.org/docs/9.1/static/functions.html)
</section>

<section markdown="block">
## `SELECT` Examples

__Using the student table created earlier__ &rarr; 

* {:.fragment} get all students
	<pre class="fragment"><code data-trim contenteditable>
SELECT * FROM student;
</code></pre>
* {:.fragment} get all students, just netid, first, and name last name ... and alias first as fn
	<pre class="fragment"><code data-trim contenteditable>
SELECT netid, first as fn FROM student;
</code></pre>
* {:.fragment} get all students, show net id and midterm grade divided by 100
	<pre class="fragment"><code data-trim contenteditable>
SELECT netid, midterm / 100 FROM student;
</code></pre>

</section>

<section markdown="block">
## `SELECT` + `DISTINCT`

__Only show the distinct rows (remove duplicate rows) by using `DISTINCT` with `SELECT`__ &rarr;

Show the distinct first names of students:

<pre><code data-trim contenteditable>
SELECT DISTINCT first 
	FROM STUDENT;
</code></pre>

</section>


<section markdown="block">
## `SELECT` with `WHERE` Clause

__Optionally, add a `WHERE` clause to specify _conditions_ (think filtering)__ &rarr;

* conditions can use operators like `=`, `<>` (not equal), `>`, `<`
* you can also use `LIKE` and `ILIKE` with `%` representing _wildcards_ to match on substrings (`ILIKE` is case insensitive)
* use `colName IS NULL` to check for a `NULL` value
* multiple conditions can be put together with `AND`, `OR`, `NOT`
* parentheses can be added to specify precedence
</section>

<section markdown="block">
## `SELECT` + `WHERE`

__Filter your `SELECT` statement results with a `WHERE` clause__ &rarr;

* {:.fragment} only students with midterm > 80
	<pre class="fragment"><code data-trim contenteditable>
SELECT * FROM student WHERE midterm > 80;
</code></pre>
* {:.fragment} only students with between 70 and 90
	<pre class="fragment"><code data-trim contenteditable>
SELECT * FROM student 
	WHERE midterm > 70	
	AND midterm < 90;
</code></pre>

</section>

<section markdown="block">
## `SELECT` + `WHERE` Continued

* {:.fragment} students that have no midterm score
	<pre class="fragment"><code data-trim contenteditable>
SELECT * FROM student WHERE midterm IS NULL;
</code></pre>
* {:.fragment} get the netid and first name of students with that have a netid that has `jv` in it or starts with `Jo`, case insensitive
	<pre class="fragment"><code data-trim contenteditable>
SELECT netid, first FROM student 
	WHERE netid LIKE '%jv%'	
	OR first ILIKE 'Jo%';
</code></pre>

</section>

<section markdown="block">
## Ordering

__Add an `ORDER BY` clause at the end of your `SELECT` to specify ascending orderi__ &rarr;

<pre><code data-trim contenteditable>
SELECT * FROM student 
	WHERE midterm < 60
	ORDER BY registered;
</code></pre>
{:.fragment}

__Add `DESC` to order in descending order__ &rarr;
{:.fragment}

<pre><code data-trim contenteditable>
SELECT netid FROM student 
	ORDER BY registered desc;
</code></pre>
{:.fragment}

__Separate multiple column names to order by multiple columns__ &rarr;
{:.fragment}

<pre><code data-trim contenteditable>
SELECT * FROM student ORDER BY last, first;
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## `LIMIT`

__A <span class="hl">LIMIT</span> clause can be added at the end of a `SELECT` statement (after `ORDER BY`) to contrain the number of results of the query__ &rarr;

Only show 10 results:

<pre><code data-trim contenteditable>
SELECT netid, first, last
	FROM student
	ORDER BY netid
	LIMIT 10;
</code></pre>

</section>

<section markdown="block">
## Update (`UPDATE`)

__Use an `UPDATE` statement to set the value of a column for a row / rows__ &rarr;

* start with keyword, `UPDATE`
* then name of table to update
* followed by keyword `SET`
* finally column name = some value
* (see [docs on `UPDATE`](https://www.postgresql.org/docs/current/static/sql-update.html)

<pre><code data-trim contenteditable>
-- set all students' registered field to 1/1/2018
UPDATE student SET registered = '2018-01-01';
</code></pre>
</section>

<section markdown="block">
## `UPDATE`, Expressions, and `WHERE`

__Add a `WHERE` clause to `UPDATE` (after `SET`) to specify which rows to change__ &rarr;

<pre><code data-trim contenteditable>
-- only set midterm score for rows that
-- have netid fb123
UPDATE student 
	SET midterm = 80 
	WHERE netid = 'fb123';
</code></pre>

__Remember the value in `SET` can be an expression__ &rarr;

<pre><code data-trim contenteditable>
UPDATE student SET registered = NOW();
</code></pre>

<pre><code data-trim contenteditable>
-- probz a bad idea to set pk to this, but...
-- set netid to concatenation of first and last
UPDATE student SET netid = first || last;
</code></pre>

</section>

<section markdown="block">
## Delete / Remove Rows (`DELETE`) 

__To remove rows from a table use the `DELETE` statement__ &rarr;

<pre><code data-trim contenteditable>
DELETE FROM student WHERE midterm > 90;
</code></pre>

Again, note the `WHERE` clause specifying which rows to take action on.

(See the [documentation on `DELETE`](https://www.postgresql.org/docs/current/static/sql-delete.html))
</section>

<section markdown="block">
## Add / Remove Column 

__Use `ALTER TABLE` to add / remove columns__ &rarr;

* {:.fragment} add a new column
	<pre class="fragment"><code data-trim contenteditable>
ALTER TABLE student 
	ADD COLUMN final_exam_score;
</code></pre>
* {:.fragment} add a new column with a default value
	<pre class="fragment"><code data-trim contenteditable>
ALTER TABLE student 
	ADD COLUMN final_exam_score numeric DEFAULT 80;
</code></pre>
* {:.fragment} remove a column
	<pre class="fragment"><code data-trim contenteditable>
ALTER TABLE student DROP COLUMN final_exam_score;
</code></pre>
</section>

<section markdown="block">
## Modifying Columns

__`ALTER TABLE` can also be used to modify columns__ &rarr;

<pre><code data-trim contenteditable>
-- change data type of column
ALTER TABLE student 
	ALTER COLUMN netid 
	SET DATA TYPE varchar(200);
</code></pre>

<pre><code data-trim contenteditable>
-- rename column
ALTER TABLE student 
	RENAME COLUMN midterm TO midterm_score;
</code></pre>

See [full documentation on `ALTER TABLE`](https://www.postgresql.org/docs/10/static/sql-altertable.html)
</section>

<section markdown="block">
## `GROUP BY` and Aggregate Functions

__Adding a `GROUP BY` clause allows you to group together rows and run aggregate functions on those groups__ &rarr;

* the `GROUP BY` clause goes after `FROM` and `WHERE`
* a column must be specified after `GROUP BY`

Additionally, and Aggregate function can be included in the column list in `SELECT`:

* `AVG`
* `SUM`
* `MAX`
* `MIN`
* `COUNT`

See the [documentation for `GROUP BY`](https://www.postgresql.org/docs/10/static/sql-select.html#SQL-GROUPBY)
</section>

<section markdown="block">
## `GROUP BY` Examples

__Group by first name, show counts for each group (for example, 5 students named alice, 3 students named bob, etc.)__ &rarr;

<pre class="fragment"><code data-trim contenteditable>
SELECT FIRST, COUNT(*) FROM student GROUP BY first;
</code></pre>

Note that it doesn't matter what column name is passed to count (and `*` works too), since we're simply counting
{:.fragment}

__Again, group by first name, but this time show the midterm average for students with same first name__ &rarr;
{:.fragment}

<pre class="fragment"><code data-trim contenteditable>
SELECT FIRST, AVG(midterm) FROM student GROUP BY first;
</code></pre>

In this case, the `midterm` column is the argument used for `AVG`
{:.fragment}
</section>


<section markdown="block">
## Filtering Groups

__Add a `HAVING` clause after `GROUP BY` to eliminate groups that do not satisfy a condtion__ &rarr;

From the [documentation on `HAVING`](https://www.postgresql.org/docs/10/static/sql-select.html#SQL-HAVING):

* "`HAVING` is different from `WHERE`:" 
* "`WHERE` filters individual rows before the application of `GROUP BY`
* "while `HAVING` filters group rows created by `GROUP BY`

<pre><code data-trim contenteditable>
SELECT FIRST, AVG(midterm) 
	FROM student 
	GROUP BY first
	HAVING AVG(midterm) > 70;
</code></pre>

</section>

<section markdown="block">

## Casting

__To cast a value from one type to another in a SQL statement, use either of the two expressions__ &rarr;

* `CAST (colname as newType)`
* `val::newType`

<pre><code data-trim contenteditable>
SELECT netid, 
	CAST (midterm AS smallint) AS smol_mid
	FROM student;
</code></pre>

<pre><code data-trim contenteditable>
-- assume that midterm is integer
-- cast to numeric
SELECT * FROM student 
	ORDER BY ROUND(midterm::numeric, 2);
</code></pre>
</section>

<section markdown="block">
## `ROUND` / formating

__`ROUND` rounds a numeric value to a specified number of decimal places__

There's a one argument version that rounds to an integer.

Used in conjunction with casting, some simple formatting can be done:

<pre><code data-trim contenteditable>
-- assuming midterm is now an integer
-- cast to numeric
-- so that we can round to two places
SELECT netid, ROUND(CAST(midterm AS numeric), 2)
	FROM student;
</code></pre>

</section>

<section markdown="block">
## Removing Tables / Databases

__Use the `DROP` command to remove databases or tables__ &rarr;

* `DROP TABLE table_name;`
* `DROP DATABASE database_name;`

Notes on usage:

* you must connect to another database if you are planning on dropping the database that you're currently connected to
* use `IF EXISTS` to suppress errors if the table you are dropping doesn't exist
	* DROP TABLE IF EXISTS table_name;

</section>
<!--

<section markdown="block">
## update with another field

```
update movie set roi=(gross - budget)/budget;
```

</section>
-->

<section markdown="block">
## Importing Data

__Issuing a series of manual `INSERT` statements to bring in data can be quite tedious!__ &rarr;

Fortunately, <span class="hl">data can also be imported by running .sql scripts or importing files</span> (csv, tab delimited).

The typical workflow for imports is to:

1. create a table based on the data that you'll import
2. potentially clean the data so that the import works well
3. import a file with a `COPY` query or generate `INSERT` statements in a `.sql` file
</section>

<section markdown="block">
## Running SQL Scripts

__Two ways to run `.sql` scripts:__ &rarr;

* in the `psql` client, use the `\i` command:
	* `\i /path/to/stuff-to-import.sql`
* when starting psql, a file that contains sql commands can be redirected to the client so that statements within it are run:
	* `psql someDatabaseName < /path/to/stuff-to-import.sql`

In both cases, the `.sql` file can contain any number of valid sql commands.
</section>

<section markdown="block">
## A Sample .sql Script

__In songs.sql__ &rarr;

<pre><code data-trim contenteditable>
DROP TABLE IF EXISTS song;
CREATE TABLE song (
	id serial PRIMARY KEY,
	title varchar(100),
	artist varchar(100),
	danceability numeric
);

INSERT INTO song (title, artist, danceability)
	VALUES
		('Heartbeats', 'Jose Gonzalez', 0.01),
		('Heartbeats', 'The Knife', 0.9),
		('Lucid Dreams', 'Juice WRLD', 0.9);
</code></pre>

</section>
<section markdown="block">
## Running Sample SQL Script

__Before running psql:__ &rarr;

<pre><code data-trim contenteditable>
psql class11 < songs.sql
</code></pre>

__Or, while in psql:__ &rarr;

<pre><code data-trim contenteditable>
\i songs.sql
</code></pre>
</section>
<section markdown="block">
## `COPY` from csv

__`COPY` can be used to import data from a csv__ &rarr;

(note that `COPY` is not standard SQL)

<pre><code data-trim contenteditable>
COPY table_name 
    FROM filename
    options
</code></pre>

A list of specific columns can be added after `table_name` in parentheses (for example: `song (title, artist, danceability)`)

`options` can be replaced by some combination of additional options that control how file should be imported (see next slide).

The [`COPY` documentation shows more details on usage](https://www.postgresql.org/docs/current/static/sql-copy.html)
</section>

<section markdown="block">
## `COPY` options

__Options can be__ &rarr;

* format of file: `text`, `csv` or `binary` 
* `DELIMITER AS 'some char'` - specify delimiter (default is comma for csv)
* `NULL AS 'null_string'` - determines what string to treat as null (default for csv is empty string)
* `HEADER` - presence specifies that header is included in file
* `QUOTE AS 'quote_character'` - specify quote character

</section>

<section markdown="block">
## Example csv File for `COPY`

__Here's an example csv - note that there's no whitespace before or after the delimiter (otherwise, it'll be included in value!)__ &rarr;

<pre><code data-trim contenteditable>
title,artist,danceability
Heartbeats,Jose Gonzalez,0.01
Heartbeats,The Knife,0.9
Lucid Dreams,Juice WRLD,0.9
Happy Birthday,N/A,0.9
</code></pre>

</section>

<section markdown="block">
## `COPY` Examples

__Assuming that a table exist with appropriate types__ &rarr;

<pre><code data-trim contenteditable>
id serial PRIMARY KEY,
title varchar(100),
artist varchar(100),
danceability numeric
</code></pre>

* a file with a comma delimiter can be imported 
* the fields to be filled are `title`, `artist`, `danceability`
* this allows a `serial` primary key to not have to be specified in the csv (id will be generated!)

<pre><code data-trim contenteditable>
-- 4 columns, but only 3 in csv
COPY song (title, artist, danceability) 
	FROM '/tmp/songs.csv' 
	csv HEADER NULL AS 'N/A';
</code></pre>

</section>

<section markdown="block">
## Another Example, w/ Tabs

__Assuming a tab delimited file that contains all the columns needed (for example, primary key is not artifical, but a _natural_ key instead)__ &rarr;

<pre><code data-trim contenteditable>
COPY student 
	FROM '/tmp/students.txt' 
	csv HEADER DELIMITER AS E'\t';
</code></pre>

In this case: 

* every column in table exist in csv
* the delimiter is specified as `E'\t'` ... 
* E means use backslash to escape (and, consequently, specifies tab as the delimiter)

</section>

{% comment %}
<section markdown="block">
## Running SQL Scripts

</section>
<section markdown="block">
## import


```
DROP TABLE IF EXISTS sd;
CREATE TABLE sd (
	sd_state text,
	sd_geoid text,
	sd_name text,
	sd_lowestGrade text,
	sd_highestGrade text,
	sd_pop_2010 integer,
	sd_hu_2010 integer,
	sd_aland real,
	sd_awater real,
	sd_aland_sqmi real,
	sd_awater_sqmi real,
	sd_intptlat real,
	sd_intptlong real,
	PRIMARY KEY(sd_geoid)
);
```
```
copy sd from '/tmp/Gaz_elsd_national.txt' delimiter E'\t' CSV header;
```
</section>



<section markdown="block">
## 

<pre><code data-trim contenteditable>

select count(*) as movie_count, director, round(avg(budget::numeric), 2)::money as avg_budget 
	from movie 
	group by director 
	having count(*) > 1 
	order by movie_count desc;

</code></pre>

</section>

{% endcomment %}








