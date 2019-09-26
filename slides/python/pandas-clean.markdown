---
layout: slides
title: "Missing Data and Basic Transformations"
---
<section markdown="block" class="intro-slide">
# {{ page.title }}

### {{ site.vars.course_number}}-{{ site.vars.course_section }}

<p><small></small></p>
</section>

<section markdown="block">
## Missing Data

__The special value, `np.nan`, means missing data.__ &rarr;

* {:.fragment} btw, that's a floating point value!?
* {:.fragment} is seen through pandas to represent
	* {:.fragment} missing data
	* {:.fragment} also referred to as NA
	* {:.fragment} __sentinal__ value - easily distinguished from valid values
* {:.fragment} (btw, the reason for missing data should be investigated before determining _what to do with missing data_)

</section>

<section markdown="block">
## WAT Can be Done?

__If we have a bunch of `np.nan` values, what should be done with those values (if anything)?__ &rarr;

It depends on the context, right? ...  ok, well, what are the options then, regardless of context
{:.fragment}

First, two extremes:
{:.fragment}

1. {:.fragment} revisit data collection
2. {:.fragment} ignore

But also:
{:.fragment}

1. {:.fragment} set to some default value
2. {:.fragment} interpolate based on surrounding data

</section>

<section markdown="block">
## Ignoring It 🙈🙉

For a DataFrame, use `dropna`

* drops all rows containing NA value
* can pass `axis=1` to drop columns

```
df = pd.DataFrame(np.arange(12).reshape((3, 4)), 
	columns=list('abcd'))
df.loc[1, 'd'] = np.nan
df.loc[2, 'c'] = np.nan
df.dropna(axis=1)
```
{:.fragment}
</section>

<section markdown="block">
## Ignoring it More Precisely (for a DataFrame) 🎯


__Remember: on a Series, `.isnull` or `.notnull` produces a boolean Series__ &rarr;

1. {:.fragment} use  `.isnull` or `.notnull` on a column
2. {:.fragment} then filter by using the resulting Series


```
# (df is dataframe from previous slides)
df[df['d'].notnull()]
```
{:.fragment}
</section>

<section markdown="block">
##  Set to Value

__Maybe you just want to fill those pesky missing values with some default value 🔨
.__ 
&rarr;

* {:.fragment} use `fillna` on DataFrame
* {:.fragment} will fill with value passed in 
* {:.fragment} if `dict`, keys will match column names, and values will be used on those columns
* {:.fragment} (also, interpolation w/ keyword args same as reindex, like `method='ffill'`)

```
# using df from previous slides
df.fillna(0)
df.fillna({'c': 100, 'd': 123})
df.fillna({'c': 100, 'd': df['d'].mean()}) # 💪
df.fillna(method='ffill')
```
{:.fragment}

</section>

<section markdown="block">
## Transformation

The following can be used to a call function on: 

* `map` - every element in a Series
* `applymap` - every element in a DataFrame
* `apply` - go across an axis and call function on collection of values

</section>

<section markdown="block">
## `str` Accessor

Using `.str` on a series allows you to perform vectorized string operations!

* `lower` / `upper`
* `strip`
* `split`
* `replace`

...etc. (much like regular string methods)

</section>

<section markdown="block">
## Type Conversion

__Data frame columns have types (they're Series after all!)__

```
df.dtype
df.count()
df.info()
```
{:.fragment}

To convert from one type to another:

* {:.fragment} `astype('new type')`
* {:.fragment} `pd.to_numeric()`
* {:.fragment} `pd.to_datetime()`
* {:.fragment} (the last two allow ignoring type errors)
</section>
