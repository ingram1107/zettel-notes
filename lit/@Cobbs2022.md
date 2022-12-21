---
title: Persistence Programming
authors: Archie L. Cobbs
year: 2022
date: 2022-09-17 22:01
tags: [literature, database]
url: https://cacm.acm.org/magazines/2022/9/263808-persistence-programming/fulltext
---

Persistence Programming, as defined by the article author, Cobbs, is where data
are store in a *persistent* manner where it can be read by any form of software
even if they are written in a completely different programming language than
what the data was input or output. Cobbs gave an example: we can be certain one
is doing persistence programming when they create an *XML document* and send it
over the network, at where the receiver will *open and read* from it.

He pointed out the limitation of the current programming languages as they
provide limited support for persistence programming such as serialisation of
basic data types. Programmers themselves are often required to build their own
component and/or library to support persistence programming. The author claimed
that this has resulted in the creation of databases and their subsidiaries such
as SQL and the libraries that allow programs to send SQL queries over a network
and for data retrieval. Then the programmers are expected to put up a bridge
between the programming language world and the practical world of SQL tables,
query design, performance, transactions, and network failures, ending up having
the programmer to succumb to the demands and requirements of the database
technology.

The author reviewed the functionalities of a database and its practicalities:
store data values into raw bits which can be read by both programming languages
and database, key-value mapping, efficient key sorting for iteration and
querying, efficient key lookup with hash tables (unsorted data) or balanced
trees (for sorted data) and allow [concurrent access](../202202011815.md)#
(which emphasises heavily on data atomicity, isolation, consistency and
durability).

By pointing out the shortcomings of JPA (Java Persistence API, later renamed to
Jakarta Persistence), the current popular library for persistence programming in
Java, Cobbs identified several characteristics for a genuine persistence
programming language should look like.

Firstly, there should be a **universal encoding for the basic types** such as
primitive, wrapper, array type, and the common types such as `String` and `Date`.
In this way, there will be no compatibility issue unlike the current state of
JPA where some Java basic types doesn't match their respective SQL types. For
example, `DATETIME` in SQL is not the same as `java.util.Date`, and `NaN`
floating point values are usually unsupported in SQL.

Now going down further to the limitation of JPA of **utilising Java classes and
interfaces**. Since JPA has to map Java class hierarchies onto rectangular
tables in SQL, unused columns or extra joins are possible. Additionally, JPA is
partly incompatible with Java generics as its properties must be concrete due to
its limited inheritance from interfaces. A proper persistence programming
language should be able to maximise the efficiency of class hierarchies and full
compatibility with interface inheritance and generics.

Furthermore, a **precise, synchronous notifications for both object life cycle
and individual property change events** should be implemented. This will result
in a more elegant way to detect non-local changes within objects via references.
For instance, when a colour property in any child tree node changes, it should
notify its parent node immediately. Such notifications should be done in a
per-property manner instead of just per-entity manner which is what the current
JPA implementation of the notification handling.

Instead of relying on internal balanced trees for indexing, Cobbs argued that it
is possible to derive an index using any combination of **a secondary data
structure** from primary data. And when there are changes in the primary data,
notifications should be generated from it and updates the secondary data
structure respectively. This could be especially helpful if the frequent query
or access values are derives from the primary data and/or hard or impossible to
index as a simple query in SQL. Those values could be updated whenever there is
a newly inserted primary data, and be stored onto a secondary data structure
where the programmer could later retrieve the value from there. From here we
could see why it is necessary to implement a *synchronous notification system*
for property change events.

**Enforcing efficient validation**, that is maintenance and verification of
required invariants or validation constraints, is an essential part of a
persistence programming language. In order to do so, it needs to have code that
can verify the constraint and firing notification when a re-checked is necessary
for the constraint as a result of a change in the associated data. Cobbs
proposed a verification check mechanism that should be deferred until the
transaction commit time unless specified earlier unlike those implemented in JPA
and Java where phantom foreign-key violations could be triggered and the checks
happened before the invariants have been re-established. Programmers should be
able to manually assigned which field should be checked when changes happened.

As the programming language designed around persistence programming, it improves
the **transparency of a query**, so one could spot its efficiency rather quickly
in programming language sense instead of from the database administrator's point
of view. It is especially true if the data types to store the data persistently
have the same encoding across the programming languages and the languages used
by the database. The programmers would be able to find out whether such query is
efficient by just looking at it.

The programming language should be able to handle **schema migrations** from
*structural* and *semantic* changes with little overhead. Structural change as
defined by Cobbs, is the change that made to the actual data format or layout,
thus making `ALTER TABLE` necessary for the operation as the database needs to
modify the table itself or its column. Semantic change on the other hand, is the
change that are the corresponding "fix-ups" to the data, which determine how the
data should present or represent in the table or column. For such functionality
to be effective, the underlying database should be able to support multiple
schemas at a time and allow rolling schema migrations, that is the objects
stored could be upgraded over time or on demand without needing drastic
structural reorder command such as `ALTER TABLE`. Those schemas will be tracked
automatically in the database and verification will be done against what the
code expects at the start of each transaction. These features allow the
structural changes to be automated and improve the comprehensibility of the
codes that initiate semantic changes.

Cobbs envisioned a more precise way to define what data to copy and retain as
offline data after the closing of the transaction by **using an in-memory
mini-database**. The querying on the offline data should be identical to the
querying directly on the database itself, which means that secondary index data
should be retained as well other than just having primary data. We could expand
this particular utility to include database designs that emphasise efficient
snapshots (e.g., log-structured databases), where the in-memory mini-database
could be used as a read-only, memory-mapped view into a snapshot. Compare to a
rather unsafe, inefficient approach from JPA on handling the data collection
where one could touch it when the transaction is still loading and lost after
JPA clear its online cache, this is a better implementation.

Transferring database information via network could be rather easy as the
database will be treated as **simple sorted key-value pairs** and with
**in-memory database including its schema-tracking information**. The database
will be converted to Java objects correspondingly, and be serialised and sent
over the network. The receiving end will be able to open and read the database
just as reading a bunch of Java objects, and since the schema information is
also included, schema migrations will happen automatically.

For such, the author developed [Permazen](https://github.com/permazen/permazen),
an [open source](../202110161031.md)# project that try to implement the
described earlier concept of persistence programming.
