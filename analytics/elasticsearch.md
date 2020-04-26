
- Example : In DynamoDB, you can only find by primary key or indexes
- With ElasticSearch, you can search any field, even partial matches
- It's common to use ElasticSearch as a complement to another databases
- It also has some usage for Big Data applications
- You can provision a cluster of instances
- Build-in-integrations: Amazon Kinesis Data firehose, AWS IoT, and Amazon cloudwatch logs for data ingestion
- Security through Cognito & IAM,KMS Encryption, SSL & VPC
- Comes with Kibana(visualization) & Logstash(log integration - ELK stack


5 pillars
- Operations : similar to RDS
- Security : Cognito ,IAM,VPC, KMS, SSL
- Reliability : Multi-AZ, clustering
- Performance : based on ElasticSearch project, petabyte scale
- Cost: pay per node provisioned ( similar to RDS )

- Remember : ElasticSearch = Search/Indexing
