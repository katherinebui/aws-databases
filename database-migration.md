Database migrations
https://s3-us-west-1.amazonaws.com/architectureweeks/Database/SF+Februray+21-23%2C+2017/Database+S3.pdf

What are DMS and SCT?
  - AWS Database Migration Services(DMS): easily and securely migrate and/or replicate your databases and data warehouses to AWS
  - AWS Schema Conversion Tool(SCT): convert your schema (EVERYTHING)

Modernize your db tier:
  - commercial to open-source
  - commercial to Amazon Aurora
Modernize your warehouse:
  - commercial to Redshift

Migrate:
  - migrate subsets of your db
Replicate:
  - create cross regions read replicas, run your analytics in the cloud, keep your dev/test and production environment sync

SCT is a tool you can download: data extracting agent
  - you can set it up and then extract data from your warehouse
  - can have mutli agents running, one working on an individual table
  - optimizing data to make it Redshift friendly, then it loads to S3

Why use DMS and SCT?
  - remove barriers to entry
  - near zero downtown
  - secure
  - easy to use but sophisticated
  - allows aws freedom
  - cost effective


