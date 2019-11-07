---
layout: slides
title: "Strings and Arrays"
---

<script src="../../resources/js/table.js"></script>
<link rel="stylesheet" href="../../resources/css/data-table.css" type="text/css" media="screen" title="no title" charset="utf-8">


<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## Data Source for Examples

Again, we'll use the _adverse_ food events data: [CAERS](https://www.fda.gov/food/compliance-enforcement-food/cfsan-adverse-event-reporting-system-caers)

1. slides use 2014-2019 reports
2. [documentation of fields](https://www.fda.gov/media/97035/download)
3. sample data:

* {:.header} Report ID, CAERS Created Date, Date of Event, Product Type, Product, Product Code, Description, Patient Age, Age Units, Sex, MedDRA Preferred Terms, Outcomes
* 172940, 1/1/2014, , SUSPECT, DANNON DANNON LITE & FIT GREEK YOGURT CHERRY, 09,  Milk/Butter/Dried Milk Prod, , , , NAUSEA, Other Outcome
* 175277, 4/7/2014, 3/15/2013, SUSPECT, CHIA PLUS COCONUT CHIA GRANOLA, 05,  Cereal Prep/Breakfast Food, 15, year(s), F, BURNING SENSATION, Other Outcome
{:.fragment}
{:.table}

Some fields, such as `MedDRA`, contain comma separated lists.
{:.fragment}

</section>

<section markdown="block">
## Quick Table Layout

<pre><code data-trim contenteditable>
drop table if exists caers_event;
create table caers_event (
    caers_event_id serial,
    report_id varchar(255),
    created_date date,
    event_date date,
    product_type text,
    product text,
    product_code text,
    description text,
    patient_age integer,
    age_units varchar(255),
    sex varchar(255),
    terms text,
    outcomes text,
    primary key(caers_event_id));
</code></pre>
</section>



<section markdown="block">
## Importing Data

__Use copy to import data assuming download is in /tmp folder (note that is encoded in `LATIN1`__ &rarr;

<pre><code data-trim contenteditable>
copy caers_event (
	report_id, created_date, event_date, 
	product_type, product, product_code, 
	description, patient_age, age_units, 
	sex, terms, outcomes)
from '/tmp/CAERSASCII 2014-20190331.csv'
(format csv, header, encoding 'LATIN1');
</code></pre>
{:.fragment}

A quick check...
{:.fragment}

<pre><code data-trim contenteditable>
select * from caers_event limit 10;
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## Number of Characters

Our initial table creation had some open-ended text types. __How do we find out what the longest string is in a column__ &rarr;

`char_length` is a string function that gives back number of characters
{:.fragment}

<pre><code data-trim contenteditable>
select terms, char_length(terms) as length
from caers_event
where terms is not null
order by length desc
limit 1;
</code></pre>
{:.fragment}

(we can also use subqueries for this!)
{:.fragment}
</section>

<section markdown="block">
## A Side Note on Arrays


__Despite 1st normal form specifying that only one value can exist at at an intersection of a row and column...__ &rarr;

* {:.fragment} postgres has an [`Array` type](https://www.postgresql.org/docs/current/arrays.html)
* {:.fragment} Arrays are composed of a single type
* {:.fragment} columns can be declared as an Array: `col_name integer[]`
* {:.fragment} they can be indexed into using `[]`: `col_name[1]`
* {:.fragment} ⚠️ __the first element is at index 1__!
* {:.fragment} slicing syntax works, both sides inclusive: `arr[1:4]`
* {:.fragment} displayed as `{val1, val2, val3}`
* {:.fragment} can be created as a literal using `ARRAY` keyword:
	* `ARRAY['foo', 'bar', 'baz']`


</section>

<section markdown="block">
## Array Examples

__Let's see a few _actual_ examples of creating an Array and indexing into it__ &rarr;

<pre><code data-trim contenteditable>
-- an array literal
select ARRAY['foo', 'bar', 'baz'];
</code></pre>
{:.fragment}

<pre><code data-trim contenteditable>
-- first element plz
select (ARRAY['foo', 'bar', 'baz'])[1];
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## Number of Elements in an Array

__To retrieve the number of elements in an Array (its length), use the built in function, `array_length`.__ &rarr;

* {:.fragment} note that `array_length` has two arguments ✌️:
	* {:.fragment} the array to get the length of
	* {:.fragment} the dimension to count (typically 1, unless dealing with nested arrays)
* {:.fragment} `select array_length(ARRAY['foo', 'bar', 'baz'], 1);`

</section>


<section markdown="block">
## Breaking Strings Apart

__It may be useful to split a string into a collection of individual strings using some boundary character...__ &rarr;

For example we saw that the `terms` field has a comma separated list of values embedded in a single column as a string.
{:.fragment}

The following functions break a string up into an array
{:.fragment}

* {:.fragment} `string_to_array(text, delimiter)`
* {:.fragment} `regexp_split_to_array(text, pattern)`
</section>

<section markdown="block">
## `string_to_array`

__Let's see what `terms` looks like as an Array__ &rarr;

<pre><code data-trim contenteditable>
select string_to_array(terms, ',') from caers_event limit 5;
</code></pre>
{:.fragment}

We can even index into it to get the first value... &rarr;
{:.fragment}

<pre><code data-trim contenteditable>
select string_to_array(terms, ',') as t,
       (string_to_array(terms, ','))[1]  as first
from caers_event limit 5;
</code></pre>
{:.fragment}



</section>

<section markdown="block">
## Arrays in Queries

__We can work with Arrays in parts of our query other than the select list (for example, the where clause or in a group by)__ &rarr;

<pre><code data-trim contenteditable>
select product, description, patient_age, age_units, terms
from caers_event where (string_to_array(terms, ','))[1] = 'NIGHTMARE';
</code></pre>
{:.fragment}

<pre><code data-trim contenteditable>
select (string_to_array(terms, ','))[1], max(patient_age)
from caers_event group by (string_to_array(terms, ','))[1];
</code></pre>
{:.fragment}
</section>
{:.fragment}


<section markdown="block">
## Casing and Whitespace

__As you might expect, there are functions that can be used to transform a string's casing...__ &rarr;

* `upper`
* `lower`
* `initcap` 
* `trim` (`ltrim`, `rtrim`)
	* remove leading and trailing whitespace
{:.fragment}

<pre><code data-trim contenteditable>
select upper(description) from  caers_event;
</code></pre>
{:.fragment}

<pre><code data-trim contenteditable>
select trim('   hi   ');
</code></pre>
{:.fragment}


</section>

<section markdown="block">
## Aggregating Strings 

__Sometimes it's useful to aggregate all strings in a group into a single value__ &rarr;

* `string_agg(col_name, join_string`
* like `max`, `count`, etc. ... `string_agg` is an aggregate function
* it will concatenate all values in the group for a particular column
* here's a contrived example of concatenation...

<pre><code data-trim contenteditable>
select patient_age, string_agg(product, '|')
from caers_event group by patient_age order by patient_age;
</code></pre>
{:.fragment}

btw ... to join an array: `array_to_string`
{:.fragment}
</section>

{% comment %}
* no index out of bounds error
* `array_to_string`
* more about creating arrays / dealing with arrays in the context of strings?
{% endcomment %}

