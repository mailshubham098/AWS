> #### [Amazon Elastic File System](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)

- Amazon EFS provides file storage for your Amazon EC2 instances. With Amazon EFS, you can create a file system, mount the file system on your EC2 instances, and then read and write data from your EC2 instances to and from your file system.
- POSIX file system access
- simple, scalable file storage for use with Amazon EC2.
- storage capacity is elastic, growing and shrinking automatically as you add and remove files
- supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol
- EFS oﬀers two storage classes, Standard and Infrequent Access.
- strong data consistency and file locking
- supports encryption in transit and encryption at rest
- By using an Amazon EFS file system mounted on an on-premises server, you can migrate on-premises data into the AWS Cloud hosted in an Amazon EFS file system .
  - on-premise n/w -> AWS direct connect -> EFS


Data Consistency in Amazon EFS
- provides open-after-close consistency semantics that applications expect from NFS
- write operations are durably stored across Availability Zones in these situations:
  - An application performs a synchronous write operation (for example, using the open Linux command with the O_DIRECT flag, or the fsync Linux command).
  - An application closes a file.
- Depending on the access pattern, Amazon EFS can provide stronger consistency guarantees than open-after-close semantics.

Storage Classes and Lifecycle Management
- Amazon EFS offers
  - Standard storage class
  - Infrequent Access storage class(EFS IA)
- With Lifecycle Management enabled, EFS automatically moves files not accessed for 30 days from the Standard storage class to the EFS IA storage class.

Performance Modes
- General purpose
- Max I/O

Throughput Modes
- Bursting throughput mode (default)
  - throughput grows as your filesystem grows
- Provisioned Throughput mode
  - you specify the throughput of your filesystem independent of the data stored

Mount Targets
- To access filesystem (efs) in a vpc ,you create a mount target.A mount target provides an IP address for and NFSv4 endpoint
- can create one mount target per AZ in a region
- you mount your filesystem using its DNS , which will resolve to an IP add of the EFS mount target
- Format  : file-ssytem-id.efs.aws-region.amazonaws.com
- To use EFS with an on-premise server , the on-premise server should be Linux based

Transferring Data into Amazon EFS
- Recommended to use AWS DataSync to transfer data into Amazon EFS
- You can also use DataSync to transfer files between two EFS file systems, including file systems in different AWS Regions and file systems owned by different AWS account

Backing Up Amazon EFS
- There are two options available for backing up your EFS file systems:
  - AWS Backup service
    -  simple and cost-effective way to back up your Amazon EFS file systems
  - The EFS-to-EFS backup solution
    - suitable for all Amazon EFS file systems in all AWS Regions.

Amazon EFS Performance Tips
- Average I/O Size
- Simultaneous Connections
- Request Model
- NFS Client Mount Settings
- Amazon EC2 Instances
- Encryption

Performance Comparison, Amazon EFS and Amazon EBS

|| Amazon EFS     |Amazon EBS Provisioned IOPS   |
| :------------- | :------------- |:-----------|
| Per-operation latency	|Low, consistent latency.	|Lowest, consistent latency.|
|Throughput scale	|10+ GB per second.	|Up to 2 GB per second.|

Storage Characteristics Comparison, Amazon EFS and Amazon EBS


|	|Amazon EFS	|Amazon EBS Provisioned IOPS |
| :------------- | :------------- |:-----------|
| Availability and durability	|Data is stored redundantly across multiple AZs.|	Data is stored redundantly in a single AZ.|
|Access	|Up to thousands of Amazon EC2 instances, from multiple AZs, can connect concurrently to a file system.	|A single Amazon EC2 instance in a single AZ can connect to a file system.|
|Use cases	|Big data and analytics, media processing workflows, content management, web serving, and home directories.	|Boot volumes, transactional and NoSQL databases, data warehousing, and ETL.|


Resource Limits

|Resource	|Limit|
|-----|-----|
|Number of file systems for each customer account in an AWS Region|1000|
|Number of mount targets for each file system in an Availability Zone|1|
|Number of mount targets for each VPC	|400|
|Number of security groups for each mount target|	5|
|Number of tags for each file system|	50|
|Number of VPCs for each file system|	1|
