---
layout: slides
title: "MongoDB Aggregation"
---

<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>



<section markdown="block">
## Aggregation

Like `GROUP BY` in relational databases and the `groupby` method in pandas, __MongoDB also supports performing aggregation functions on groups of data (documents instead of rows, of course!)__.

There are a few ways to do this in MongoDB:

1. {:.fragment} Aggregation Pipeline
2. {:.fragment} Single Purpose Aggregation Operations
3. {:.fragment} Map-Reduce

We'll focus on 1: the aggregation pipeline.
{:.fragment}

</section>

<section markdown="block">
## Aggregation Pipeline

__The aggregation pipeline is a multi-stage process that transforms documents into an aggregated result.__ &rarr;

* {:.fragment} the stages and order of execution are determined by the user
* {:.fragment} the output from one stage becomes the input for the next stage

To start an aggregation, call the `aggregate` function on a collection:
{:.fragment}

<pre><code data-trim contenteditable>
db.collectionName.aggregate(...)
</code></pre>
{:.fragment}

Pass in an Array containing a sequence of aggregate operations.
{:.fragment}

</section>

<section markdown="block">
## $match

__The `$match` operator filters documents__ &rarr;

* {:.fragment} acts like standard `find` query
* {:.fragment} similar to a combination of `HAVING` and `WHERE` in sql
* {:.fragment} value is query object

<pre><code data-trim contenteditable>
{$match: {city: "Brooklyn"}}
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## $group

__The `$group` operator creates distinct groups as separate documents__ &rarr;

* {:.fragment} similar to `GROUP BY` in sql
* {:.fragment} value includes the following keys:
	* `_id`: the field to group by
	* the name of this field is prefixed with a dollar sign and quoted as a string
	* this syntax references the _value_ in a field from the original document
	* any number of additional fields in the document
	* with each field potentially having an accumulator operation:`$sum`, `$avg`, `$max`, `$last`, `$push` 


<pre><code data-trim contenteditable>
// counts the number of listings by neighborhood
{$group: {_id: "$neighbourhood", listingCount: {$sum: 1}}}
</code></pre>
{:.fragment}

{% comment %}
remove italics_
{% endcomment %}
</section>

<section markdown="block">
## Accumulator Operators

__There's a [long list of accumulator operators](https://docs.mongodb.com/manual/reference/operator/aggregation/group/#accumulators-group)__ ....you'll notice the similarities between these and their corresponding aggregate functions in sql:

* {:.fragment} `$sum`: "$fieldName" / value - sums the values in fieldName
* {:.fragment} `$avg`: "$fieldName" / value - calculate the mean of the values in fieldName
* {:.fragment} `$max`: "$fieldName" / value - finds the max value in fieldName
* {:.fragment} `$last`: "$fieldName" / value - gives the last fieldName value in group
* {:.fragment} `$push`: "$fieldName"

</section>

<section markdown="block">
## $match Details

__Unlike sql `SELECT`, the order of the stages in a call to `aggregate` can be specified! 😲__ &rarr;


* {:.fragment} try to place `$match` operations earlier... __why__ &rarr;
* {:.fragment} $match reduces the total number of documents to be processed 
* {:.fragment} which means that later stages won't have to deal with large volumes of documents!

</section>

<section markdown="block">
## Example Data

__Using "scraped" `listings` data from [insideairbnb.com](http://insideairbnb.com/get-the-data.html), we can practice some aggregations__  &rarr;

* {:.fragment} download the latest `listings` for New York City
* {:.fragment} `mongoimport --headerline --type=csv --db=test --collection=reviews --file=./listings.csv`

</section>


<section markdown="block">
## Example Query 

__Count the number of listings per neighborhood, not in the city of Brookyln. Exclude counts less than 10.__

<pre><code data-trim contenteditable>
db.listings.aggregate([ 
	{$match: {city: "Brooklyn"}}, 
	{$group: {_id: "$neighbourhood", listingCount: {$sum: 1}}}, 
	{$match: {listingCount: {$lt: 10}}}
])
</code></pre>

</section>
