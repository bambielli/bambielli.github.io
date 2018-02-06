---
title:  "Postgres Extension - CITEXT for Case Insensitive Columns"
date:   2016-12-28 09:44:00
category: til
tags: [postgres, databases, backend, CITEXT, case, insensitive, column, extension]
---

TIL about Postgresql extensions, particularly the CITEXT extension for making a case insensitive column in a table.

## The Use Case

I'm building out a user table for an app. I'd like a column of that table to be email (with a uniqueness constraint). I'd also like that data to be stored in a case insensitive manner.

Side note: the word `user` in Postgres is a reserved keyword, so you are unable to create a table named user. You can get around this constraint by wrapping the word user in quotes (i.e. "user") but then all queries to the table would need to reference the table in quotes. This seemed like an easy thing to forget, so I just pluralized my table name (along with the other tables in my database).

## The CITEXT Extension

Postgres has a column extension called CITEXT (stands for case-insensitive extension). This type allows one to query for the column in a case insensitive manner.

{%highlight sql%}
CREATE TABLE users (
    nickname CITEXT PRIMARY KEY,
    full_name TEXT NOT NULL
);

INSERT INTO users VALUES ( 'brian',  'Brian Ambielli'  );
INSERT INTO users VALUES ( 'Lauren', 'Lauren Ambielli' );

SELECT * FROM users WHERE nickname = 'Brian';
{%endhighlight%}

In the above scenario, the row that contains the value "brian" would be returned, even though the query was for `nickname = 'Brian'`. This is because the Postgres does a case-insensitive comparison of CITEXT columns by converting the query string and the value of the comparing column to lowercase using LOWER.

Data is not stored as lowercase; rows are just returned as if both the query and row value were lower case.

## Postgres Extensions

CITEXT is an example of a [postgres extension][pgext]{:target="_blank"}, of which there are many (see link). Extensions are enabled in a database by database manner, and are enabled by executing a CREATE EXTENSION statement in the database.

{%highlight sql%}
-- after connecting to database, execute the following statement.
CREATE EXTENSION IF NOT EXISTS citext;
{%endhighlight%}

This will create the CITEXT extension in the database to which you are connected if the extension hasn't already been created for the database. You can view all enabled extensions for a Postgres database with the `\dx` command.

## Is CITEXT Necessary for My Use Case?

Probably not. Querying a CITEXT column is slower than a VARCHAR or TEXT column, because of the underlying conversion of both the query string and the column value for the CITEXT row to LOWER. Per the docs, using CITEXT is slightly more efficient than calling LOWER in your queries explicitly to perform case-insensitive matching **in the database**.

Since my use case is for storing an email, and it is trivial to downcase the value entered by a user before submitting it to the database, it is probably better from a data cleanliness perspective to just continue with that strategy. This way I can guarantee that all email addresses are stored as lower case (along with other client-side email validation that I will do), and can just use a VARCHAR field to store email values for a row in my users table.

I didn't know about Postgres extensions before looking in to CITEXT, though, so this was a good exercise overall!

[pgext]: https://wiki.postgresql.org/wiki/Extensions
