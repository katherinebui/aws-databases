February 21, 2017: Aurora
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
- Aurora doesn't rely on build technology
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

Aurora doesn't use build logs, what does it use?
Aurora + MySQL: write in all 6 locations and apply it to locations
  it is a master, distributed to all locations
  write to a storage that understands and applies the redirect

preview Aurora + PostgreSQL?
(look at pricing for everything)
