> #### [Redshift](https://docs.aws.amazon.com/redshift/latest/dg/welcome.html)

- Amazon Redshift is a fast, fully managed, petabyte-scale data warehouse service that makes it simple and cost-effective to efficiently analyze all your data using your existing business intelligence tools. It is optimized for datasets ranging from a few hundred gigabytes to a petabyte or more and costs less than $1,000 per terabyte per year, a tenth the cost of most traditional data warehousing solutions.
- Columnar Storage
- primarily used for Data Warehouse i.e. online analytic processing (OLAP) and business intelligence (BI) applications, also reporting, data, and analytics tools.
For example, where online transaction processing (OLTP) applications typically store data in rows, Amazon Redshift stores data in columns, using specialized data compression encodings for optimum memory usage and disk I/O

Key features
- Massively Parallel Processing
- Columnar Data Storage
- Data Compression
- Query Optimizer
- Result Caching
- Compiled Code

Data Warehouse System Architecture
- Client applications
  - data loading and ETL (extract, transform, and load) tools
  - business intelligence (BI) reporting
  - data mining
  - analytics tools
  - Amazon Redshift is based on industry-standard PostgreSQL, so most existing SQL client applications will work with only minimal changes
- Connections
  - Amazon Redshift communicates with client applications by using industry-standard JDBC and ODBC drivers for PostgreSQL
- Clusters
  - core infrastructure component of an Amazon Redshift data warehouse is a cluster.
  - composed of one or more compute nodes
  - leader node coordinates the compute nodes and handles external communication
- Leader node
  - manages communications with client programs and all communication with compute nodes
  - parses and develops execution plans to carry out database operations
  - Based on the execution plan, the leader node compiles code, distributes the compiled code to the compute nodes, and assigns a portion of the data to each compute node.
- Compute nodes
  - compute nodes execute the compiled code and send intermediate results back to the leader node for final aggregation.
  - Each compute node has its own dedicated CPU, memory, and attached disk storage, which are determined by the node type
  - Amazon Redshift provides two node types
    - dense storage nodes - which enables you to create very large data warehouses using hard disk drives (HDDs) for a very low price.
    - dense compute nodes - which enables you to create high-performance data warehouses using fast CPUs, large amounts of RAM, and solid-state disks (SSDs).

- Node slices
  - A compute node is partitioned into slices
  - Each slice is allocated a portion of the node's memory and disk space, where it processes a portion of the workload assigned to the node
  - leader node manages distributing data to the slices and apportions the workload for any queries or other database operations to the slices.
  -  slices then work in parallel to complete the operation.
  - number of slices per node is determined by the node size of the cluster.
  - When you create a table, you can optionally specify one column as the distribution key. When the table is loaded with data, the rows are distributed to the node slices according to the distribution key that is defined for a table. Choosing a good distribution key enables Amazon Redshift to use parallel processing to load data and execute queries efficiently

Internal network
- Amazon Redshift takes advantage of high-bandwidth connections, close proximity, and custom communication protocols to provide private, very high-speed network communication between the leader node and compute nodes. The compute nodes run on a separate, isolated network that client applications never access directly.

Databases

- A cluster contains one or more databases.
- User data is stored on the compute nodes
- Amazon Redshift is a relational database management system (RDBMS), so it is compatible with other RDBMS applications.
- Although it provides the same functionality as a typical RDBMS, including online transaction processing (OLTP) functions such as inserting and deleting data, Amazon Redshift is optimized for high-performance analysis and reporting of very large datasets.
- Amazon Redshift is based on PostgreSQL 8.0.2


Performance
- Amazon Redshift achieves extremely fast query execution by employing these performance features.
  - Massively Parallel Processing
  - Columnar Data Storage
  - Data Compression
  - Query Optimizer
  - Result Caching
  - Compiled Code

Designing Tables
- Choosing a Column Compression Type
- Choosing a Data Distribution Style
- Choosing Sort Keys
- Defining Constraints
- Analyzing Table Design

Amazon Redshift Spectrum
- efficiently query and retrieve structured and semistructured data from files in Amazon S3 without having to load the data into Amazon Redshift tables
- Much of the processing occurs in the Redshift Spectrum layer, and most of the data remains in Amazon S3
- You create Redshift Spectrum tables by defining the structure for your files and registering them as tables in an external data catalog
- The external data catalog can be AWS Glue, the data catalog that comes with Amazon Athena, or your own Apache Hive metastore

Amazon Redshift Spectrum Considerations
- The Amazon Redshift cluster and the Amazon S3 bucket must be in the same AWS Region
- If your cluster uses Enhanced VPC Routing, you might need to perform additional configuration steps.
- External tables are read-only. You can't perform insert, update, or delete operations on external tables.
- You can't control user permissions on an external table. Instead, you can grant and revoke permissions on the external schema.
- To run Redshift Spectrum queries, the database user must have permission to create temporary tables in the database.


Factors Affecting Query Performance
- Number of nodes, processors, or slices
- Node types
- Data distribution
- Data sort order
- Dataset size
- Concurrent operations
- Query structure
- Code compilation

Redshift Enhanced VPC Routing
- forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC
- can use standard VPC features
- If Enhanced VPC Routing is not enabled, Amazon Redshift routes traffic through the internet, including traffic to other services within the AWS network.
- can use VPC flow logs to monitor all the COPY and UNLOAD traffic of your Redshift cluster that moves in and out of your VPC.

Redshift Parameter Groups
- parameter group is a group of parameters that apply to all of the databases that you create in the cluster.
- These parameters configure database settings such as query timeout and datestyle.
- When you create a parameter group, the default WLM configuration contains one queue that can run up to five queries concurrently. You can add additional queues and configure WLM properties in each of them if you want more control over query processing. Each queue that you add has the same default WLM configuration until you configure its properties. When you add additional queues, the last queue in the configuration is the default queue. Unless a query is routed to another queue based on criteria in the WLM configuration, it is processed by the default queue. You cannot specify user groups or query groups for the default queue.
- If you want to modify the WLM configuration, you must create a parameter group and then associate that parameter group with any clusters that require your custom WLM configuration.

Redshift Snapshots
- Snapshots are point-in-time backups of a cluster.
- There are two types of snapshots:
  - automated
  - manual
- Redshift stores these snapshots internally in Amazon S3 by using an encrypted Secure Sockets Layer (SSL) connection.
- Redshift automatically takes incremental snapshots that track changes to the cluster since the previous automated snapshot.
- To ensure that your backups are always available to your cluster, Amazon Redshift stores snapshots in an internally managed Amazon S3 bucket that is managed by Amazon Redshift
- Amazon Redshift provides free storage for snapshots that is equal to the storage capacity of your cluster until you delete the cluster
- Automated Snapshots
  - when enabled , periodically takes snapshots of that cluster.
  - takes a snapshot about every eight hours or following every 5 GB per node of data changes, or whichever comes first
  - snapshots are deleted at the end of a retention period. The default retention period is one day, but you can modify it
  - To disable automated snapshots, set the retention period to zero
  - Only Amazon Redshift can delete an automated snapshot; you cannot delete them manually
  - Also supports automated Cross-Region Snapshot Copy for Amazon Redshift
- Automated Snapshot Schedules
  - To precisely control when snapshots are taken, you can create a snapshot schedule and attach it to one or more clusters.
  - Snapshot schedules that lead to backup frequencies less than 1 hour or greater than 24 hours are not supported. If you have overlapping schedules that result in scheduling snapshots within a 1 hour window, a validation error results.

- Manual Snapshots
  - can take a manual snapshot any time.
  - By default, manual snapshots are retained indefinitely, even after you delete your cluster.
  -  If you create a snapshot using the Amazon Redshift console, it defaults the snapshot retention period to 365 days.


Redshift Database Encryption
- In Amazon Redshift, you can enable database encryption for your clusters to help protect data at rest. When you enable encryption for a cluster, the data blocks and system metadata are encrypted for the cluster and its snapshots.
- You can enable encryption when you launch your cluster, or you can modify an unencrypted cluster to use AWS Key Management Service (AWS KMS) encryption. To do so, you can use either an AWS-managed key or a customer-managed key (CMK). When you modify your cluster to enable KMS encryption, Amazon Redshift automatically migrates your data to a new encrypted cluster. Snapshots created from the encrypted cluster are also encrypted. You can also migrate an encrypted cluster to an unencrypted cluster by modifying the cluster and changing the Encrypt database option


Configuring Workload Management
- In Amazon Redshift, you use workload management (WLM) to define the number of query queues that are available, and how queries are routed to those queues for processing. WLM is part of parameter group configuration. A cluster uses the WLM configuration that is specified in its associated parameter group.
When you create a parameter group, the default WLM configuration contains one queue that can run up to five queries concurrently. You can add additional queues and configure WLM properties in each of them if you want more control over query processing. Each queue that you add has the same default WLM configuration until you configure its properties.
When you add additional queues, the last queue in the configuration is the default queue. Unless a query is routed to another queue based on criteria in the WLM configuration, it is processed by the default queue. You can specify mode and concurrency level for the default queue, but you can't specify user groups or query groups for the default queue.

Redshift Security Overview
- Sign-in credentials
- Access management
- Cluster security groups
- VPC
- Cluster encryption
- SSL connections
- Load data encryption
- Data in transit
