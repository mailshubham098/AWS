> #### [Amazon Kinesis](https://docs.aws.amazon.com/kinesis/?id=docs_gateway)
- Amazon Kinesis makes it easy to collect, process, and analyze video and data streams in real time.

#### Amazon Kinesis Data Streams
- collect and process large streams of data records in real time.
- create data-processing applications, known as Kinesis Data Streams applications.
- great for real-time big data
- Great for stream processing frameworks  (like spark,NiFi etc)
- alternate for apache kafka
- Data is automatically replicated to 3 AZ
- Use cases
  - Accelerated log and data feed intake and processing
  - Real-time metrics and reporting
  - Real-time data analytics
  - Complex stream processing

- offerings
  - Kinesis streams : low latency streaming ingest at scale
  - kinesis Analytics : perform real-time analytics on streams using SQL
  - Kinesis firehose : load streams to S3,Redshift, ElasticSearch

- flow
  - source(click streams/IoT/Metrics&logs) -> Amazon Kinesis Streams -> Amazon Kinesis Analytics -> Amazon Kinesis Firehose -> Sink (S3/redshift/elastic search)

Data Streams Terminology
- Kinesis Data Stream :  set of shards
- Data Record : unit of data stored in a Kinesis data stream. Data records are composed of a sequence number, a partition key, and a data blob, which is an immutable sequence of bytes
- Retention Period : length of time that data records are accessible after they are added to the stream
- Producer : put records into Amazon Kinesis Data Streams
- Consumer : get records from Amazon Kinesis Data Streams and process them
- Kinesis Data Streams Application :  is a consumer of a stream that commonly runs on a fleet of EC2 instances.
- Shard : uniquely identified sequence of data records in a stream.A stream is composed of one or more shards, each of which provides a fixed unit of capacity.
- Partition Key : used to group data by shard within a stream
- Sequence Number : Each data record has a sequence number that is unique within its shard
- Server-Side Encryption
- Application Name
- Kinesis Client Library


Kinesis streams overview
- Streams are divided in ordered Shards/Partitions
- Data retention is 1 day by default , can go upto 7 days
- Ability to reprocess/replay data
- Multiple applications can consume the same stream
- Real-time processing with scale of throughput
- Once data is inserted in Kinesis, it can't be deleted (immutability)

Kinesis Streams Shards
- One stream is made of many different shards
- 1MB/s or 1000 msgs/sec at write per shard
- 2MB/s at read per shard
- Billing is per shard provisioned , can have as many shards as you want
- the number of shards can evolve over time(re-shard/merge)
- Records are ordered per shard

AWS Kinesis API - Producers
- PutRecord API + Partition key that gets hashed
- The same key goes to the same partition( helps with ordering for a specific key)
- Messages sent get a "sequence number"
- Choose a partition key that is highly distributed (helps prevent "hot platform")
- ProvisionedThroughputExceeded if we go over the limits

AWS Kinesis API - Exceptions
- ProvisionedThroughputExceeded Exceptions
  - happens when sending more data(exceeding MB/s or TPS for any shard)
  - Make sure you don't have a hot shard ( such as your partition key is bad and too much data goes to that partition)
- Solution:
  - Retries with backoff
  - Increase shards ( scaling )
  - Ensure your partition key is a good one

AWS Kinesis API - Consumers
- Can use a normal consumer (CLI, SDK etc)
- Can use kinesis client lib ( java, nodejs etc)
  - KCL uses DynamoDB to checkpoint offsets
  - KCL uses DynamoDB to track other workers and share the work amongst shards
  
#### Kinesis Data Firehose
-  fully managed service for delivering real-time streaming data to destinations like
  - Amazon S3
  - Amazon Redshift
  - Amazon Elasticsearch Service (Amazon ES)
  - Splunk

Key Concepts
  - Kinesis Data Firehose delivery stream
  - record
  - data producer
  - buffer size and buffer interval

Data Flow
  - Data source -> Firehose -> S3 bucket
  - Data source -> Firehose -> intermediate S3 bucket -> Redshift cluster
  - Data source -> Firehose -> Elasticsearch cluster
  - Data source -> Firehose -> Splunk

Data transformation
- Kinesis Data Firehose can invoke your Lambda function to transform incoming source data and deliver the transformed data to destinations.
- Kinesis Data Firehose supports a Lambda invocation time of up to 5 minutes

#### Amazon Kinesis Video Streams


##### other points

- A new shard iterator is returned by every GetRecords request (as NextShardIterator), which you then use in the next GetRecords request (as ShardIterator). Typically, this shard iterator does not expire before you use it. However, you may find that shard iterators expire because you have not called GetRecords for more than 5 minutes, or because you've performed a restart of your consumer application
- If the shard iterator expires immediately before you can use it, this might indicate that the DynamoDB table used by Kinesis does not have enough capacity to store the lease data. This situation is more likely to happen if you have a large number of shards. To solve this problem, increase the write capacity assigned to the shard table.
