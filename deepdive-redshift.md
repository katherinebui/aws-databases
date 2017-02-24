## February 23, 2017: Deep Dive on Amazon Redshift ##
https://s3-us-west-1.amazonaws.com/architectureweeks/Database/SF+Februray+21-23%2C+2017/Database+S3.pdf

Leader node:
  - does the parsing, send query and it parses it
  - aws rewrites the query when it comes in
  - ie: inner join: table, next table, where clause
    - ripping out syntactic sugar
  - generate C++ code, in application and goes down to the computer nodes
    - how it picks up files, etc.
    - the compile step on leader node
  - postgresql tables on leader node as well

Zone map:
  - in memory data structure
  - restore min and max values for every data blocks
  - 1 meg chuck in memory
  - check inner memory before going into blocks

example) 4 blocks with data with SQL query
  - check zone maps to filter certain tables
  - then sort the data: zone maps look different
  - Redshift with sort keys find targets very fast
  - don't store the value of the key, not the z index

3 different type of sort keys:
  - regular sort key
  - interleave: allows you to apply equal weight to multiple column, doesn't matter what the first value is you are trying to filter by
  - most data in Redshift, time series data- make sort keys based on time stamp

Data Distribution: use when joins or group bys
  - Key: value is hashed, same value goes to same location (slice)
    - pick a key that will spread evenly
    - pick key ONLY when there will be a even distribution
    -
  - Even: Round robin, this picks a key for you if you don't have an even separated key
    - when key cannot produce an even distribution
  - All: full table data goes to first slice of every node
    - every record that is written in the disk is written to the node
    - mainly for joins
    - small and med size dimension
Goals:
  - dist data evenly for parallel processing
  - minimize data movement during query processing

Storage Deep Dive: Disks
  - the compute nodes have 2/3 more times storage than advertise
  - the size advertise is available to use
    - data in Redshift is mirror across the cluster
    - the moment you type commit, two type commit - data is written to two different compute nodes
    - TWO PARTITIONS
    - mirror can be access by remote
  - partitions: raw devices
    - local storage devises are ephemeral in nature
    - tolerant to multiple disk failures on a single node

Blocks:
  - Columns data is persisted to 1MB immutable blocks
  - 11 encoding types
  - Each block contains in memory data
  - Block properties:
    - small writes
      - pick up last block, clone, stick new value into it and then delete the block and write a new block
      - batch processing system, opt for processing massive amounts of data

Columns:
  - set of blocks, doubly linked list
  - SET OF BLOCKS = BLOCK CHAIN
  - logical structure accessible via SQL
  - blockchains exist on each slice for each column
  - all sorted and unsorted blockchains compose a column
  - Column properties include:
    - distributed key
    - sort key
    - compression encoding
  - column shrink and grow independently, 1 block a time
  - three system columns per table per slice for MVCC


Query:
  - collection of streams,
  - stream: a collection of segments
  - segments have steps
  - need to finish the first stream before moving onto the second stream
  - separate C++ programs

Query life cycle:
  - client to leader node
    - explains plans
    - generate all segment for the stream
    - produce some C++ code
  - then moves into the compute node
  - may need results to feed into next stream
  - takes the result and streams back up to leader node
    - maybe need some summation or an order by, applied limit
    - leader node will take final results and takes it back up and gives it to the client

Compute Nodes:
  - slices execute the query segments in parallel
  - executable segments are created for one stream at a time. when the segments of the stream are complete, the engine generates the segments for the next stream
  - when the compute nodes are done, they return the query results to the leader node for final processing
  the leader node merges the data in a single result set and addresses ant needed sorting or aggregation
  the leader then returns the results to the client
  - by design, want to try to use hardware as much as possible, so it runs faster

what happens when leader node goes down?
  - replace by Redshift
  - leader node doesn't really hold data, so it's easily to recover
  - 5-10 mins?
