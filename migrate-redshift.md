## February 23, 2017: Migrating to Redshift ##

Massively parallel, shared nothing:
- top: leader node
  - SQL endpoint
  - stores metadata
  - query comes in and sends to computing nodes
- then connected to computing nodes: where the data is stored
  - gets data
- and that's connected to s3/dynamodb
  - sends data back up

Designed for I/O reduction

Slices:
  - core!
  - virtual compute nodes
  - split them out into virtual ones
    - unit of data partitioning
  - parallel query processing
  - facts about slice
    - data stored per slice as well
  - how to get parallelism
  - each work on independently and stores data independently from other slice

Data Distribution:
  - style is a table property which dictates how that tables data is distributed throughout the cluster
  - usually takes the longest
  - KEY: value is hashed, same value goes to same location (slice)
  - ASK: full table goes to first slice of every node
  - how data is sharded, if you don't know what to do, take the even
  - doing modulo to the slice, don't do date because that means all data from today goes to the same cluster

Data loading best practices:
  - drop CSV into s3 = easiest for Redshift
  - use at least as many input files as there are slices in cluster
  - cut file into 16 slices: with 16 input files, all slices are working so you maximize thoughput
  - COPY continue scale linearly as you add nodes

Schema design:
  - varchar lengths, make narrow as you can
  - buffers allocated based on declared column width
  - will just slow down process if lengths are too long
  - wider than needed columns mean memory is wasted
  - fewer fit into memory, increase likelihood of queries spilling to disk
  - there are tools to help check length of varchars

Redshift:
  - single row inserts and deletes are expensive
  - mark as deleted and then insert at the bottom
  - optimized for batch inserts
    the time to insert a single row in redshift is roughly the same as inserting 100,000 rows
  - want to do a delete? then do it in a batch, with some larger set of data

Column Compression:
  - auto compression
    - samples data automatically when COPY into an empty table
    - bake encodings in your DDL or use Create Table
  - analyze compression
    - data profile has changed
    - run after changing sort key
    - built in command

Mistake: load data and not do compression, DO COMPRESSION

Primary keys and Foreign Key Constraints - there are some (check slides)

User SCT for schema conversions - takes care of most of the things
