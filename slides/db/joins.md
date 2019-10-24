---
layout: slides
title: "Joins"
---
<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## Joins

__A relational algebra operation implemented in SQL... that combines columns from one or more tables__

</section>

<section markdown="block">
## Genre Table

<pre><code data-trim contenteditable>
CREATE TABLE genre (
	genre_id integer,
	name varchar(20),
	description text,
	PRIMARY KEY (genre_id)
);
</code></pre>
</section>

<section markdown="block">
## Genre Data

<pre><code data-trim contenteditable>
 genre_id |      name       |                description
----------+-----------------+-------------------------------------------
        1 | Comedy          | LOL
        2 | Drama           | Such feels.
        3 | Fantasy         | Swords. And maybe fairies too.
        4 | Horror          | Ghosts, goblins, and other spooky things.
        5 | Science Fiction | Robots and stuff.
        6 | Super Hero      | Mostly capes.
        7 | Thriller        | Close. Your. Eyes.
</code></pre>

</section>

<section markdown="block">
## Movie Table

<pre><code data-trim contenteditable>
CREATE TABLE movie (
	movie_id serial,
	title varchar(50) NOT NULL,
	year smallint NOT NULL,
	runtime smallint NOT NULL, 
	genre_id integer REFERENCES genre (genre_id),
	PRIMARY KEY (movie_id)
);
</code></pre>
</section>

<section markdown="block">
## Movie Data

<pre><code data-trim contenteditable>
 movie_id |                 title                  | year | runtime | genre_id
----------+----------------------------------------+------+---------+----------
        1 | Alphaville                             | 1965 |      99 |
        2 | La Montaña Sagrada (The Holy Mountain) | 1973 |      99 |
        3 | Dune                                   | 1984 |     136 |        5
        4 | Point Break                            | 1991 |     122 |        7
        5 | Strange Days                           | 1995 |     145 |        5
        6 | 2046                                   | 2004 |     122 |        2
        7 | Hellboy                                | 2004 |     122 |        6
        8 | Los Abrazos Rotos (Broken Embraces)    | 2009 |     128 |        7
        9 | Blade Runner 2049                      | 2017 |     163 |        5
       10 | Wonder Woman                           | 2017 |     141 |        6
       11 | Shape of Water                         | 2018 |     123 |        3
</code></pre>
</section>
<section markdown="block">
## Cross Join

__Cartesian product__

* {:.fragment} all rows from one table combined with each row from another table
* {:.fragment} results in n * m rows

<pre><code data-trim contenteditable>
SELECT * FROM movie 
	CROSS JOIN genre;
</code></pre>
{:.fragment}

</section>

<section markdown="block">
## Inner Join

__Combines rows from one table and another based on matching column values__ &rarr;

* {:.fragment} condition for matching column is the join predicate
* {:.fragment} _commonly_ used!


<pre><code data-trim contenteditable>
SELECT title, name 
	FROM movie 
	INNER JOIN genre on movie.genre_id = genre.genre_id;
</code></pre>
{:.fragment}

... can also be done with `WHERE` clause (implicit join)
{:.fragment}

<pre><code data-trim contenteditable>
SELECT title, name 
	FROM movie, genre 
	WHERE movie.genre_id = genre.genre_id;
</code></pre>
{:.fragment}
</section>


<section markdown="block">
## ⚠️ Here There Be Nulls! 

__What happened to the `movie` rows that had a `null` `genre_id`__?

* {:.fragment} they weren't included in the results!
* {:.fragment} that implies that when a column that can have a `null` value is used in the join predicate, some rows may be omitted from the query result

</section>
<section markdown="block">
## Other Alternatives

__An `INNER JOIN` where the join predicate involves equality can be written with a `USING` clause__ &rarr;

<pre><code data-trim contenteditable>
SELECT title, name 
	FROM movie 
	INNER JOIN genre USING (genre_id);
</code></pre>
{:.fragment}

A __natural join__ performs an inner join implicitly on matching column names (v risk, tho! ⚠️)
{:.fragment}

<pre><code data-trim contenteditable>
SELECT * FROM movie NATURAL JOIN genre;
</code></pre>
{:.fragment}


</section>

<section markdown="block">
## OUTER JOINS

__Same as inner, but include everything in the first (left) table: `LEFT OUTER JOIN`__ &rarr;

<pre><code data-trim contenteditable>
SELECT * FROM movie LEFT OUTER JOIN genre ON movie.genre_id = genre.genre_id;
</code></pre>

__Same as above, but include everything in second (right) table: `RIGHT OUTER JOIN`__ &rarr;

<pre><code data-trim contenteditable>
SELECT * FROM movie RIGHT OUTER JOIN genre ON movie.genre_id = genre.genre_id;
</code></pre>

__Both! `FULL OUTER JOIN`__ &rarr;

<pre><code data-trim contenteditable>
SELECT * FROM movie FULL OUTER JOIN genre ON movie.genre_id = genre.genre_id;
</code></pre>

</section>

<section markdown="block">
## Self Joins

__Which movies were made in the same year, shown as pairs?__ &rarr;

<pre><code data-trim contenteditable>
SELECT a.movie_id, a.title, b.movie_id, b.title, a.year
FROM movie as a INNER JOIN movie as b ON a.year = b.year
-- prevents "duplicate" rows
WHERE a.movie_id < b.movie_id
ORDER BY a.year, a.movie_id, b.movie_id;
</code></pre>
</section>

<section markdown="block">
## Table Relationships

1. 1 to many
2. many to many
3. 1 to 1

__Where would the ids go to implement all of these relationships?__ &rarr;
</section>
