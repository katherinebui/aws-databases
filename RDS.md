##February 21, 2017: Amazon RDS##
https://s3-us-west-1.amazonaws.com/architectureweeks/Database/SF+Februray+21-23%2C+2017/Database+S3.pdf

- A choice of 6 db's
  - multi-engine support
- Provisioning and Effortless Scaling
  - handle higher load or lower usage
  - naturally grow over time
  - control flow
- Potential conflicts
  - sizing - How big is the db?
  - can change the scale of the server up or down
  - can scale up if anticipating work load
- Read Replicas (through console or api call)
  - bring data close to your consumer's applications in different regions
  - can find something that is closer to user to make things run faster
  - relieve pressure on your master node for supporting reads and writes
  - promote a read replica to a master for faster recovery in even of disaster
  - can redirect the replica has the master in case master is fucked up
- High availability Multi- AZ Deployments
  - can have multiple replicas and it will handle the payload for you
- Security and Compliance
  - different mechanisms for security
- Network isolation
- Amazon Virtual Private Cloud (Amazon VPC)
  - securely control network configuration
    - different constructs
    - subnets to isolate application components
    - who gets assess to your db? (private subnet)
    - can set up a VPN connection from corporate to RDS db
    - AWS direct connect: more consistent performance on network
    - when having application in VPC, use VPC peering (want to access over Internet, Internet gateway)
      - control traffic and subnets
- Database instance IP firewall protection
  - security groups: a collection of access rules
  - source: IP address
  - all instances of security group has access to the database
- IAM governed access
  - You can use AWS Identity and Access Management (IAM) to control who can perform actions on RDS
  - who can run the admin stuff
  - users that are authenticated through the database
- Database encryption
  - At Rest Encryption for all RDS Engines AWS Key Management Service (KMS)
  - spin up a service and gets a key to encrypt the data
  - the keys are encrypted are wrapped in a master encrypt key (envelope)
  - two tiered key hierarchy
- Compliance?
- Standard monitoring
  - keep eye on database performance, every minute
  - can use metric to build alarms, set different thresholds to notify important events
- Enhance monitoring
  - up to one second regularity
  - in order to capture, need to install agent to capture the metrics
  - open to all 6 db engines


