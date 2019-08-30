---
layout: slides
title: "Data Modeling in MongoDB"
---

<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## Defining Relationships / Data Models

__In MongoDB, documents can be related to each other in two ways:__ &rarr;

* embedded (that is, one document contains one or more other documents)
* related (...one document references another document by id)
    * {:.fragment} seems familiar, eh?

</section>

<section markdown="block">
## Embedded

* __contains__ relationship
* all related info in same document since related docs are embedded
    * generally better performance
    * fewer queries 
    * perhaps fewer updates (update related data in single operation)?
    * use dot notation 
    * for queries: (`{"parent_prop.child_prop": "val"}`)


</section>

<section markdown="block">
## Use Embedded

__Embedded document use cases:__ &rarr;

* {:.fragment} contains relationship: one-to-many or one-to-one
* {:.fragment} fast reads are important

But...
{:.fragment}

* {:.fragment} maybe u don't want everything at once!
* {:.fragment} will more closely resemble your data usage
</section>

<section markdown="block">
## Use References

__References use cases__ &rarr;

* {:.fragment} many to many
* {:.fragment} want to reduce redundant data

But...
{:.fragment}

* {:.fragment} as u no! complexity!
* {:.fragment} more queries
</section>
