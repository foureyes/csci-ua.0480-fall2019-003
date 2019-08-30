---
layout: slides
title: "Databases Overview"
---

<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## ⚠️ Broad Generalizations Ahead

__Just a warning: There are some generalizations about databases coming up ahead:__ &rarr;

* {:.fragment} they're true for most of the databases you'll encounter
* {:.fragment} however, it's a constantly evolving field with many technologies, so...
	* {:.fragment} there are likely databases out there that are outside of these generalizations
	* {:.fragment} existing databases typically have lesser known features that share the same functionality as other competing databases 
	* {:.fragment} for example, although Postgres is well known as a relational database, you can use it as a _NoSQL_ - like document store to store JSON (really Binary JSON - _JSONB_) documents
</section>

<section markdown="block">
## Where Does Your Data Live? 🏠

So far, as we've been working with data in class and in our homework, __where was our data stored__? &rarr;

* {:.fragment} on the file system as `.csv`s, `.txt` files, etc.)
* {:.fragment} and I suppose, technically, on _someone else's_ file system (like say, a website)

<br>
__What are some downsides to storing data in `.csv`s, `.txt` files or even `.html` (😮)?__ &rarr;
{:.fragment}

* {:.fragment} it's difficult to manipulate that data <span class="fragment">unless you use another tool, like `DataFrame`</span>
* {:.fragment} if you're using `pandas` and reading a file in-memory as a `DataFrame`, what other problems might you encounter?
* {:.fragment} you're gonna run out of memory 🤷
</section>

<section markdown="block">
## Storing Data

__What are some _other_ options for storing data...__ &rarr;

* {:.fragment} _in the cloud_ (S3, firebase, SalesForce)
* {:.fragment} in a database

__Enough `.csv` files... we'll be using a database now!__
{:.fragment}

__But wait, what's a database? And where does _it_ store its files__ &rarr;
{:.fragment}
</section>

<section markdown="block">
## A Database

__A <span class="hl">database</span> is a repository or _organized_ collection of data__ &rarr;

__However, it doesn't just store data, it also _manages_ data through a <span class="hl">database management system</span> (DBMS)__ &rarr;
{:.fragment}

* {:.fragment} a __DBMS__ is software that gives users and applications the ability to __define, create, query and administer a database__
* {:.fragment} data is usually stored in a __DBMS specific format on the file system__ (though this could vary based on the DBMS... for example, in memory only)


</section>
<section markdown="block">
## SO MANY DATABASES

[Check out this ranking of databases on db-engines.com](https://db-engines.com/en/ranking)...

Sooo many databases! Literally 100's. __Let's see how wen categorize these databases__ &rarr;
{:.fragment}

__We can categorize databases as:__ &rarr;
{:.fragment}

* {:.fragment} relational
* {:.fragment} object
* {:.fragment} nosql (also _non-relational_ ... or _not only SQL_ ... or _Not SQL_)
* {:.fragment} hybrids of the above!

</section>

<section markdown="block">
## More NoSQL

__So, NoSQL, is kind of a catch all term for anything that's not-relational, but there can be a ton of these!__

__NoSQL databases can be further categorized by the data model they use:__ &rarr;
{:.fragment}

* {:.fragment} key-value
* {:.fragment} document
* {:.fragment} column
* {:.fragment} graph
* {:.fragment} triple/quad store (RDF) 

[See the wikipedia article](https://en.wikipedia.org/wiki/NoSQL#Types_and_examples_of_NoSQL_databases)
{:.fragment}


</section>

<section markdown="block">
## Relational Databases

__Relational databases__ organize data in a collection of tables (relations).  __You can probably describe characterstics of a relational database because we've already worked with tabular data!__ &rarr;

* {:.fragment} each table has named <span class="hl">columns</span>... with the actual data that populates the table in separate <span class="hl">rows</span>
* {:.fragment} rows are sometimes referred to as <span class="hl">records</span> or <span class="hl">tuples</span>
* {:.fragment} each table represents a kind or _type_ of data, with every row representing an _instance_ of that data



</section>


<section markdown="block">
## Relations, Rows, and Keys

__Eeach table row has <span class="hl">primary key</span> that:__

* {:.fragment} uniquely identifies that row 
* {:.fragment} rows in one table can be _related_ to rows in other tables by:
	* adding a column that contains the _other_ table's primary key
	* this column is called <span class="hl">foreign key</span>
	
Using this pattern, complex data relationships can be modelled using primary keys and foreign keys.
{:.fragment}
</section>

<section markdown="block">
## Relational Databases Stereotypes

__Because relational databases are usually used to model relationships between _entities_ ...__ &rarr;

* {:.fragment} relational databases are typically pretty rigid:
* {:.fragment} they're highly structured
* {:.fragment} columns and types of columns must be defined prior to inserting rows
* {:.fragment} many relational database features deal with maintaining  _data integrity_ (such user defined data constraints, foreign keys, etc.)

[This database consultant has a pretty good write-up on relational databases](http://r937.com/relational.html)
{:.fragment}
</section>

<section markdown="block">
##  Transactions

__A <span class="hl">Transaction</span> is a single logical operation or unit of work in a DBMS__ 
{:.fragment}

* {:.fragment} this unit of work can be composed of multiple create, read, update, and delete operations (CRUD)
* {:.fragment} for example, a single transaction may:
	1. {:.fragment} read a value from one record in one table
	2. {:.fragment} then update another record in another table using the value that was previously read



</section>

<section markdown="block">
## Transactions Continued

__In order for a database to:__
{:.fragment}

* {:.fragment} be <span class="hl">consistent</span> / recover correctly from a crash or failure (fault tolerant)
* {:.fragment} maintain <span class="hl">isolation</span> between concurrent accesses to the database
* {:.fragment} transactions must be: <span class="fragment"><span class="hl">atomic</span>, <span class="hl">consistent</span>, <span class="hl">isolated</span>, and <span class="hl">durable</span></span> 

</section>

<section markdown="block">
## ACID ⚗️😮

__These properties of a transaction - atomicity, consistency, isolation and durability - are sometimes collectively referred to as <span class="hl">ACID</span>__ &rarr;

* {:.fragment} __Atomicity__ - each _transaction_ / (series of operations in a transaction) is all or nothing 
* {:.fragment} __Consistency__ - every _transaction_ ensures that the resulting database state is valid (goes from one valid state to another)
* {:.fragment} __Isolation__ - a failed _transaction_ should have no effect on other transactions (even if the transactions are concurrent)
* {:.fragment} __Durability__ - once a _transaction_ / operation is done, the results will remain persistent even through crash, power loss, etc.

See [the ACID article on wikipedia](https://en.wikipedia.org/wiki/ACID) for more details
{:.fragment}

</section>

<section markdown="block">
## SQL

__To query (read, update, etc.) and maintain a relational database, a domain specific language called <span class="hl">SQL</span> is used.__ &rarr;

* {:.fragment} it's <span class="hl">declarative programming language </span>
	* your program describes _what_ you want
	* rather than _how_ you want to achieve it
	* (describe the outcome rather than the algorithm)
* {:.fragment} __SQL has been standardized__ by The American National Standard Institute (ANSI) and the International Standard Organization (ISO)
* {:.fragment} despite the standardization, there are still __many dialects of SQL specific to the database platform being used__ (and are consequently _non portable_)
* {:.fragment} SQL's theoretical foundation is [relational algebra](https://en.wikipedia.org/wiki/Relational_algebra) (which we'll discuss later in this course)

</section>

<section markdown="block">
## Declarative Programming

__Remember, <span class="hl">SQL is declartive</span>, so it describes what you want the outcome of your program to be, not how to get to the outcome__ &rarr;
```
# we're describing _how_ in this Python code
oldies = []
for user in users:
	if user.birthday > '1990-01-01':
		oldies.append(user)
```
{:.fragment}

```
--we're explaining _what_ in this SQL query
SELECT first, last 
	FROM user 
	WHERE birthday > '1990-01-01';
```
{:.fragment}

</section>

<section markdown="block">
## Quick Demo of Designing a Data Model for a Relational Database

Maybe we want to store these fields (we'll discuss this in more detail in a later class):

* first name
* last name
* street address
* city
* state
* zip

<br>
__Let's get to it!__ &rarr;

</section>

<section markdown="block">
## Examples of Relational Databases

__What are some examples of relational databases?__ &rarr;

* {:.fragment} MySQL / MariaDB
* {:.fragment} PostgreSQL
* {:.fragment} Oracle
* {:.fragment} Microsoft SQL Server

<br>
These are all great choices for storing highly structured data, related data.
{:.fragment}

For our purposes (small to medium sized data sets, basic queries and data analysis), there is not much difference among these databases with the exception of perhaps platform, terms of use, and _cost_ 
{:.fragment}

</section>

<section markdown="block">
## Object Databases

__Of course, tables aren't the only way to organize data__ &rarr;

With the popularity of _object-oriented programming_ in the 80's and 90's, databases began to represent information in the form of objects... and they supported features such as:
{:.fragment}

* {:.fragment} the ability to declare custom types
* {:.fragment} inheritance
* {:.fragment} object versioning support (you'll see this in NoSQL document stores as well)
* {:.fragment} relationships through pointers and the ability to retrieve _all_ the data about an object in a single query

In addition to object databases, hybrid object-relational databases sprang up as well
{:.fragment}
</section>

<section markdown="block">
## NoSQL Databases

__NoSQL__ databases can be categorized by how they store their data:

* key-value
* document
* column
* graph
* triple/rdf store
* there are others 
	* (such as object, tuple store, etc.)
	* [check out a whole list](https://en.wikipedia.org/wiki/NoSQL#Types_and_examples_of_NoSQL_databases)
* note that nosql databases _can_ have reliable transactions as well, but this is usually not the focus of a nosql database

</section>

<section markdown="block">
## Key Value Store

Probably the most simple conceptually... data is stored in key/value pairs. __This should sound similar to some data structures that you've seen before.__ &rarr;

* {:.fragment} maybe a hash
* {:.fragment} or a dictionary
* {:.fragment} or an associative array

<br>
They're typically good at scaling to handle large amounts of data and dealing with high volumes of changes in data.
{:.fragment}

</section>

<section markdown="block">
## Key Value Store Examples

Some key value databases include:

* HBase (key value and columnar, distributed, large data sets)
* Redis (a popular backend for queuing)
* Memcache (as the name implies, typically used for caching)
* Riak
* [many others](https://en.wikipedia.org/wiki/Key-value_database#KV_-_eventually_consistent)
</section>


<section markdown="block">
## Document Stores

As you might guess by the name, __document stores__ organize data semi-structured documents. 

* think JSON (but there are many possible formats, such as XML, YAML, etc.)
* or... a _richer_ key-value store (there's _meta data_ within the document... the keys are usually meaningful)
* typically, no schema is required (that is, data types of values are inferred from values)
* typically, semi structured (documents, property names, etc... do not have to be pre-defined)
* some document stores are particularly featureful when it comes to high availability and scaling (through replication/redundancy and sharding/separating large databases into smaller ones)

</section>

<section markdown="block">
## Document Stores Continued

__Document stores are particularly good for applications where flexible data storage or constantly changing data storage is required.__

Two of the most popular NoSQL document stores are:
{:.fragment}

* {:.fragment} CouchDB
* {:.fragment} MongoDB

<br>
Of course, there are a [bunch of others](https://en.wikipedia.org/wiki/Document-oriented_database#Implementations)
{:.fragment}



</section>
<section markdown="block">
## Document Stores Use Cases

__Some use cases for document stores include:__

* applications that require semi structured data / data that has does not have rigid requirements (perhaps a resume)
* again, large volumes of data
* _fluid_ data or data whose structure is prone to change

</section>

<section markdown="block">
## Graph Databases

__In a <span class="hl">Graph Database</span>__:

* data is stored as nodes and edges of a graph
* edges represent relationships that directly link items in the database
* useful for modelling complex hierarchical relationships, networks, or any data set that fits into a graph-like structure
* ex. [Neo4j](https://en.wikipedia.org/wiki/Neo4j), and [Neo4j use cases](https://neo4j.com/use-cases/)
</section>

<section markdown="block">
## NewSQL

__Uh, yes, really. NewSQL__

* relational databases that...
* provide same scalability as NoSQL databases
* and ACID guarantees of relational databases

Examples: Google Spanner, Apache Ignite
</section>

<section markdown="block">
## Postgres

__We'll be using PostgreSQL (also called postgres, pgsql):__ &rarr;

* {:.fragment} it's a hybrid _object-relational_ database
* {:.fragment} it's _open source_ 
* {:.fragment} it's _up there_ in terms of [relational database rankings](https://db-engines.com/en/ranking)

Um, why?
{:.fragment}

__Mainly because of wide support in the Python community (not necessarily for data analysis, but for other libraries)__
{:.fragment}

Again... you'll find very few differences between mysql and postgres for the work that we do, and as new versions of each are released, they usually catch up in terms of features and performance.
{:.fragment}


</section>

