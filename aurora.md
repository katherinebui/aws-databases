Aurora
a new relational database engine, built from the group up to leverage AWS

5 important things:
- mysql compilable with up to 5x better perforamce on the same hardware 100000 writes/sec 500000 reads/sec
- scalable with up to 54 tb in single db up to 15 read replicas
- high available, durable and fault tolerance custom ssd storage layer
(missing 2)

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
