S3 - sever access logging vs cloudtrail
https://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html#cloudtrail-logging-vs-server-logs

- An Amazon S3 Glacier (Glacier) vault can have one resource-based vault access policy and one Vault Lock policy attached to it. A Vault Lock policy is a vault access policy that you can lock. Using a Vault Lock policy can help you enforce regulatory and compliance requirements. Amazon S3 Glacier provides a set of API operations for you to manage the Vault Lock policies.
As an example of a Vault Lock policy, suppose that you are required to retain archives for one year before you can delete them. To implement this requirement, you can create a Vault Lock policy that denies users permissions to delete an archive until the archive has existed for one year. You can test this policy before locking it down. After you lock the policy, the policy becomes immutable. For more information about the locking process, see Amazon S3 Glacier Vault Lock. If you want to manage other user permissions that can be changed, you can use the vault access policy

- You can only specify one launch configuration for an Auto Scaling group at a time, and you can't modify a launch configuration after you've created it. Therefore, if you want to change the launch configuration for an Auto Scaling group, you must create a launch configuration and then update your Auto Scaling group with the new launch configuration.


Q 4.9 ,4.22,,4.24,4.27,4.28,4.29,4.32,4.33,4.52,4.60




- SWF is a fully-managed state tracker and task coordinator service. It does not provide serverless orchestration to multiple AWS resources.

- AWS global accelerator vs S3 transfer accelerator vs cloudfront

https://aws.amazon.com/global-accelerator/

https://aws.amazon.com/global-accelerator/faqs/


The main differences are that:

Spot instances typically offer a significant discount off the On-Demand prices
Your instances can be interrupted by Amazon EC2 for capacity requirements with a 2-minute notification
Spot prices adjust gradually based on long term supply and demand for spare EC2 capacity.
You can choose to have your Spot instances terminated, stopped, or hibernated upon interruption. Stop and hibernate options are available for persistent Spot requests and Spot Fleets with the maintain option enabled. By default, your instances are terminated


T1
Q - 1,12,27,52,53,58,

T2
Q - 1, 7,10,13,22,27,31,34,39,41,44,46-r,49,50,59,63,

T3
Q - 5,7,10,13, 19,

T4
Q - 15, 18,20,23,26,50,58,63,
Understand policy json

https://www.examtopics.com/exams/amazon/aws-certified-solutions-architect-associate/view/
Q - 9,14,19,23,25,30,41,50,52,57,65,66,89,91,97,101,124,156,160,161,164,165,170,171,176,188,198,201,204,211,218,
216,227,244,250,253,258,271,289,294,

T5
Q-4,9,20,40,42,43,53,56,58,


T6
Q-6,15,19,
TODO
43

Imp links to read
https://aws.amazon.com/blogs/database/how-to-configure-a-private-network-environment-for-amazon-dynamodb-using-vpc-endpoints/
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#interruption-behavior
https://aws.amazon.com/blogs/database/automating-sql-caching-for-amazon-elasticache-and-amazon-rds/
https://aws.amazon.com/blogs/aws/cloudwatch-log-service/
https://aws.amazon.com/es/premiumsupport/knowledge-center/cloudfront-serving-outdated-content-s3/
https://aws.amazon.com/blogs/aws/new-host-based-routing-support-for-aws-application-load-balancers/
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html
https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-add-elb-healthcheck.html
https://aws.amazon.com/premiumsupport/knowledge-center/internet-access-lambda-function/
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/snapshot-lifecycle.html
https://aws.amazon.com/blogs/database/scaling-your-amazon-rds-instance-vertically-and-horizontally/
https://aws.amazon.com/blogs/database/a-serverless-solution-to-schedule-your-amazon-dynamodb-on-demand-backup/
https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-target-ecs-containers.html
https://aws.amazon.com/premiumsupport/knowledge-center/users-connect-rds-iam/



- Data stored in Amazon Glacier is immutable, meaning that after an archive is created it cannot be updated. This ensures that data such as compliance and regulatory records cannot be altered after they have been archived.

- indexed big data -> dynamo
-  AZ’s are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles) of each other.
- For an Amazon RDS encrypted DB instance, all logs, backups, and snapshots are encrypted.

- SSE-S3: AWS manages both data key and master key
  SSE-KMS: AWS manages data key and you manage master key
  SSE-C: You manage both data key and master key
- Amazon RDS now supports Storage Auto Scaling
- Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses.You can use these access logs to analyze traffic patterns and troubleshoot issues.Access logging is an optional feature of Elastic Load Balancing that is disabled by default.
- Amazon S3 Glacier automatically encrypts data at rest using Advanced EncryptionStandard (AES) 256-bit symmetric keys and supports secure transfer of your data over Secure Sockets Layer (SSL).
- CloudFront is for content delivery. S3 Transfer Acceleration is for faster transfers and higher throughput to S3 buckets (mainly uploads).
- S3 with static site can't do https. It has to be enabled with cloudfront with S3 bucket as origin
- An application load balancer can take the place of multiple individual classic load balancers to load balance microservices (e.g. running on AWS ECS) saving costs.
- you can configure Amazon CloudWatch Events to invoke a Lambda action in response to suspicious or unexpected behavior in your AWS environment detected by Amazon GuardDuty.
- for rds short-lived connection credentials -> Create the user account to use the AWS-provided AWSAuthenticationPlugin with IAM.
- AWS Lambda automatically monitors functions on your behalf, reporting metrics through Amazon CloudWatch. These metrics include total invocation requests, latency, and error rates. The throttles, Dead Letter Queues errors and Iterator age for stream-based invocations are also monitored.
- You will be billed when your On-Demand instance is preparing to hibernate with a stopping state is correct because when the instance state is stopping, you will not billed if it is preparing to stop however, you will still be billed if it is just preparing to hibernate.
-  You will be billed when your Reserved instance is in terminated state is correct because Reserved Instances that applied to terminated instances are still billed until the end of their term according to their payment option.
- Expedited retrievals allow you to quickly access your data when occasional urgent requests for a subset of archives are required. For all but the largest archives (250 MB+), data accessed using Expedited retrievals are typically made available within 1–5 minutes. Provisioned Capacity ensures that retrieval capacity for Expedited retrievals is available when you need it.
- Provisioned capacity ensures that your retrieval capacity for expedited retrievals is available when you need it. Each unit of capacity provides that at least three expedited retrievals can be performed every five minutes and provides up to 150 MB/s of retrieval throughput. You should purchase provisioned retrieval capacity if your workload requires highly reliable and predictable access to a subset of your data in minutes. Without provisioned capacity Expedited retrievals are accepted, except for rare situations of unusually high demand. However, if you require access to Expedited retrievals under all circumstances, you must purchase provisioned retrieval capacity.
- There is a vCPU-based On-Demand Instance limit per region which is why subsequent requests failed. Just submit the limit increase form to AWS and retry the failed requests once approved.
- Remember that an Active-Active Failover uses all available resources all the time without a primary nor a secondary resource.you cannot set up an Active-Active Failover with One Primary and One Secondary Resource
- AWS AppSync simplifies application development by letting you create a flexible API to securely access, manipulate, and combine data from one or more data sources. AppSync is a managed service that uses GraphQL to make it easy for applications to get exactly the data they need.
- Amazon S3 does not currently support Object Locking for concurrent updates. Amazon S3 Object Lock feature prevents an object from being deleted or overwritten for a fixed amount of time or indefinitely
- to change the ec2 instance type , just change the instance type in the current auto scaling launch config , no need to create new config
- In RDS, the Enhanced Monitoring metrics shows
  - RDS child processes
  - RDS processes
  - OS processes
- it is more efficient to use CloudWatch agent than an SSM agent.

unified CloudWatch agent
To collect logs from your Amazon EC2 instances and on-premises servers into CloudWatch Logs, AWS offers both a new unified CloudWatch agent, and an older CloudWatch Logs agent. It is recommended to use the unified CloudWatch agent which has the following advantages:
  - You can collect both logs and advanced metrics with the installation and configuration of just one agent.
  - The unified agent enables the collection of logs from servers running Windows Server.
  - If you are using the agent to collect CloudWatch metrics, the unified agent also enables the collection of additional system metrics, for in-guest visibility.
  - The unified agent provides better performance.
- An Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications. EFA enables you to achieve the application performance of an on-premises HPC cluster, with the scalability, flexibility, and elasticity provided by the AWS Cloud.
- AWS Global Accelerator is incorrect because this service is primarily used to optimize the path from your users to your applications which improves the performance of your TCP and UDP traffic.
- AWS Transit Gateway is a service that enables customers to connect their Amazon Virtual Private Clouds (VPCs) and their on-premises networks to a single gateway. As you grow the number of workloads running on AWS, you need to be able to scale your networks across multiple accounts and Amazon VPCs to keep up with the growth. Today, you can connect pairs of Amazon VPCs using peering. However, managing point-to-point connectivity across many Amazon VPCs, without the ability to centrally manage the connectivity policies, can be operationally costly and cumbersome. For on-premises connectivity, you need to attach your AWS VPN to each individual Amazon VPC. This solution can be time consuming to build and hard to manage when the number of VPCs grows into the hundreds.With AWS Transit Gateway, you only have to create and manage a single connection from the central gateway in to each Amazon VPC, on-premises data center, or remote office across your network. Transit Gateway acts as a hub that controls how traffic is routed among all the connected networks which act like spokes. This hub and spoke model significantly simplifies management and reduces operational costs because each network only has to connect to the Transit Gateway and not to every other network. Any new VPC is simply connected to the Transit Gateway and is then automatically available to every other network that is connected to the Transit Gateway. This ease of connectivity makes it easy to scale your network as you grow.
- Lambda@Edge is a feature of Amazon CloudFront that lets you run code closer to users of your application, which improves performance and reduces latency. With Lambda@Edge, you don't have to provision or manage infrastructure in multiple locations around the world. You pay only for the compute time you consume - there is no charge when your code is not running.
- By default, CloudTrail event log files are encrypted using Amazon S3 server-side encryption (SSE). You can also choose to encrypt your log files with an AWS Key Management Service (AWS KMS) key
- All of the APIs created with Amazon API Gateway expose HTTPS endpoints only.
- When you create an encrypted EBS volume and attach it to a supported instance type, the following types of data are encrypted:
  - Data at rest inside the volume
  - All data moving between the volume and the instance
  - All snapshots created from the volume
  - All volumes created from those snapshots
- frequently accessed, throughput-intensive workloads with large, sequential I/O operations -> st1
- AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics. You can create and run an ETL job with a few clicks in the AWS Management Console.
- Outputs is an optional section of the CloudFormation template that describes the values that are returned whenever you view your stack's properties.
- If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica, which in turn is promoted to become the new primary. Start-to-finish, failover typically completes within 30 seconds.
- If you are running Aurora Serverless and the DB instance or AZ become unavailable, Aurora will automatically recreate the DB instance in a different AZ.
- If you do not have an Amazon Aurora Replica (i.e. single instance) and are not running Aurora Serverless, Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance. This replacement of the original instance is done on a best-effort basis and may not succeed, for example, if there is an issue that is broadly affecting the Availability Zone.
- Amazon FSx provides fully managed third-party file systems. Amazon FSx provides you with the native compatibility of third-party file systems with feature sets for workloads such as Windows-based storage, high-performance computing (HPC), machine learning, and electronic design automation (EDA). You don’t have to worry about managing file servers and storage, as Amazon FSx automates the time-consuming administration tasks such as hardware provisioning, software configuration, patching, and backups. Amazon FSx integrates the file systems with cloud-native AWS services, making them even more useful for a broader set of workloads.
- Amazon FSx provides you with two file systems to choose from: Amazon FSx for Windows File Server for Windows-based applications and Amazon FSx for Lustre for compute-intensive workloads.
- Elastic Beanstalk : Application files are stored in S3. The server log files can also optionally be stored in S3 or in CloudWatch Logs
- Cloudformation:
 -Take note that all of the sections here are optional, except for Resources, which is the only one required.
 -Format Version
 -Description
 -Metadata
 -Parameters
 -Mappings
 -Conditions
 -Transform
 -Resources (required)
 -Outputs
- CloudWatch Logs agent :
  - CloudWatch Logs agent provides an automated way to send log data to CloudWatch Logs from Amazon EC2 instances hence, CloudWatch Logs agent is the correct answer.
  - The CloudWatch Logs agent is comprised of the following components:
    - A plug-in to the AWS CLI that pushes log data to CloudWatch Logs.
    - A script (daemon) that initiates the process to push data to CloudWatch Logs.
    - A cron job that ensures that the daemon is always running.
- AWS hosts a variety of public datasets that anyone can access for free.
- Lamda:
  - The default timeout is 3 seconds and the maximum execution duration per request in AWS Lambda is 900 seconds, which is equivalent to 15 minutes.
  - AWS Lambda limits the total concurrent executions across all functions within a given region to 1000.If that limit is exceeded, the function will be throttled but not terminated
- EBS : EBS volume is only available in the Availability Zone it was created in and cannot be attached directly to other Availability Zones.
- EC2 placement group  :
  - It is recommended that you launch the number of instances that you need in the placement group in a single launch request and that you use the same instance type for all instances in the placement group. If you try to add more instances to the placement group later, or if you try to launch more than one instance type in the placement group, you increase your chances of getting an insufficient capacity error.
  - If you stop an instance in a placement group and then start it again, it still runs in the placement group. However, the start fails if there isn't enough capacity for the instance.
  - If you receive a capacity error when launching an instance in a placement group that already has running instances, stop and start all of the instances in the placement group, and try the launch again. Restarting the instances may migrate them to hardware that has capacity for all the requested instances.
- RDS MySQL : Recommended storage engine for MySQL is InnoDB and not MyISAM.However, in case you require intense, full-text search capability, use MyISAM storage engine instead.

- Route53
  - Amazon Route 53 currently supports the following DNS record types:
    -A (address record)
    -AAAA (IPv6 address record)
    -CNAME (canonical name record)
    -CAA (certification authority authorization)
    -MX (mail exchange record)
    -NAPTR (name authority pointer record)
    -NS (name server record)
    -PTR (pointer record)
    -SOA (start of authority record)
    -SPF (sender policy framework)
    -SRV (service locator)
    -TXT (text record)
- VPC:
  - Multicast is a network capability that allows one-to-many distribution of data. With multicasting, one or more sources can transmit network packets to subscribers that typically reside within a multicast group. However, take note that Amazon VPC does not support multicast or broadcast networking.
  - You can use an overlay multicast in order to migrate the legacy application. An overlay multicast is a method of building IP level multicast across a network fabric supporting unicast IP routing, such as Amazon Virtual Private Cloud (Amazon VPC).

- ELB :
  - N/W or classic load balancer supports cross-zone load balancing.
  - If the load balancer nodes for your Classic Load Balancer can distribute requests regardless of Availability Zone, this is known as cross-zone load balancing. With cross-zone load balancing enabled, your load balancer nodes distribute incoming requests evenly across the Availability Zones enabled for your load balancer. Otherwise, each load balancer node distributes requests only to instances in its Availability Zone.
- Kinesis
  - By default, records of a stream in Amazon Kinesis are accessible for up to 24 hours from the time they are added to the stream. You can raise this limit to up to 7 days by enabling extended data retention.
- RDS
  - Transparent data encryption (TDE) is primarily used to encrypt stored data on your DB instances running Microsoft SQL Server, and not the data that are in-transit.
- EB - automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring whereas if you use ECS it should be configured manually
- IAM: Identity providers
  - Web Identity Federation
    - With web identity federation, you don't need to create custom sign-in code or manage your own user identities. Instead, users of your app can sign in using a well-known external identity provider (IdP), such as Login with Amazon, Facebook, Google, or any other OpenID Connect (OIDC)-compatible IdP.
  - SAML 2.0
    - AWS supports identity federation with SAML 2.0 (Security Assertion Markup Language 2.0), an open standard that many identity providers (IdPs) use. This feature enables federated single sign-on (SSO), so users can log into the AWS Management Console or call the AWS API operations without you having to create an IAM user for everyone in your organization.By using SAML, you can simplify the process of configuring federation with AWS, because you can use the IdP's service instead of writing custom identity proxy code.
- Remote Desktop Protocol port is 3389
- You should consider using AWS CloudHSM over AWS KMS if you require:
  - Keys stored in dedicated, third-party validated hardware security modules under your exclusive control.
  - FIPS 140-2 compliance.
  - Integration with applications using PKCS#11, Java JCE, or Microsoft CNG interfaces.
  - High-performance in-VPC cryptographic acceleration (bulk crypto).
- RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ) that is in the same region and not in a different one.
- ELB works at region level , for across region load balancing use Route53
- only Convertible Reserved Instances can be exchanged for other Convertible Reserved Instances.
- In Amazon S3, all objects are private by default.
- AWS Resource Access Manager (RAM) is a service that enables you to easily and securely share AWS resources with any AWS account or within your AWS Organization.
- AWS ParallelCluster is simply an AWS-supported open-source cluster management tool that makes it easy for you to deploy and manage High-Performance Computing (HPC) clusters on AWS
- You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.An authentication token is a unique string of characters that Amazon RDS generates on request. Authentication tokens are generated using AWS Signature Version 4. Each token has a lifetime of 15 minutes. You don't need to store user credentials in the database, because authentication is managed externally using IAM. You can also still use standard database authentication.
- EBS snapshots occur asynchronously which makes the option that says: The volume can be used as normal while the snapshot is in progress the correct answer. This means that the point-in-time snapshot is created immediately, but the status of the snapshot is pending until the snapshot is complete (when all of the modified blocks have been transferred to Amazon S3), which can take several hours for large initial snapshots or subsequent snapshots where many blocks have changed. While it is completing, an in-progress snapshot is not affected by ongoing reads and writes to the volume hence, you can still use the volume.
