---
layout: slides
title: "Database Design"
---

<script src="../../resources/js/table.js"></script>
<link rel="stylesheet" href="../../resources/css/data-table.css" type="text/css" media="screen" title="no title" charset="utf-8">

<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## Steps for Writing Software?

__So... u want to write an _awesome_ app..__ &rarr;

What are the high level steps for writing a program (_do you just start coding_)?
{:.fragment}

1. {:.fragment} requirements
2. {:.fragment} implementation
3. {:.fragment} testing
4. {:.fragment} bug fixing, refactoring (go back to 2 or even 1)

(Regardless of software development methodology you use, you'll end up using some variant of the above)
{:.fragment}

</section>

<section markdown="block">
## Makin' a Database

__As with software engineering, you don't want to just jump in and start creating tables haphazardly...__ &rarr;

The database design process may look something like:
{:.fragment}

1. {:.fragment} requirements analysis - specify the problem, work with _domain experts_, explore existing data, etc. ... to: 
	* {:.fragment} determine what data needs to be stored 
	* {:.fragment} how the data is related to each other?
2. {:.fragment} __conceptual design__ - just entities, attributes and relationships
	* {:.fragment} use requirements to formally describe the data, relationships and constraints
	* {:.fragment} says nothing about _actual_ implementation (no details on platform, physical storage, etc.)	
	* {:.fragment} most simple model

</section>

<section markdown="block">
## Continued...

__After conceptual design...__ &rarr;

3. {:.fragment} translate conceptual design to __logical design__ - no database creation yet, but add types, unique identifiers... more complex than conceptual design
4. {:.fragment} translate logical design to actual _objects_ in specific DBMS / db platform

</section>
<section markdown="block">
## Design Rules

What should your database design accomplish? &rarr;

* {:.fragment} ability to solve problem
	* understand problem
	* fulfill _business_ requirements
		* reporting and data analysis
* {:.fragment} can store required data
	* structure can satisfy required queries
	* correct fields and types of fields
* {:.fragment}  models relationships

</section>

<section markdown="block">
## Design Rules Continued 

* {:.fragment}  as we saw previously... data integrity
	* data is correct
	* relationships are correct
	* ... avoid anomalies, inconsistency, and redundancy
*  {:.fragment} flexibility to handle future changes
*  {:.fragment} performance, efficiency
	* indexes, modify queries, etc.
	* typically done after-the-fact
	* design should be influenced by this if efficiency is the first requirement, but typically this leads to compromises in other parts of the design

</section>

<section markdown="block">
## Jargon Again

* {:.fragment} __data model__ - specifies the data and relationships to a DBMS; The actual implementation may vary based on the DBMS, so the data model is independent from the DBMS that is being used.
* {:.fragment} __entity__ - some _thing_ that we store data about, like a student, a dog bite incident, a pizza order, etc (essentially, a table)
* {:.fragment} an __attribute__ is the data that describes an entity, like netid, breed of dog, or type of crust (think: columns)
* {:.fragment} __entity identifier__ - an attribute or attributes that uniquely identify an instance of an entity (u no, like, primary 🔑)
* {:.fragment} an __instance__ of an entity is the actual attribute data of an entity (like a row in a table)
</section>


<section markdown="block">
## Attribute Values

An attribute can contain a single value... but also, you could imagine an attribute potentially having multiple values... &rarr;

* {:.fragment} a student may have an attribute called contact information which contains phone number, multiple email addresses, and or twitter handle (uh? wat?) 
* {:.fragment} it may look like this... 555-555-5555,abc123@nyu.edu,sql.is.my.life@nyu.edu,@sqlXallXday
* {:.fragment} this is a __multi value attribute__ <span class="fragment">and it's a baaad thing. Why?</span>
	* {:.fragment} searching on that particular attribute slower (no longer exact match, but sub)
	* {:.fragment} choosing only that piece of information is more difficult
	* {:.fragment} better to movie out into its own entity
</section>

<section markdown="block">
## Attribute Domains

__An attribute's value can be restricted to a set of possible values__ &rarr;

* {:.fragment} this is an attribute's domain
* {:.fragment} for example, a student's gpa should be a floating point number, a pie can be one of: small, medium, large, or [record breaking](https://imgur.com/gallery/YLFomNd)
* {:.fragment} it's essentially _type_

</section>

<section markdown="block">
## Entities Can be Related

__Entities can be related to each other. What are these relationship types agian?__ &rarr;

* {:.fragment} one to one
* {:.fragment} one to many
* {:.fragment} many to many
* {:.fragment} it's possible that either side can be zero too... unless
	* {:.fragment} a student can exist without a related course
	* {:.fragment} a course can exist without a related student
</section>

<section markdown="block">
## Weak Entities

A __weak entity__ is an entity that must be related to another entity, otherwise, it cannot exist.

* {:.fragment} for example a pizza order cannot exist with a customer
* {:.fragment} an exam can't exist without a course
* {:.fragment} int this case, these cases, the relationships are mandatory for one of the entities... and the other side can't be 0
* {:.fragment} think: foreign key, not null

</section>

<section markdown="block">
## Composite Entities

A __composite entity__ models the relationship between other entities.

* {:.fragment} for example... the relationship between a student and a course could be an _enrollment_
* {:.fragment} it's essentially the _join_ table in a many-to-many

</section>

<section markdown="block">
## Documentation, ER diagrams 

We can use an __Entity Relationship Diagram__ to show entities, their attributes and their relationships.

Some are picky about what's a true ER diagram, but for our purposes, it could refer to any style of ER diagram

* [wikipedia has a list of 'em](https://en.wikipedia.org/wiki/Entity–relationship_model)
* however, we'll stick to either:
	1. Chen
	2. _crow's feet_

</section>



<section markdown="block">
## Demo

Please [see the book](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/relational-database-design/9780123747303/B9780123747303000048.xhtml) for a more complete guide on ER diagrams. Here's we'll demo using:

1. Chen
2. Crow's Feet

To model...

* entities
* attributes
* relationships
* join tables


</section>

<section markdown="block">

## Normalization

__Normalization__ is the process of structuring a relational database to to:

1. {:.fragment} reduce redundant data / use less space
2. {:.fragment} improve quality and integrity of data
	* {:.fragment} make it easier to change data (avoid insert, update and delete anomalies)
	* {:.fragment} allow for growth, evolution of data (reduce the need for restructuring tables completely when new data is introduced)
	* {:.fragment} make referential integrity constraints easier to enforce
	* {:.fragment} create clear and easy-to-understand structure that closely models the situation that the data represents



</section>

<section markdown="block">
## Normalization History

This process was proposed by Edgar F Codd, in a paper called ["A Relational Model of Data for Large Shared Data Banks"](https://dl.acm.org/citation.cfm?id=358007)
{:.fragment}

* {:.fragment} Codd added more _normal forms_ in subsequent works
* {:.fragment} ... and eventually collaborated with Raymond F. Boyce
* {:.fragment} CJ Date's works also added to the interpretation of modeling data

</section>

<section markdown="block">
## Normalization _Controversy_

__As both relational databases and the process of normalization have evolved, some concepts are actively being debated__ &rarr;

* {:.fragment} for example, _atomicity_ is cited in the original work, but its meaning is actually up for interpretation 
	* {:.fragment} a string 'hello', can be decomposed into individual characters, but _should it?_
	* {:.fragment} ... a timestamp can be decomposed into year, month, day, hours, minutes and seconds)
* {:.fragment} does `null` violate a rule of only having a set data type for a column or is it accepted as a special _marker_ for denoting missing values


__With that said, keep praticality and the _goals_ of normalization in mind rather than a rigid adherance to rules or a philosophical viewpoint__
{:.fragment}
</section>

<section markdown="block">
## First Normal form (1NF)

__The first normal form specifies:__ &rarr;

* {:.fragment} no _repeating groups_
	* meaning of __repeating groups__ has evolved since original article
	* it _can_ mean... no repeated columns or columns that contain multiple values (_atomicity_)
	* basically, when none of a relation's domains (types of columns) have a set as a possible value
	* every row and col intersection only contains one value from the domain (again, atomic)
* {:.fragment} key attributes are defined and...
* {:.fragment} attributes depend on primary key




</section>

<section markdown="block">
## 1NF Continued

Another way to look at it is... 1NF is related to the __shape__ of a record type:

* {:.fragment} all rows must have same number of fields
* {:.fragment} atomicity - a single column can't have multiple values
	* {:.fragment} `123-456-7890, 555-555-5555`
	* {:.fragment} `alice butler`

Some additional attributes of 1NF:
{:.fragment}

* {:.fragment} ordering of columns or rows shouldn't be relevant
* {:.fragment} no duplicate rows 

</section>

<section markdown="block">
## 1NF or Not?

__A student takes many courses__ &rarr;

* {:.header} netid, first, course
* abc123, alice, ait
* , , nlp
* def456, bob, nlp
{:.fragment}
{:.table}

* {:.header} netid, first, course
* abc123, alice, ait,nlp
* def456, bob, nlp
{:.fragment}
{:.table}

* {:.header} netid, first, course1, course2
* abc123, alice, ait, nlp
* def456, bob, nlp, &nbsp;  
{:.fragment}
{:.table}

### Nope, none of these are in first normal form
{:.fragment} 

* {:.fragment} attributes should depend on primary key
* {:.fragment} there should be no _repeating groups_ / values should be _atomic_

</section>

<section markdown="block">
## 1NF or Not? Continued

__A student takes many courses__ &rarr;

* {:.header} netid, first, course
* abc123, alice, ait
* abc123, alice, nlp
* def456, bob, nlp
{:.fragment}
{:.table}

## Ok - this is 1NF, but...
{:.fragment}

* {:.fragment} note that primary key now has to be combination of course and netid
* {:.fragment} probably better as multiple tables (we'll see this later)

</section>

<section markdown="block">
## Second Normal Form (2NF)

A relation is in __second normal form__ if:

1. {:.fragment} it's in 1NF
2. {:.fragment} __AND__ there are no partial dependencies
	* {:.fragment} a __partial dependency__ is when a non-prime attribute has a _functional dependency_ on a part of composite primary key (or even on a candidate key)
	* {:.fragment} a __candidate key__ is a column or set of columns that can uniquely identify a row... but it is not necessarily _the_ primary key
	* {:.fragment} there can be multiple candidate keys in a table, but only one primary key
	* {:.fragment} if a table has a primary key that consists of only one column, it's already in 2NF

</section>

<section markdown="block">
## 2NF or Not?

__Student takes many courses; course has many students__ 

* {:.header} netid, first, last, course name, course number
* abc123, alice, cho, applied internet tech, 0480-003
* abc123, alice, cho, drawing on the web, 0380-001
* bcd456, bob, davis, drawing on the web, 0380-001
* cd78, carol, diaz, applied internet tech, 0480-007
* cde901, carol, evans, drawing on the web, 0380-001
{:.fragment}
{:.table}

1. {:.fragment} name a candidate key
	* {:.fragment} a composite key: `netid`, `course number`
	* {:.fragment} a composite key: `last`, `course number` (there would like be duplicate last names, tho!)
2. {:.fragment} name a part-key dependency
	* {:.fragment} `first` depends on `netid` or `last`
	* {:.fragment} `course name` depends on `course number`

</section>

<section markdown="block">
## 2NF Continued

__Break up composite keys, and move out keys and attributes that are functionaly dependent... into other tables__. __Will this work?__ &rarr;

* {:.header .colspan} student
* {:.header} netid (pk), first, last, course num (fk)
* abc123, alice, cho, 0480-003
* abc123, alice, cho, 0380-001
* bcd456, bob, davis, 0380-001
* cd78, carol, diaz, 0480-007
* cde901, carol, evans, 0380-001
{:.fragment}
{:.table}

* {:.header .colspan} course
* {:.header} course name, course num (pk)
* applied internet tech, 0480-003
* applied internet tech, 0480-007
* drawing on the web, 0380-001
{:.fragment}
{:.table}

* {:.fragment} __ugh... what's weird about this?__
* {:.fragment} primary key of student must be composite now
* {:.fragment} but we still have partial dependency!
* {:.fragment} ok - moar tables!

</section>

<section markdown="block">
## 2NF Now?


__Let's add one more table to move out that pesky `course num`__ &rarr;

* {:.header .colspan} student
* {:.header} netid (pk), first, last
* abc123, alice, cho
* bcd456, bob, davis 
* cd78, carol, diaz 
* cde901, carol, evans
{:.fragment}
{:.table}

* {:.header .colspan} student_course
* {:.header} netid (fk), course num (fk)
* abc123, 0480-003
* abc123, 0380-001
* bcd456, 0380-001
* cd78, 0380-007
* cde901, 0380-001
{:.fragment}
{:.table}

* {:.header .colspan} course
* {:.header} course name, course num (pk)
* applied internet tech, 0480-003
* applied internet tech, 0480-007
* drawing on the web, 0380-001
{:.fragment}
{:.table}


(essentially, we added a join table for __a _many to many_ relationship__) 
{:.fragment}

</section>
<section markdown="block">
## 2NF Addendum

__Note that 2NF isn't necessarily about _many to many_ relationships.__ &rarr; 

* {:.fragment} it just so happened that, because of the _many to many_ our first attempt was not quite correct
* {:.fragment} however, __if multiple duplicate students didn't show up__ in the `student` table 
	* {:.fragment} (that is, if we didn't have to model a student taking many classes)
	* {:.fragment} then our first attempt would have worked
	* {:.fragment} but it would have been restricted to a single course can have many students!

</section>

<section markdown="block">
## Third Normal Form (3NF)

A relation is in __third normal form__ if:

1. {:.fragment} it's in 2NF
2. {:.fragment} __AND__ it doesn't have any __transitive dependencies__ 
	* {:.fragment} no non-prime attribute is dependent on the primary key _through another_ non-prime attribute
	* {:.fragment} if there's only one non-key attribute and its in 2NF, then it's already in 3NF

</section>

<section markdown="block">
## 3NF or Not?

__I think I'm sensing a pattern here__....

* {:.header .colspan} course
* {:.header} course name, course num (pk), dept, dept description
* applied internet tech, 0480-003, CS, bits and stuff
* probability and statistics, 0235-002, Math, numbers and stuff
* applied internet tech, 0480-007, CS, bits and stuff
* drawing on the web, 0380-001, CS, bits and stuff
{:.fragment}
{:.table}

* {:.fragment} is `dept` a key?
	* {:.fragment} nope!
* {:.fragment} does `dept description` depend on course number _through_ `dept`?
	* {:.fragment} yes!
* {:.fragment} new table plz!


</section>


<section markdown="block">
## 3NF-ized

__OK, how about this?__ &rarr;

* {:.header .colspan} course
* {:.header} course name, course num (pk), dept_id
* applied internet tech, 0480-003, 2
* probability and statistics, 0235-002, 1
* applied internet tech, 0480-007, 2
* drawing on the web, 0380-001, 2
{:.fragment}
{:.table}

<br>

* {:.header .colspan} dept 
* {:.header} dept_id, name, dept description
* 1, Math, numbers and stuff
* 2, CS, bits and stuff
{:.fragment}
{:.table}

</section>

<section markdown="block">
## The _Pledge_ 

2NF and 3NF essentially deal with the __relationships between keys and non-keys__ &rarr;

* Via [Bill Kent's A Simple Guide to Five Normal Forms...](http://www.bkent.net/Doc/simple5.htm): 

* "[Every] non-key [attribute] must provide a fact about the key, the whole key, and nothing but the key."

__Does this cover 1NF, 2NF, and 3NF?__ &rarr;

* {:.fragment} 1NF: "the key" implies that the key _exists_
* {:.fragment} 2NF: "the whole key" is a reference to no partial dependencies
* {:.fragment} 3NF: "nothing but the key" means that it doesn't depend on non-key attributes

</section>

<section markdown="block">
## 4NF and 5NF

__Typically, 3NF is _good enough_ for preventing insert, update and delete anomalies__ &rarr;

With that said, there are additional normal forms:
{:.fragment}

* {:.fragment} __4NF__: 3NF + "a record type should not contain two or more __independent__ multi-valued facts about an entity"
	* `name`, `skillz`, `language`
	* where `skillz` and `langauge` are independent
	* (always seems like the answer is more tables, amirite?)
* {:.fragment} __5NF__: 4NF + "information content cannot be reconstructed from several smaller record types, i.e., from record types each having fewer fields than the original record" 

</section>


