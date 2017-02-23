## February 22, 2017: NoSQL ##
https://s3-us-west-1.amazonaws.com/architectureweeks/Database/SF+Februray+21-23%2C+2017/Database+S3.pdf

Data volume since 2010:
90% of data of stored data generated in last 2 years

Over 40-45 years from taking notes to relational dbs (queries)

Look at the data and look at the client = pick your dbs

Difference between NoSQL vs. Relational DB's:
Traditional SQL:
  - single db that has master (+ slaves)
  - when need visibility and read = need primary and secondary, scale up!
NoSQL:
  - organize in ring or shard
  - cluster of dbs
  - no upper limit
  - in this design: relationship not with a single db, more of relations with shard (in the ring, many db) = low latency
  - not good for joins
  - key is to avoid joins and query it in a way to break down data

Example:
SQL:
  - products, breaking down to separate tables
  - need to know each rows need certain conditions
NoSQL:
  - can access without certain attributes
  - (like JSON doc)

Why NoSQL?
SQL:
  - master + slave, primary, secondary
  - scale up, but you reach a max
  - optimized for storage
  - ad hoc queries, scale vertically, food for OLAP
  - each table has it's own schema
NoSQL:
  - optimized for computer
  - denormalized/ hierarchical
  - instantiated views
  - scale horizontally
  - built for OLTP at scale

NoSQL = Cassandra, Mark Logic, Amazon DynamoDB, Couchbase, MongoDB

Amazon DyanmoDB:
  - fully managed: you can create it in the console (just need to reach  out to set $$)
  - fast, consistent performance/latency
  - high scalable
  - flexible
  - event driven programming
  - find grained access control

How to do things (with DyanmoDB):
  - Create a table
    - table name is set in advance and can't change
    - add a date
    - partition key: MOST IMPORTANT - this tells where data is stored
      - uses hashing scheme
    - provisioned capacity set at 5 reads and 5 writes
  - loaded data items into table
  - table is distributed in 3 availability zones
  - attributes have name and values (tuples)
  - option: can have a sort key: an index, the field used to sort data on disk (binary/string/etc)
    - way to sort your data
  - structure: nothing, just need a partition sort key
    - three different sections in the table
    - partition key: mandatory for data distributions

Global secondary index (GSI) : on line index
  - alternate partition (+sort) key
  - index is across all table partition key
  - can remove and added whenever
The way everything works, capacity and consumptions: all about capacity

GSI almost like second db, appear in the console, but might be stored in db as separate tables

Projected attributes: took items at the base table and replicated into the index
(keys only, or name attributes up to 20 by name = projected attrs because chosen to copy over)
Copying of table is done async

How do GSI update work?
  - writes to the base table are accepted
  - set on queue to add to the index to GSI

Local secondary index (LSI):
  - alternate sort key attributes
  - is like having an index (sorting db on disk and can easily jump to data)
  - index is local to a partition key
  - need to be created at the beginning and can't change
  - limit of 5
  - need to figure out/plan out LSI i the beginning
  - when you want a consistent read
  - the data is on the same partition
  - total of 11/12 queries for the db

Lambda function:
  - C sharp, Java, etc.
  - write a function that is container and operate on input
  - s3, can use to keep two tables in sync










