##February 21, 2017: Aurora##
https://s3-us-west-1.amazonaws.com/architectureweeks/Database/SF+Februray+21-23%2C+2017/Database+S3.pdf

a new relational database engine, built from the group up to leverage AWS

Summary:
- mysql compilable with up to 5x better perforamce on the same hardware 100000 writes/sec 500000 reads/sec
- scalable with up to 54 tb in single db up to 15 read replicas
- high available, durable and fault tolerance custom ssd storage layer
(performance enhancement key to aurora)

- supports postgresSQL
- 15 read replicas, failover without data loss, encryption etc.

- performance insights for Amazon RDS
  - simplify monitoring the AWS management console

- db migration service
  - fully managed service for migration from on premises to aws cloud with minial downtime
  - migrates data to and from all widely used commercial and open source dbs
2 tools:
  - dms - move database to target from source
    - (database migration service)
  - schema conversion tool that converts source db schemas stored procedures and application code to a difference target format
    - shows where you can keep and where things might need to be changed
    - once conversation of schema. then use dms to actually do the migration

- Heterogenous migration?

How to compare MySQL to RedShift
- Redshift, column db - data warehouse workloads, optimize to load giant data in S3

Outside of performances with Aurora
- data is replicated 6 ways
- spread into 6 zones
- i.e. read replicas - less than 10 milsec lag
- while others depend on build logs and then applied to read replicas = could be gaps
- Aurora doesn't rely on bin technology
- From application perspective: faster, more accurate and more availability

Pricing between RDS and MySQL:
- Aurora = more expensive on config
- there is a pricing calculator

PostgreSQL: limited preview
Aurora + MySql: not yet available until later 2017?

Does a master have a cluster behind it?
if the master fails, will another master in the cluster will pick up?
in a cluster, only 1 master is active
a passive is not open for reads
if config in multi and adds active passover, it will when the active master fails?

Aurora doesn't use bin logs, what does it use?
Aurora + MySQL: write in all 6 locations and apply it to locations
  it is a master, distributed to all locations
  write to a storage that understands and applies the redirect

preview Aurora + PostgreSQL?
(look at pricing for everything)



------ What is new with Aurora? --------
combination of compatible relational dbs

- take MySQL and PostgreSQL to make it more enterprise

History:
- not much as changed in last 20 years
- even when you scale it, you're still replicating the same stack
- sharding, coupled at the application layer
- shared nothing, coupled at the SQL layer
- shared disk, couple at the caching and storage layer
- same replicating monolith stack

cloud native, allows you to scale out, durable
- scale out and distributed design
- serviced orientated architect leveraging
  - 6 copies of every data
  - look at different layers of db
able to integrate with other amazon web services

Amazon RDS: automate admin tasks
- Aurora is part of RDS, one of manage dbs

Aurora -
Do less work:
  - do fewer IOs
  - minimize network packets
  - cache prior results
  - offload the database engine

Be more efficient:
  - process asyn
  - reduce latency path
  - use lock-free data structure
  - batch operations together

when replicating db in storage system, not creating a new db, but attached to an existing one?
  - milsec update time between replica and master

(replicas - talking among other replicas during commits to make sure it's up to date)
(asyn group commits - request IO when something is written in the buffer, fill with log records, form for the rights)
  - wait for 4/6 commits, then will proceed, other 2 will check with others replicas and then catch up
  - 4/6 of active copies are moving forward

MySQL: thread base, kernel handling - can handle 500,000 concurrent sessions

Lock Manager: broken the lock up, in memory data structure (MySQL), broke it up so that can access in mutli locations for scaling

Cache read performance: catalog concurrency, NUMA aware scheduler, read views
None-cache read performance: smart scheduler, smart selector, logical read ahead (LRA)

insert performance: tries to remember where you are

speed of building indexes: build the leafs, and then branches of the tree

spatial index: need to store and reason about spatial data, supports spatial data types (points/polygon)

R-tree used in MySQL vs Z-index used in Aurora

always backing up: find the right versions of the right pages in the stream

more replicas:
  - automatic replacement of failed nodes
  - fail order
  - can fail order to same region or different

continuous backups:
  - take Roderic snapshots of each segment in parallel, stream the redo logs to amazon s3
  - backup happens continuously without performance or avail impact
  - at restore, retrieve the app segment snapshots and log streams to storage nodes
  - apply log streams

instant crash recovery:
  tradition: have to replay logs since the last checkpoint, typical 5 minutes between checkpoints, single threaded in mysql, requires a large number of disk accesses
  aurora: underlying storage replays redo records or demand as part of a disk read, parallel, distributed, async, no one line stream, no replay for start up

survivable caches: can restart where it is warm, do not have to refill memory from storage

faster fail-over: standard: detect where it fails (15-20 seconds), failsover: need to DNS propagation and map into a new IP address and db recovery = recovery time = LONG
  aurora: aurora aware: knows what secondary becomes the new primary, no need to wait for DNS propagation, about 40 seconds or less

PostgreSQL + Berkeley
owned by a foundation, open source
vs. MySQL is Oracle

PostgreSQ = good for SQL
high performance out of the box
OO and ANSISQL 2008 compatible
most geospatial features of any open source db
Most Oracle compatible open source db
Schema Conversion Tool automatic conversation rates are Oracle to PostgreSQL
(available later this year)

What does PostgreSQL compatible mean?
- easy to manage with Amazon RDS
- easy to load and unload
- cloud native security and encryption
- fully compatible with PostgreSQL now and for foreseeable future
  - not a compatible layer, native PostgreSQL

Roadmap:
- encrypt at rest, encrypt at transit, amazon VPC by default
- 2x faster than PostgreSQL
- all PostgreSQL features, All RDS for PostgreSQL extenstions
- AWS DMS supported inbounded
- up to 15 readable failover targets
- instant recoveries

RDS MySQL and PostgreSQL don't scale automatically










