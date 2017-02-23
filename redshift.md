## February 23, 2017: Redshift ##

Getting started with Amazon Redshift:
  - can migrate whole data center in several days
  - S3 = big data store
  - good at joins and complex queries
  - machine learning and cloud native
  - ($1k/TB/Year, ~ .25/hour)

In a nutshell:
  - relational data warehouse
  - massively parallel, petabypte scale
  - fully managed
  - hdd and ssd platforms

Use cases:
Tradition Data Warehousing:
  - Business reporting
    - about expanding capabilities and how easy it is to migrate to the cloud
  - advanced pipelines and queries
  - secure and compliant
  - bulk loads and updates
Log Analysis:
  - want to do it fast
  - log and machine IOT data
  - clickstream events and data
  - time-series data
  - i.e. Lyft using Redshift to find fastest routes (to reduce cost to riders)
  Redshift:
    - pay as you go model (for people with commitment issues)

Architecture:
  - leader node:
    - dimple sql endpoint
    - stores metadata
    - optimizes query plan
    - coordinates query executions
  - computes nodes
    - local column storage
    - parallel/distributed execution of all queries, loads, backups, restores and resize
    - looks at specific column

Benefits of Redshift:
  - only visit the blocks that make sense, not the whole entire table and look at all the different columns
    - dramatically less i/o
    - column storage
    - data compression
    - zone maps
    - direct attacked storage
    - large data block sizes
      = can store more and able to run queries faster
  - parallel and distributed
    - query, load, export, backup, restore, resize
    - i.e. want to resize, can resize and then charge, then shut down the resources
  - hardware optimize for I/P intensive workloads, 4gb/sec/node
  - enhanced networking, over 1 mil packet/sec/node
  - choice of store type
  - FAST... even faster?
    - 5x query improvement
  - vacuuming to optimize your cluster
  - reclaim space from deleted rows
  - pricing is simple
    - number of nodes * price/hour
    - no charge for leader node
    - no upfront cost
    - pay as you go
  - fully managed
    - continuous/incremental backups
      - multiple copies within cluster
        - provision the amount bought and mirror it, (2 turns into 6 tbs) for backup purposes
        - continuous and incremental backups to s3
        - continuous and incremental backups across regions
        - streaming regions
  - fault tolerance
    - disk failures
    - node failures
    - network failures
    - availability zone/region level disaster
      - prepared and will be open to another region
    - within seconds can detect failure, park the connections, replace the fault node within minutes, and reconnect + queries are re-submitted
    - what happens to the process itself and the network?
      - additional monitoring layer for the leader node and network
  - security is built in
    - load encrypted from s3
    - encrypt end to end
    - SSL to secure in transit
    - every block is encrypted with it's own key
      - there is a cluster key
    - Amazon VPC for network isolation
    - encryption to secure dta is rest
  - POWER
    - approx functions
    - user defined functions
    - machine learning
    - data science
 - large ecosystem
    - work with other organizations that are already partners (can certified)
- service orientated architecture
  - Kinesis to stream data
  - 95 services that can integrate well with each other

Query's life:
  - queues have slots where queries live
  - whenever query is not fast enough, it will be downgraded into the next queue
Redshift is changing this having a power start: all queries receive a power start, shorter queries benefit the most

Coming soon: Query monitoring rules
  - allows auto handling of runaway, poorly written queries
  - metrics with operators and values create a predictive

Data modeling:
 - single column
  - fast to load
  - fast to join/group by
 - compound
  - can be sorted
  - delete of rows
 - interleaved
  - equal weight is given to each column
  - better than scanning the whole table
  - queries that use different columns in filter
  - queries get faster the more columns used in the filter
  - 3 copies of your data

How to distribute data across the node:
  - even
    - first row will distributed to first node, second to second, etc.
    - hash rows to different nodes
    - other nodes are co-located and can be access and don't need to travel across table to find data
    - tables with no koings or group bys
  - key
    - map two compute nodes = wasting resources
    - key with commonality to distributed evenly
    (not optimal)
    - large fact tables
    - large distributed tables
  - all
    - all distributed, complete copy locally
    - med fact tables


Data loading options
  - S3 to Redshfit

(use multiple input files to maximize throughput) *review

transition for resizing:
  - during the resizing (downgrade or up), can read the old cluster
  - can read and write to new cluster

















