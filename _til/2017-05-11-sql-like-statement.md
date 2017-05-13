---
title:  "Pattern Matching in SQL"
date:   2017-05-11 20:59:00
category: til
tags: [sql, mysql, postgres, backend]
---

TIL how to properly write a SQL statement using the `LIKE` operator, including how to use wildcards to match various patterns.

I spent the day working on a Java service to pull data from our Business Intelligence database (MySql). The data in which I was interested involved a string column (name) and an integer column (id). I wanted to write a query that matched **any name OR any id** that started with a prefix that the user input to the service.

The [sql `like` operator][like]{:target="_blank"} allows you to write a sql query of the format **"Give me results that contain a particular pattern."** You can customize the pattern to match on either prefixes, postfixes, substrings, or other more complicated patterns.

For example:

{% highlight sql%}

SELECT * FROM Table WHERE name LIKE '%abc%';

{% endhighlight %}

The query above will return rows where the name contains the substring 'abc'. I believe mysql is converting the integer column to a string to perform the matching under the hood.

NOTE: I also learned that `varchar` and other character string columns will be matched **in a case insensitive manner**, while `varbinary` and other binary columns will match patterns **in a case sensitive manner**. Something to keep in mind when designing your tables!

### Wildcards

There are two wildcards that can be used in your patterns

- `_` matches on a single character

- `%` matches on any number of characters (including 0)

You can use `_` and `%` in the same pattern to perform complex matches. Here are a few more examples:

{% highlight sql %}
SELECT * FROM Table WHERE name LIKE 'abc%'; -- matches all names that start with abc

SELECT * FROM Table WHERE name LIKE '_abc%'; -- matches all names where the characters abc are the second third and fourth characters in the name

SELECT * FROM Table WHERE name LIKE 'a_%_'; -- matches all names that are at least three characters in length and start with a.

SELECT * FROM Table WHERE name LIKE '1%3'; -- matches all names that start with 1 and end with 3
{% endhighlight %}

### Solving my Problem

I needed to write a query that returned all rows where either the name or id column started with a prefix that the user passed. The statement that got me there was the following:

{% highlight sql %}

SELECT DISTINCT * FROM Table WHERE name LIKE ':userInput%' or id LIKE ':userInput%' ORDER BY id;

{% endhighlight %}

I made sure to add a distinct constraint to prevent duplicates from being returned, and ensuring sorted order by id. These were things that our service was doing in application code previously... doing these types of sorting and filtering operations in the database makes much more sense.

EDIT: Well we switched from `MySql` to `postgres`, and I realized that the `like` operator performs a case-sensitive search in postgres regardless of the column type! If you want case insensitivity in your searches in `postgres`, use the [`ilike` operator instead.][ilike]{:target="_blank"}

[like]: https://dev.mysql.com/doc/refman/5.7/en/pattern-matching.html
[ilike]: https://www.postgresql.org/docs/9.2/static/functions-matching.html
