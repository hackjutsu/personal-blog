# Database Genres
title:  Database Genres
date: 2015-12-10 10:58:00
tags:
- Database

---

Like music, databases can be categorized into genres onto one or more styles. The first chapter of *[Seven Databases in Seven Weeks](http://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)* leads us into the exploration of five main database genres.  In this section, I quote those descriptions from the book below.

<!--more-->

> **Relational Database**
> 
> The relational model is generally what comes to mind for most people with database experience. Relational database management system (RDBMSs) are set-theory-based systems implemented as two-dimensional tables with rows and columns. The canonical means of interacting with an RDBMS is by writing in Structured Query Language (SQL). Data values are typed and may be numeric, strings, datas, uninterpreted blobs, or other types. The types are enforced by the system. Importantly, tables can join and morph into new, more complex tables, beacuase of their mathematical basis in relational (set) theory.
>
> There are lots of open source relational databases to choose from, including [MySOL](https://www.mysql.com/), [H2](http://www.h2database.com/html/main.html), [HSQLDB](http://hsqldb.org/), [SQLite](https://www.sqlite.org/), and many others.


----------


> **Key-Value Database**
>
> The key-value (KV), as the name suggested, is a KV store pairs keys to values in much the same way that a map (or hashtable) would in any popular programming language. Some KV implementations permit complex value types such as hashes or lists, but this is not required. Some KV implementations provide a means of iterating through the keys, but this again is an added bonus. A filesystem could be considered a key-value store, if you think of the file path as the key and the file contents as the value. Because the KV moniker demands so little, database of this type can be incredibly performant in a number of scenarios but generally won't be helpful when you have complex query and aggregation needs.
>
> As with relational databases, many open source options are available. Some of the more popular offerings include [memcached](http://memcached.org/), [Voldemort](http://www.project-voldemort.com/voldemort/), [Redis](http://redis.io/) and [Riak](http://basho.com/products/).


----------


> **Columnar Database**
> 
> Columnar, or column-oriented, databases are so named because the important aspect of their design is that data from a given column (in the two-dimensional table sense) is sorted together. By contrast, a row-oriented database (like an RDBMS) keeps information about a row together. The difference may seem inconsequential, but the impact of the design decision runs deep. In column-oriented databases, adding columns is quite inexpensive and is done on a row-by-row basis. Each row can have a different set of columns, or none at all, allowing tables to remain sparse without incurring a storage cost for null values. With respect to structure, columnar is about midway between relational and key-value.
>
> In the columnar database market, there's somewhat less competition than in relational databases or key-value stores. The three most popular are [HBase](https://hbase.apache.org/), [Cassandra](http://cassandra.apache.org/), and [Hypertable](http://hypertable.org/).


----------


> **Document Database**
>
> Document-oriented databases store, well, documents. In short, a document is like a hash, with a unique ID fields and values that may be any of a variety of types, including more hashes. Documents can contain nested structures, and so they exhibit a high degree of flexibility, allowing for variable domains. The system imposes few restrictions on incoming data, as long as it meets the basic requirement of being expressible as a document. Different document databases take different approaches with respect to indexing, ad hoc querying, replication, consistency, and other design decisions. Choosing wisely between them requires understanding these differences and how they impact your particular use cases.
>
> The two major open source players in document database market are [MongoDB](https://www.mongodb.org/) and [CouchDB](http://couchdb.apache.org/).


----------


> **Graph Database**
>
> One of the less commonly used database styles, graph databases excel at dealing with highly interconnected data. A graph database consists of nodes and relationships between nodes. Both nodes and relationships can have properties--key-value pairies--that store data. The real strength of graph databases is traversing through the nodes by following relations.
>
> The most popular graph database is [Neo4J](http://neo4j.com/).


----------


Most of the problems we face could be solved by most of the databases we mentioned above. The question is less about whether a given database style could be shoehorned to model your data and more about whether it's the best fit for the problem space, the usage pattern, and the available resources.