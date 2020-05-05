
## Storage

### EBS
- block storage n/w drive
- locked to single AZ, must be in same AZ as EC2.Snapshot it to move
- can be attached to only one instance at a time
- for root vol deleteOnTermination is true by default
- by default not encrypted but can be set at region level
- the instance to which it attaches must be in the same Availability Zone.
- would work as is when the snapshot is being taken
- cheaper than efs
- Types
  - gp2 (ssd,balances price & performance)
  - io1 (ssd,freq. accessed, io intensive,low latency & high thoughput mission critical, low-latency , random io)
  - st1 (hdd,freq. accessed , throughput intensive , large sequential i/o)
  - sc1 (hdd,infreq. accessed, throughput intensive, large sequential i/o)
-  max 16TB size
- hdd : cannot be boot vol
- Availability : stored in multiple physical volumes in the same AZ at no cost.
- Partitioning Schemes
  - MBR
  - GPT
- RAID config
  - RAID 0 (split) - high performance
  - RAID 1 (mirror) - availability
  - RAID 10 ( split & mirror) - mix
- EBS backups use IO and you shouldn't run them while your application is handling a lot of traffic.Use snapshots or Raid 1 instead for seamless backups.
- EBS Snapshots
  - Asynchronous ,incremental,
  - Recommended to detach root volume to do snapshot but not necessary
  - Uses IO: Shouldn't run while your application is handling a lot of traffic
  - Share snapshot with other accounts,(If encrypted) Share the custom key used to encrypt the volume.
  - Constrained to the Region -> copy to share across region
  - Stored in S3 (but you won't directly see them)
  - Can make Image (AMI) from Snapshot for e.g. faster deploy on ASG.
  - Snapshots can be automated using DLM - Amazon Data Lifecycle Manager
- You can now increase capacity (GB & IOPS), or change the volume type while in use.
- Volume Encryption
  - The encryption occurs on the servers that host EC2 instances
Provides encryption of data as it moves between EC2 instances and EBS storage i.e. all the data in flight moving between the instance and the volume is encrypted
  - leverages keys from KMS (AES-256)
  - data in transit between an instance and an encrypted volume is also encrypted
  - while copying an unencrypted snapshot allows encryption
  - By default
    - vol not encrypted ( but can change the default setting for vol encrption at region level)
    - snapshot encrypted
    - vol created from snapshot encrypted
  - To encrypt an unencrypted volume : snapshot vol -> copy snapshot(with encryption) -> create vol from this snapshot
- Amazon EBS–Optimized instances uses optimized configuration stack and provides additional & dedicated capacity for EBS I/O. Recommended to increase storage performance of an instance.

Instance Store
- Actual root volume of some instances, (some come with EBS volumes)
- Cannot be detach / reattached
- Ephemeral (temporary) storage physically attached to the machine
  - Terminate/stop the instance -> its gone
  - Data survives reboots
- Unexpected terminations might happen from time to time (AWS will e-mail you)
- Higher performance but cannot be remapped to another instances
  - Choose instance store if you can handle losing data e.g. caches otherwise EBS
- It's not a network drive (as opposed to EBS) so better I/O
  - No managed backups, cannot resize.
  - You can't attach instance store volumes to an instance after you've launched it

### EFS
  - POSIX
  - uses - filesystem , file access and sharing ,dir/file level access
  - NFS
  - Expensive
  - Region level
  - Auto-scaling (without provisioning)
  - default Multi AZ
  - Storage class
    - Standard
    - Infrequent access ( EFS IA)
  - With Lifecycle Management enabled, 30 days -> std -> IA
  - Mode :
    - performance
      - gp (default)
      - max i/o ( low latency )
    - throughput
      - bursting
      - provisioned
  - Backup support
  - Encryption at rest using KMS keys
  - use AWS DataSync to transfer data to efs, b/w efs, b/w accounts


### S3
- key-value object store
- S3 is a global service (not region specific)
- A bucket however is attached to a region
- Objects (files) are stored in buckets (directories(also called prefix))
-  Buckets
  - Bucket names must be unique across all existing bucket names in Amazon S3.
  - Buckets are defined at the region level and must have globally unique name as it'll following URLs
  - `https://s3-<region>.amazonaws.com/<bucket>/<object>`
  - `https://<bucket>.s3.amazonaws.com/<object> (virtual host style addressing)`
  - You can set default encryption
    - Options are: None, AES-256 (SSE-S3) and AWS-KMS (SSE-KMS)
  - 100 S3 buckets per account -> soft limit
  - Version Control
    - Can enable versioning at bucket level.
      - Once enabled, it cannot be disabled but just suspended.
    - Same key overwrite will increment the version
    - Protection against accidental deletion i.e. unintended deletes.
      - Deleting files does not really delete versioned files
      - It only puts Delete marker and files can still be downloaded.
      - Deleting a delete marker -> Restores the object & adds a new delete marker.
        - Only the owner of a bucket can permanently remove a delete marker e.g. delete a version.
    - Any file that is not versioned prior to enabling versioning will have the version `null`.
    - The is no concept of directories within buckets just key names with slashes.
- Objects
  - Have a key which is the FULL path
  - Max size is 5 TB, min size is 0 bytes (touched)
  - If file is more than 5 GB, you need to use multi part upload
  - Metadata is list of text key/value pairs.
    - Can be system or user metadata.
    - Cannot be modified after creation
  - Tags are unicode key/value pairs
    - Up to 10
    - Useful for security / lifecycle / replication rules
- Logging
  - Server access logging
    - provides detailed records for the requests that are made to a bucket.(access request)
    - Do not store your own access logs in the same bucket, otherwise; recursion
  - Object-level logging
    - Records object-level api activity (reads/writes) using CloudTrail data events feature (additional cost)

- S3 Consistency Model
  - Read after write consistency for PUTS of new objects
  - Eventual consistency for DELETEs and PUTS of existing objects
- Make an object publicly accessible
  - On object level
    - Set a predefined grant (also known as a canned ACL) for a bucket in Amazon S3.
      - In Console -> Bucket -> Permissions -> Public access settings -> Untick "Block new public bucket policies" and "Block public and cross-account access if the bucket has public policies"
    - Update the object's ACL -> Ensure your object is set to be publicly accessible.
  - On bucket level
    - Use a bucket policy that grants public read access to a specific object tag/prefix
- Pricing
  - charged based on: • storage • requests • storage management pricing (tiers) • data transfer • region replication • transfer acceleration
  - S3 data transfer with any AWS services in that same region is free (still pay for requests)
  - Requester pays
    - Requester pays for requests + storage in bucket instead of bucket owner.
    - Requester is from AWS (not anonymous) & sends requester-pays flag and billed by AWS.
- Lifecycle Policies
  - Transition actions
    - When objects are transitioned to another storage class
    - Objects must be stored at least 30 days in standard for standard to => Standard IA or OneZone IA
  - Expiration actions
    - Expire & delete an object after a certain time period.
- Multipart Upload
  - Use if file is more than >100MB (required for >=5GB)
  - Allows uploading parts concurrently
    - Improved throughput
    - Quick recovery from any network failures
    - Pause and resume object uploads
    - Begin an upload before you know the final object size.
  - All storage that any parts the aborted multipart upload consumed is freed & deleted.
    - S3 supports a bucket lifecycle rule to abort multipart upload that don't complete within a specified number of days.
- Event Notifications
  - Events: Object created, object removed, object restored, object lost (RSS) etc.
  - Destinations: SNS, SQS and Lambda
- [S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)
  - STANDARD (Frequently accessed data)
  - STANDARD_IA (Long-lived, infrequently accessed data)
  - INTELLIGENT_TIERING (Long-lived data with changing or unknown access patterns)
  - ONEZONE_IA (Long-lived, infrequently accessed, non-critical data)
  - GLACIER (Low cost, archive data with retrieval times ranging from minutes to hours < 12 hrs)
  - Glacier Deep Archive (Lowest cost, Archive data that rarely, if ever, needs to be accessed with retrieval times in hours >12 hrs)
  - RRS (Not recommended)
- S3 Transfer Acceleration
  - fast transfer via cloudfront's edge locations using optimized
   network path.
  - additional fee
- Hosting static website
  - Required configurations:
    - The bucket must have the same name as your domain or subdomain.
    - A registered domain name.
    - Route 53 as DNS service for the domain to configure DNS.
    - Enabling Website Hosting
    - Configuring Index Document Support
    - Permissions Required for Website Access
  - If you get 403 (Forbidden) error; make sure bucket policy allows public reads!
  - Supports redirects through a metadata tag.
  - Two general forms of an Amazon S3 website endpoint are as follows:
    - `bucket-name.s3-website-region.amazonaws.com`
    - `bucket-name.s3-website.region.amazonaws.com`
- S3 Batch Operations
  - perform large-scale batch operations on Amazon S3 objects
- S3 Object Lock
  - "Write Once Read Many" (WORM) model
  - prevent an object from being deleted or overwritten for a fixed amount of time or indefinitely.
  - works only in versioned buckets
  - Types
    - retention periods
      -  locks for fixed period of time
    - legal holds
      - locks with no expiration date
- S3 object tags
  -  to categorize objects , max 10 tags per object
- Replication
  - Cross region replication (buckets in different region)
  - Same region replication (buckets in same region)
  - Must enable versioning on source and destination buckets
  - Buckets configured for object replication can be owned by the same AWS account or by different accounts.
  - File can still be modified (upload, delete, etc.) while replication is in progress.
  - Replicated
    - Objects created after you add a replication configuration.
    - Both unencrypted objects and objects encrypted using Amazon S3 managed keys (SSE-S3) or AWS KMS managed keys (SSE-KMS), although you must explicitly enable the option to replicate objects encrypted using KMS keys. T
    - Object metadata and tags
  - Not replicated
      - Objects that existed before turning on replication
      - To replicate objects encrypted with CMKs stored in AWS KMS, you must explicitly enable the option

- Security
  - Block public access
  - ACL
  - Bucket policy
  - supports VPC endpoints to be accessed in VPC without internet access
  - CORS (Cross Origin Resource Sharing)
    - Useful when hosting website
    - defines a way for client web applications that are loaded in one domain to interact with resources in a different domain
  - Pre-Signed URLs
    - allows user to be able to download or upload a specific object without requiring AWS security credentials or permissions
    - valid only till the expiration date & time
  - MFA Delete
    - To use MFA-Delete, enable Versioning on the S3 bucket.
    - Only the bucket owner (root account) can enable/disable MFA-delete.
    - You will need MFA to
      - permanently delete an object version
      - suspend versioning on the bucket
  - Encryption at rest
    - SSE-S3
      - Amazon S3-Managed Keys (AES-256)
      - encrypts the key itself with a master key that it regularly rotates
    - SSE-KMS
      - AWS KMS-Managed Keys
      - KMS advantages: user control (over rotation of key) + audit trail (who and when the key was used)
      - option to create and manage encryption keys yourself via kms
      - Must set header `x-amz-server-side-encryption`="aws:kms"
      - additional charges for using this service
    - SSE-C
      - Customer-Provided Keys
      - You manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects.
      - you should send the keys and encryption algorithm with each API call in header
        - `x-amz-server-side​-encryption​-customer-algorithm` = "AES256"
        - `x-amz-server-side​-encryption​-customer-key`
        - `x-amz-server-side​-encryption​-customer-key-MD5`
    - Client Side Encryption
      - Fully client managed keys
      - Clients encrypt & decrypt data themselves  
  - Encryption in-transit
    - use HTTPS/TLS

- Monitoring      
  - CloudWatch Alarms
  - CloudTrail Log Monitoring
  - S3 dashboard

### S3 Glacier
- for data archival
- Archive
  - store data in (one or more files) with unique archive ID.
  - the content of the archive is immutable, meaning that after an archive is created it cannot be updated.
- Vault
  - collection of archives where you upload data to.
- Retrieval
  - S3 object stored through Glacier can only be retrieved with S3 APIs as it'll map object path to archive id.
  - Options
    - Expedited (1-3 mins) $$$
    - Standard (3-5 hrs) $$
    - Bulk (5 -12 hrs) $
- Deep Archive has only one option for retrieval time around 12-48 hours but is the cheapest of all
- You cannot upload directly from console.
- Glacier Select allows you to to perform filtering directly against a Glacier object using standard SQL statements.


## S3 Integrated Services

### S3 Select
- enables applications to retrieve only a subset of data from an object by using simple SQL expressions.
-  dramatically improve the performance and reduce the cost of applications that need to access data in S3.

### Snowball
- Transport terabytes or petabytes of data to and from AWS
- Encryption is enforced
- Tracking using SNS and text messages. E-ink shipping label
- Pay per data transfer job
- Use cases
  - large data cloud migrations, DC decommissions, disaster recovery
  -  Use it if data takes longer than a week to upload e.g. > 2TB for 45 Mbps (T3, 269 days), >5TB 100 Mbps (120 days), >60TB 1000 Mbps (12 days).
- Available storage options
  - 50 TB (42 TB) - US regions only
  - 80 TB (72 TB) - other regions
- E Ink display

### Snowball Edge
- Snowball + additional computational capability (e.g. lambda, compute instances, IOT, file interface, GUI etc) to the device
- Available storage options
  - 100 TB (83 TB)
  - 100 TB Clustered (45 TB per node)
- E Ink display + LCD display

### Snowmobile
- Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)
- If you have more than 100 TB or PBs data
- Each Snowmobile has 100 PB of capacity (use multiple in parallel)
- Better than Snowball if you transfer more than 10 PB e.g. video libraries, image repositories, on-premise migration

### Storage Gateway
- connects an on-premises software appliance with cloud-based storage to provide seamless and secure integration between your on-premises IT environment and the AWS storage infrastructure in the cloud.
- Allows you to expose S3 to on-premises.
- Bridge between on-premise data and cloud data in S3
- Types
  - File gateway -> S3
    - File access / NFS / SMB => File Gateway (backed by S3)
  - Volume gateway -> EBS
    - Block storage in Amazon S3 with point-in-time backups as compressed Amazon EBS snapshot.
    - Allows you to view S3 bucket as mountable volume
    - Block storage using iSCSI protocol backed by S3
    - Options
      - Cached volumes
        - most frequently data are cached on premise on the volumes
      - Stored volumes
        - entire dataset is on premise, scheduled backups to S3 as EBS snapshots
    - Volumes / Block Storage / iSCSI => Volume gateway (backed by S3 with EBS snapshots)
  - Tape Gateway -> Glacier
    - Input: Your existing tape-based processes
    - Output: As virtual tapes (using Virtual Tape Library, i.e. VTL) in S3 or Glacier.
    - Uses iSCSI which is interface & networking standard for linking data storage facilities.
    - VTL Tape solution / Backup with iSCSI => Tape gateway (backed by S3 and Glacier)

### Athena    
- Serverless service to perform analytics directly against S3 files
- Uses SQL language to query the files
- Has a JDBC / ODBC driver that allows you to connect BI tools to it.
- Charged per query and amount of data scanned
- Supports CSV, JSON, ORC, Avro and Parquet (built on Presto)
- Use cases
  - easiest way to run ad-hoc queries for data in S3 without the need to setup or manage any servers.
- Analyze data directly on S3 -> use Athena
- Allows you to easily query encrypted data stored in S3 and write encrypted results back.
  - Both, server-side encryption and client-side encryption are supported
- thena scales automatically—executing queries in parallel—so results are fast, even with large datasets and complex queries.
- helps you analyze unstructured, semi-structured, and structured data stored in S3.
- Athena uses a managed Data Catalog to store information and schemas about the databases and tables that you create for your data stored in Amazon S3. In regions where AWS Glue is available, you can upgrade to using the AWS Glue Data Catalog with Amazon Athena. In regions where AWS Glue is not available, Athena uses an internal Catalog

## Networking

### Route 53
- AWS managed DNS (Domain Name System)
- Hosted Zone
  - Container that holds information about how you want to route traffic for a domain
  - Types
    - Public
      - for public domain names
      - route internet traffic to your resources
      - Accessible from www e.g. `application1.mypublicdomain.com`
    - Private
      -  for private domain names
      - route traffic within an Amazon VPC
      - Gets resolved by instances in VPC e.g. `application1.company.internal`
      -  If you use custom DNS domain names in a private zone in Route 53, you must set both attributes to true in VPC: `enableDnsSupport`, `enableDnsHostname`.
  - You need to delete all record sets in order to delete Hosted Zone
- TTL (time to live)
  - Computer caches DNS request for TTL duration, after which it again reaches to Route 53 to refresh cache
- DNS Records
  - Types
    - A (address record)
    - AAAA (IPv6 address record)
    - CNAME (canonical name record)
    - CAA (certification authority authorization)
    - MX (mail exchange record)
    - NAPTR (name authority pointer record)
    - NS (name server record)
    - PTR (pointer record)
    - SOA (start of authority record)
    - SPF (sender policy framework)
    - SRV (service locator)
    - TXT (text record)
  - DNSSEC is not supported
- In AWS, the most common records are:
  - A ->	URL to IPv4
  - AAAA ->	URL to IPv6
  - CNAME	-> URL to URL
    - You can’t create a CNAME record at the zone apex
    - charges for CNAME queries.
    - can point to any DNS record that is hosted anywhere.
  - Alias	-> URL to AWS resource
    - You can create an alias record at the zone apex. Alias records must have the same type as the record you’re routing traffic to.
    - Free of charge
    - can only point to selected AWS resources or to another record in the hosted zone that you’re creating the alias record in.
- Routing Policies
  - Simple routing policy
    - One domain to one/more IP/URL
    - You can't attach health checks to simple routing policy
  - Weighted routing policy
    - Control % of request to which targets
  - Latency routing policy
    - Redirect to least latency target
  - Failover routing policy
    - Active-passive failover
    - 2 record sets, one primary and one secondary for same target
  - Geolocation routing policy
    - Based on user location
    - Should create "default" policy if there's no match
  - Geoproximity routing policy
    - Based on user and resource location
    - Uses configurable biases to route more or less
    - Bias expands or shrinks zone from where traffic is routed to a resource
  - Multi Value routing policy
    - up to eight healthy records selected at random
    - Enables client side load balancing
- Failover configurations
  - Active-Active Failover
  - Active-Passive Failover

### CloudFront
- Content Delivery Network (CDN)
- speeds up distribution of your static and dynamic web content as content is cached at the edge locations (+136 point of presence globally)
- Edge cache -> Regional Cache -> Origin server
- Regional edge caches have feature parity with edge locations. For example, a cache invalidation request removes an object from both edge caches and regional edge caches before it expires. The next time a viewer requests the object, CloudFront returns to the origin to fetch the latest version of the object.
- Proxy methods PUT/POST/PATCH/OPTIONS/DELETE go directly to the origin from the edge locations and do not proxy through the Regional Edge Caches.
- Dynamic content, as determined at request time (cache-behavior configured to forward all headers), does not flow through regional edge caches, but goes directly to the origin.
- Signed URLs
  - for RTMP distribution -> a video/media (streaming) protocol over SSL
  - restrict access to individual files
  - for those clients that doesn't support cookies
- Signed cookies
  - to provide access to multiple restricted files
  - you don't want to change your current URLs.
- Origin Access Identity
  - More secure: Clients must go through CloudFront, cannot go directly to S3
  - Serving static content, globally
    - Client <-- CloudFront --> Amazon S3
  - Serving static content, globally, securely
    - Client <-- CloudFront (OAI: Origin Access Identity) <--> Amazon S3 (bucket policy + only authorize from OAI)
- Geo Restriction allows you to specify a list of whitelisted or blacklisted countries in your CloudFront distribution.
- Supports: Perfect Forward Secrecy (new SSL key for each session)
- Cache invalidation
  - You can clear cache objects by creating an invalidation but you will be charged.
  - AWS recommends versioned file names instead of invalidating if files are updated frequently.
- Integrates with AWS Shield for DDoS protection.
- Integrates with AWS WAF for application layer security.
- `Cache-Control` and `Expires` headers control how long objects stay in the cache.  
  - `Cache-Control max-age` lets you specify how long (in seconds) you want an object to remain in the cache before CloudFront gets the object again from the origin server.
  - Minimum 0 ; Max 3600

### VPC
- max 5 VPCs in a region - soft lim
- VPC CIDR should not overlap with your other networks
- A VPC is multi AZ but deployed into a single region.
- Min size is /28 = 16 addrs , Max size is /16 = 65536 addresses
- One subnet is tied to one AZ
- Default VPC
  - All new accounts have a default VPC in each region.
  - It comes with
    - Subnet per AZ
    - Internet gateway
    - Network ACL: allows all inbound and outbound traffic
    - Main route table that routes VPC CIDR range to local and 0.0.0.0/0 to Internet gateway.
    - Security group denying inbound from internet, allowing outbound to internet.
  - Instances get public & private IPv4 + hostnames
    - In custom VPC = private IP + hostname, and public depending on settings
- DNS Resolution in VPC
  - `enableDnsSupport`
    - Helps decide if DNS resolution is supported for the VPC
    - If true (default), queries the AWS DNS server at 169.254.169.253
  - `enableDnsHostname`
    - If True, assign public hostname to EC2 instance if it has a public IP
     - Private DNS is always assigned regardless true or false
    - False by default for newly created VPC, True by default for Default VPC
    - Won't do anything unless `enableDnsSupport=true`
  - Private DNS + Route 53: enable DNS Resolution + DNS hostnames (VPC) DNS domains and their subdomains keeping the resources masked from the Internet.
- Multicast is not supported.
  - Multicasting: one or more sources can transmit network packets to subscribers
  - You can enable virtual overlay network
    - It's IP level multicast across network fabric unicast IP routing supported by VPC.
- Network Access Control Lists (NACLs)
  - Operates at the subnet level
  - control traffic from and to subnets.
  - Is stateless: Return traffic must be explicitly allowed by rules
  - Supports allow rules and deny rules
  - Rules are numbered and evaluated in order.If a rule is hit it won't evaulate other rules
  - Default NACL allows everything outbound and everything in bound
    - Newly created NACL will deny everything
- Security Group
  - Operates at the instance level
  - Supports allow rules only
  - Is stateful: Return traffic is automatically allowed, regardless of any rules
  - We evaluate all rules before deciding whether to allow traffic
- Internet Gateways
  - to provide a target in your VPC route tables for internet-routable traffic
  - to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses.
  - If your subnet is associated with a route table that has a route to an internet gateway, it's known as a public subnet.
- Egress-Only Internet Gateways
  - To enable outbound-only Internet communication over IPv6
  - cannot associate a security group with an egress-only Internet gateway.
  - stateful
- NAT device
  - enable instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating connections with the instances
  - supported only for IPv4
  - Types
    - NAT gateway
      - must also specify an Elastic IP address to associate with the NAT gateway when you create it
      - Highly available. NAT gateways in each Availability Zone are implemented with redundancy. Create a NAT gateway in each Availability Zone to ensure zone-independent architecture.
      - Can scale up to 45 Gbps.
      - Managed by AWS. You do not need to perform any maintenance.
      - Not associated with security groups
      - No need to disable the source/destination check
    - NAT instance

- VPC Endpoints
  - privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection
  - PrivateLink = VPC Endpoints
  - They scale horizontally and are redundant
  - Types
    - Interface Endpoints (Powered by AWS PrivateLink)
      - An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service
      - Ensure that the attributes Enable DNS hostnames and Enable DNS Support in VPC are set to true.
    - Gateway Endpoints
      - A gateway endpoint is a gateway that is a target for a specified route in your route table, used for traffic destined to a supported AWS service.
      - The following AWS services are supported:
        - Amazon S3
        - DynamoDB
- VPN Connections
  - You can connect your Amazon VPC from remote networks
  - connectivity options
    - AWS Site-to-Site VPN
    - AWS Client VPN
    - AWS VPN CloudHub
    - Third party software VPN appliance
- Bastion host(Jump Boxes)
  - Designed to withstand security attacks and usually hosts only a proxy server
  - Used to administer EC2 (SSH/RDP)
    - Does not replace NAT gateway/instance as they are not used for SSH/RDP but to provide internet traffic to private subnets.
  - The bastion is in the public subnet /(DMZ) which is then connected to all other private subnets
  - Small EC2 instances is enough as it does not require much processing power
  - Make sure the bastion host only has port 22 traffic from the IP you need, not from the security groups of your instances
- VPC Peering
  - Connect two VPC, privately using AWS network
  - Unsupported VPC Peering Configurations
    - Must not have overlapping CIDR
    - VPC Peering connection is not transitive
    - Edge to Edge Routing Through a Gateway or Private Connection
  - You can do cross-account & cross-region VPC peering
  - no single point of failure for communication or a bandwidth bottleneck.
- VPC Flow Logs
  - Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface Flow Logs
  - You can query VPC flow logs using Athena on S3 or CloudWatch Logs Insights
- AWS Direct Connect
  - private dedicated connection between on-premise and AWS resource
  - Connection types
    - Hosted (1Gbps, 10 Gbps)
      - Connect directly to an AWS device from your router at an AWS Direct Connect location or work with an AWS Direct Connect Partner or a network provider to connect a router from your data center, office, or colocation environment to an AWS Direct Connect location
    - Dedicated(50MBPS to 10GBPS)
      - Work with a partner in the AWS Direct Connect Partner Program to connect a router from your data center, office, or colocation environment to an AWS Direct Connect location. Only certain partners provide higher capacity connections.

### Global Accelerator
- provides static IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your Application Load Balancers, Network Load Balancers or Amazon EC2 instances.
- uses the AWS global network to optimize the path from your users to your applications, improving the performance of your TCP and UDP traffic.
- Easily move endpoints between Availability Zones or AWS Regions without needing to update your DNS configuration or change client-facing applications
- Global Accelerator + ELB
  - ELB provides load balancing within one Region, AWS Global Accelerator provides traffic management across multiple Regions
  - A regional ELB load balancer is an ideal target for AWS Global Accelerator. By using a regional ELB load balancer, you can precisely distribute incoming application traffic across backends, such as Amazon EC2 instances or Amazon ECS tasks, within an AWS Region
- Global Accelerator vs CloudFront
  - both uses AWS global network and its edge locations around the world.
  - CloudFront improves performance for both cacheable content (such as images and videos) and dynamic content (such as API acceleration and dynamic site delivery).
  - Global Accelerator improves performance for a wide range of applications over TCP or UDP by proxying packets at the edge to applications running in one or more AWS Regions. Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require static IP addresses or deterministic, fast regional failover.
- Global Accelerator vs DNS-based traffic management(Route 53)
  - some client devices and internet resolvers cache DNS answers for long periods of time. So when you make a configuration update, or there’s an application failure or change in your routing preference, you don’t know how long it will take before all of your users receive updated IP addresses. With AWS Global Accelerator, you don’t have to rely on the IP address caching settings of client devices. Change propagation takes a matter of seconds, which reduces your application downtime
  - ou get static IP addresses that provide a fixed entry point to your applications. This lets you easily move your endpoints between Availability Zones or between AWS Regions, without having to update the DNS configuration or client-facing applications.

### Amazon API Gateway
- AWS API Management platform
- Supports stateful (WebSocket) and stateless (HTTP) APIs.
- Allows you to build completely serverless applications
  - Common architecture: Client <-- REST API --> API Gateway <-- Proxy requests --> AWS Lambda <-- CRUD --> Amazon DynamoDB
- Some features
  - Transform and validate requests and responses
  - API versioning
  - Different environments
  - Monitoring
    - Using AWS X-Ray to trace messages as they travel through the APIs to backend services.
    - CloudTrail logs.
  - Cache API responses
- API endpoint types
  - Regional: Default, deployed to the specified region.
  - Edge-optimized: API requests are routed to the nearest CloudFront Point of Presence (POP)
  - Private: Exposed through interface VPC endpoints to allow clients to securely access private API resources inside a VPC.
- Public endpoints are always HTTPS!
- Throttling
  - Unlimited scaling: API Gateway can scale to any level of traffic received by an API
  - Can scale up to the default throttling limit of 10,000 requests per second, and can burst past that up to 5,000 RPS.
  - Throttling is used to protect back-end instances from traffic spikes
  - Throttling can be configured at multiple levels including Global and Service Call.
- Security
  - Track and control usage using API keys
    - Can also use Lambda authorizers or usage plans to control access to your APIs.
  - CORS
    - CORS is used by browsers against cross side scripting (XSS) attacks.
    - You must enable CORS for API gateway to send right headers to OPTIONS requests.
  - Authorization and Authentication of backend
    - IAM Roles / Users - AWS Signature Version 4 Signing Process (Sig v4)
      - Allows you to- You sign all requests with signed using an access key (derivation of access key ID + secret access key) in header
      - Great for users / roles already within your AWS account
      - Handles authentication & authorization
    - IAM policies with Lambda Authorizer (formerly Custom Authorizers)
      - Helps to use OAuth / SAML / 3rd party type of authentication
      - Option to cache result of authentication
      - Great for third party tokens
      - Handles authentication & authorization
  - Cognito User Pools
    - You manage your own user pool (can be backed by Facebook, Google, login etc..)
    - Handles only authentication, authorization must be implemented in the back-end

## App integration

### SNS(Simple Notification Service)
- Pub - sub model
- Push messaging model
- Model : Topic ,Publisher , Subscriber
  - Topic
    - placeholder where events are published to or subscribed from
    - supports SSE(server-side encryption) when enabled
  - Publisher
    - Publishers communicate asynchronously with subscribers by producing and sending a message to a topic,
  - Subscriber
    - Subscribers consume or receive the message or notification over one of the below supported protocols
    - Lambda
    - email , email-json
    - HTTP/S
    - SQS
    - SMS
    - platform application endpoint (notification to mobile)
  - Each subscriber get all the messages but can filter.
- stores the message in multiple isolated AZ
- Retention: You have to process the data right away or it's lost
- Publish types
  - Topic Publish (within your AWS server - using the SDK)
  - Direct Publish (for mobile apps SDK)
- SNS -> SQS => FAN OUT architecture
- 10,000 topics -> soft limit

### SQS(Simple Queue Service)
- fully managed message queuing service
- easy to decouple and scale microservices, distributed systems, and serverless applications.
- No limit to how many messages can be in the queue
- Types of Queue
  - Standard Queue
    - Unlimited Throughput
    - At-Least-Once Delivery
    - Best-Effort Ordering
    - max 256K messages
    - Can have duplicate messages occasionally
  - FIFO Queue
    - High Throughput but less than standard ,Up to 300 messages per seconds
    - Exactly-Once Processing
    - First-In-First-Out Delivery
    - support up to 3,000 messages per second with batching
    - Name of the queue must end in .fifo
    - No per message delay (only per queue delay)
    - ability to do content based de-duplication
    - 5 min interval de-duplication using "Deduplication ID"
    - Message groups
      - possibility to group msgs for FIFO ordering using "message groupID"
      - one one worker can be assigned per msg group
      - msg group is just an extra tag in the msg
      - Queue attributes ( mostly same for both )
- Queue attributes  
    |Attribute| Range of values | default |
    |---------|-----------------|---------|
    |Visibility Timeout | 0 secs - 12 hrs| 30 secs|
    |Msg Retention period| 1 min - 14 day| 4 days|
    |Max Msg size| 1 - 256KB| 256KB|
    |Delivery delay| 0 secs - 15 mins| 0 secs|
    |Receive msg wait time | 0 secs - 20 secs| 0 secs|

    - Delivery delay
      - delivery delay per queue
      - delivery delay per message -> use message timers
    - Purge queue
      - delete all msg in the queue but retain the queue
      -  takes up to 60 seconds
    - Server-Side Encryption (SSE)
      - All requests to queues with SSE enabled must use HTTPS and Signature Version 4.
      - When you disable SSE, existing messages remain encrypted.
    - Long Polling
      - reduce the cost of using Amazon SQS by eliminating the number of empty responses and false empty responses and decreases the number of API calls
      - Set Receive Message Wait Time, type a number between 1 and 20 secs
      - Setting the value to 0 configures short polling
      - By default, Amazon SQS uses short polling
      - enabled at the queue level or at API level using WaitTimeSeconds
    - Dead-Letter Queue
      - queue that other (source) queues can target for messages that can't be processed (consumed) successfully
      - redrive policy : We can set a threshold of how many times a msg can go back to the SQS queue after which it will be send to the designated Dead letter queue
      - dead-letter queue of a FIFO queue must also be a FIFO queue
      -  dead-letter queue of a standard queue must also be a standard queue.
      - Make sure to process the messages in the DLQ before they expire
    - Visibility Timeout
      - prevents other consumers from processing the same message  till that message is getting processed by some consumer
      - ensure that you delete the message after processing to prevent the message from being received and processed again once the visibility timeout expires
      - ChangeMessageVisibility - API to change the visibility while processing a message
      - DeleteMessage - API to tell SQL that the message was successfully processed and should be deleted
    - Delay Queue
      - postpone the delivery of new message
      - any messages send to the delay queue remain invisible to consumers for the duration of the delay period.
      - default - 0sec, max -15 min
      - For standard queues, the per-queue delay setting is not retroactive—changing the setting doesn't affect the delay of messages already in the queue.
      - For FIFO queues, the per-queue delay setting is retroactive—changing the setting affects the delay of messages already in the queue.
      - At queue level: Can set a default (default is 0 seconds -> visible right away)
      - At message level: Can override the default using the DelaySeconds parameter.
    - Retention time
      - time till the message needs to be retains in the queue (default : 4 days , range - (1min - 14 days)
- Can configure Messages Arriving in an Amazon SQS Queue to Trigger an AWS Lambda Function
- SNS isn't currently compatible with FIFO queues.

### Amazon MQ
- managed message broker service for Apache ActiveMQ that makes it easy to set up and operate message brokers in the cloud.
- For lift & shift migrations:
  - Traditional applications running from on-premises may use open protocols such as MQTT, AMQP, STOMP
  - Instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
- It supports MQTT, AMQP, STOMP, Openwire, WSS, NMS, and WebSocket protocols
- Amazon MQ doesn't scale as much as SQS / SNS
  - Have to provision it
  - Broker's are deployed into e.g. mq.m5-xlarge, mq.t2.micro etc.
- Amazon MQ runs on a dedicated machine, can run in HA with failover
- Supports
  - Both queue feature (SQS) and topic features (SNS)
  - Both pull (SQS, Kinesis) and push based (SNS) messaging.
- Broker types
  - Single-instance broker: In single AZ
  - Active/standby broker: In two AZ's with automatic failover

### Kinesis
- Amazon Kinesis makes it easy to collect, process, and analyze video and data streams in real tim
- alternate for apache kafka
- Data is automatically replicated to 3 AZ
- Use-cases
  - "Real-time" big data e.g. application logs, metrics, IoT, clickstreams.
  - Streaming processing frameworks (Spark, NIFI, etc...)
- offerings
  - Kinesis streams : low latency streaming ingest at scale
  - kinesis Analytics : perform real-time analytics on streams using SQL
  - Kinesis firehose : load streams to S3,Redshift, ElasticSearch
  - Kinesis video streams: Process & analyze streaming media in real-time.
- flow
  - source(click streams/IoT/Metrics&logs) -> Amazon Kinesis Streams -> Amazon Kinesis Analytics -> Amazon Kinesis Firehose -> Sink (S3/redshift/elastic search)
- Kinesis Data Streams
  - Low latency streaming ingest at scale like Kafka.
  - Streams are divided in ordered Shards / Partitions
  - Producers -> Stream (Shard 1 / Shard 2 / Shard 3) -> Consumers
  - Pub/sub through streams (topic)
  - To scale: Add shards -> Increased throughput
  - Data retention -> 1 day by default, can go up to 7 days
  - Data
    - The message is base64-encoded blob
    - Data (before encoding) cannot exceed 1 MB
    - Ability reprocess / replay data as data is not removed after message is handled.
    - Immutable: Once data is inserted in Kinesis, it cannot be deleted
    - Batching available or per message calls
      - 💡 Use batching with PutRecords API to reduce costs and increase throughput
  - Shards
    - One stream is made of many different shards
    - The number of shards can evolve over time (re-shard / merge)
    - Records are ordered per shard
    - Partition key
      - Partition key gets hashed to determine the shard ID
      - Ensures ordering in a shard and same key always goes to same shard.
      - Choose a partition key that's highly distributed
      - Helps preventing hot partition or hot shard
    - Throughput
      - 1 MB/s or 1000 messages/s at write PER SHARD
      - 2 MB/s at read PER SHARD
      - ProvisionedThroughputExceeded if you go over the limits
        - Solution
          - Use retries with exponential back-off
          - Increase shards (scaling)
          - Ensure your partition key is a good one e.g. you don't have a hot shard
- Kinesis Firehose
  - Load data into Redshift / Amazon S3 / ElasticSearch / Splunk
  - auto scaling
  - Near real time (60 seconds latency)
- Kinesis Data Analytics
  - Perform real-time analytics on Kinesis Streams using SQL
  - Fully managed with auto scaling to match source stream throughput.
- Kinesis Video Streams
  - Ingest video streams for e.g. analytics and machine learning.
  - HTTP Live Streaming (HLS) enables you to playback video for live and on-demand viewing
  - Uses Amazon S3 as the underlying data store.


### SWF(Simple Workflow Service)
- Coordinate work amongst applications
- Code runs on EC2 (not serverless)
- 1 year max runtime
- One time only completion is key feature that ensures that a task is assigned only once and is never duplicated.
- Step functions is recommended to be used for new applications, except if you need:
  - external signals to intervene in the processes
  - child processes that return values to parent processes
  - long running up to 1 year
- Actors
  - Workflow starters: Any application that initiates workflow
  - Deciders: control flow activity tasks
  - Activity workers: carry out the activity tasks
- Steps: Activity, Decision, Human intervention
- Tasks
  - Activity task: Tell activity worker to execute its task.
  - Decision task: It tells the decider the state of the workflow execution.
  - Lambda task: Executes lambda
- Use case e.g. order fulfilment from web to warehouse to delivery.

### Step Functions
- Build serverless visual workflow to orchestrate your Lambda functions
  - E.g. Start => Submit Job => Wait X seconds => Get Job Status => Job Compete ? next : Go back to Wait X seconds -> End
- Represent flow as a JSON state machine
- Features: sequence, parallel, conditions, timeouts, error handling ...
- The steps of your workflow can exist anywhere, including in AWS Lambda functions, on Amazon EC2, or on-premises
- Maximum execution time of 1 year
- Possibility to implement human approval feature
- Use cases: any workflow
- Can use Callback patterns



## Security

### AWS Organizations
  - Consolidate multiple AWS accounts into an organization that you create and centrally manage
    - Enforce (SCP) policies centrally on root or OU's.
  - Grouping all of your AWS accounts into Organizational Units (OUs) as part of a hierarchy
  - Allows Consolidated billing: Multiple standalone accounts are combined and may reduce your overall bill
  - Allows you to create accounts programmatically.

### IAM
  - Global. Does not apply to regions
  - Identity Federation
    - Federating Users of Mobile/Web app using Cognito
      - Using Amazon Cognito will create identities(with the help of Mobile SDK) for users and sync across devices.
      - Web Identity Federation is an alternative to using Cognito but AWS recommends against it.
    - Federating Users with OpenID Connect or public identity service provider
      - Using third-party services like Facebook, Google or any identity providers(IdP) compatible with OIDC can federate users. But Amazon recommends using Cognito.
    - Federating users with SAML 2.0
      - If companies managing users with identity store like Microsoft Active Directory and Active Directory Federation Service can federate users with single-sign on (SSO) to aws management console. Company can create trust between organization as an identity provider (IdP) and AWS as the service provider if it is compatible with SAML 2.0
    - Federating users by creating a custom identity brokers
      - If identity store is not compatible with SAML 2.0 then custom identity broker application can be built.
  - Multifactor Authentication
    - If your MFA device is lost/damaged/notworking:
      - You can sign in using alternative methods of authentication.
  - IAM Query API to make direct calls to the IAM web service
  - IAM Entities
    - Principal is an entity(user or role) in AWS that can perform actions and access resources.
      - Principal is an entity in AWS that can perform actions and access resources.
      - IAM Users
        - Programmatic Access: ID + secret key
          - Mostly for machines.
          - Best practice for services is to assume roles
          - Assigned an Access Key ID and Secret Access Key when first created.
        - Console Access: Password
          - Ensure no one share same users by creating different users per person.
          - By default, newly created IAM users has no permissions.
      - IAM Roles
        - Use cases:
          - Create roles and can then assign them to AWS resources
          - Give cross-account access to users.
          - Instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it.
            - However for unique access of one you can delegate a role through:
              - In account add trust policy that specifies which trusted account members are allowed to assume the role.
              - The trust policy specifies which trusted account members (principals) are allowed to assume the role.
        - Credentials are rotated automatically -> Only temporary access.
          - Uses STS with expirations to generate secret id + secret key.
        - You can only assume one role a time
        - service-linked role
          - is a type of role that links to an AWS service (also known as a linked service) such that only the linked service can assume the role. Using these roles, you can delegate permissions to AWS services to create and manage AWS resources on your behalf.
          - A service-linked role can be assumed only by the linked service. This is the reason why the trust policy of a service-linked role cannot be modified.
      - IAM Groups
        - A collection of users under one set of permissions (policies)
        - A group is not an entity and cannot be identified as a principal in an IAM policy
      - IAM Policies      
        - JSON document that defines one (or more) permissions
        - **Resource-based**
          - control what actions a specified principal can perform on that resource and under what conditions. Resource-based policies are inline policies, and there are no managed resource-based policies.
          - controls who can access this resource
        - **Identity-based**
          - Managed policies
            - AWS managed policies
            - Customer managed policies
          - Inline policies
              - Policies that you create and manage and that are embedded directly into a single user, group, or role.
        - Overlap => Deny overrides allow.
        - Trust policies define which principal entities (accounts, users, roles, and federated users) can assume the role
        - Because an IAM role is both an identity and a resource that supports resource-based policies, you must attach both a trust policy and an identity-based policy to an IAM role.
        - To define resources ARN's are used (Amazon Resource Names) to uniquely identify AWS resources
        - Principle of Least Privilege: Always adhere to it when creating IAM user, restrict access with only resources/actions to do the job.
      - IAM Authentication to Services
        - Most of AWS services authenticates a user / role.
        - User / role (IAM) credentials must be sent in header
          - It's called Sig v4 AWS Signature Version 4 Signing Process
          - You sign all requests with signed using an access key (derivation of access key ID + secret access key) in header
        - All requests to AWS API's are signed automatically by SDK/CLI.

### Encryption key
- Two solutions currently exist for managing encryption keys:
  - Hardware security modules (HSM)
    - Designed and certified to be tamper-evident and intrusion-resistant, provide the highest level of physical security.
    - AWS CloudHSM
      - Cloud-based hardware security module (HSM)
      - Enables you to easily generate and use your own encryption keys on the AWS Cloud.
      - Lose the keys => no way to recover
  - KMS (Key Management Service)
    - encryption and key management service scaled for the cloud
    - integrated with CloudTrail, which provides you the ability to audit who used which keys, on which resources, and when.
    - Customer Master Keys (CMKs)
      - use a CMK to encrypt and decrypt up to 4 KB (4096 bytes) of data
      -  If data > 4 KB, use envelope encryption
      - use for envelope encryption : generate, encrypt, and decrypt the data keys that you use outside of AWS KMS to encrypt your data
      - KMS does not store, manage, or track your data keys.
      - | Type of CMK | Can view | Can manage | Used only for my AWS account |Price|
  | --- | --- | --- | --- |---|
  | Customer managed CMK | Yes | Yes | Yes |Monthly fee key + usage
  | AWS managed CMK | Yes | No | Yes |No monthly fee, just usage
  | AWS owned CMK | No | No | No |  Free
    - Data Keys
      - Data keys are encryption keys that you can use to encrypt data, including large amounts of data and other data encryption keys.
      - You can use AWS KMS customer master keys (CMKs) to generate, encrypt, and decrypt data keys.
      - KMS does not store, manage, or track your data keys, or perform cryptographic operations with data keys.
    - Envelope Encryption
      - When you encrypt your data, your data is protected, but you have to protect your encryption key. One strategy is to encrypt it. Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key.
      - You can even encrypt the data encryption key under another encryption key, and encrypt that encryption key under another encryption key. But, eventually, one key must remain in plaintext so you can decrypt the keys and your data. This top-level plaintext key encryption key is known as the master key.
      - Benefits of envelop encryption
        - Protecting data keys
        - Encrypting the same data under multiple master keys
        - Combining the strengths of multiple algorithms



### AWS STS - Security Token Service
  - Allows to grant limited and temporary access (permissions) to AWS resources
  - Token is valid for between 15 minutes to one hour (must be refreshed)
  - Used mostly for:
    - Generates tokens when assuming roles.
    - STS is common service to create credentials after identity validation
      - Identity validation can be done through SAML, Cognito or Custom Identity Broker application.
    - Cross Account Access
    - Identity federation


### AWS Single Sign On (SSO)
  - Centrally Managed Single Sign On across multiple AWS Accounts and Business Applications
  - One login gets you access to everything securely
  - Integrated with Microsoft Active Directory
  - Only helpful for Web Browser, SAML 2.0 enabled applications.

### AWS Directory Services

  - Simple AD
    - is a Microsoft Active Directory–compatible directory from AWS Directory Service that is powered by Samba 4
    - supports basic Active Directory
    - is a standalone directory in the cloud

  - Amazon Cognito
    - Managed identity broker for web identity federation
    - is a user directory that adds sign-up and sign-in to your mobile app or web application using Amazon Cognito User Pools.
    - scalable directory to manage and authenticate your subscribers and that works with social media identities.
    - Web Identity Federation is an alternative to using Cognito but AWS recommends against it.

  - Amazon Cloud Directory
    - is a cloud-native directory that can store hundreds of millions of application-specific objects with multiple relationships and schemas. Use Amazon Cloud Directory if you need a highly scalable directory store for your application’s hierarchical data.

  - AD Connector
    - AD Connector is a proxy service that provides an easy way to connect compatible AWS applications, such as Amazon WorkSpaces, Amazon QuickSight, and Amazon EC2 for Windows Server instances, to your existing on-premises Microsoft Active Directory.

  - AWS Managed Microsoft AD
    - AWS Managed Microsoft AD is your best choice if you need actual Active Directory features to support AWS applications or Windows workloads, including Amazon Relational Database Service for Microsoft SQL Server. It's also best if you want a standalone AD in the AWS Cloud that supports Office 365 or you need an LDAP directory to support your Linux applications.

#### AWS Artifact
  - AWS Artifact provides on-demand downloads of AWS security and compliance documents, such as AWS ISO certifications, Payment Card Industry (PCI), and Service Organization Control (SOC) reports.
  - You can submit the security and compliance documents (also known as audit artifacts)
  - follows Shared Responsibility Model (aws and customers)


####  AWS WAF - web application firewall
  - monitor web requests that are forwarded to API Gateway, Amazon CloudFront or an Application Load Balancer
  - Useful for preventing
    - cross-site scripting.
    - IP addresses or address ranges that requests originate from
    - Country or geographical location that requests originate from
    - Length of specified parts of the request, such as the query string
    - SQL injection.
    - Strings that appear in the request eg User-agent

####  AWS Shield
  - protection against DDoS (distributed denial of service ) attacks
  - against many types of attacks
    - User Datagram Protocol (UDP) reflection attacks
    - SYN flood
    - DNS query flood
    - HTTP flood/cache-busting (layer 7) attacks

####  Amazon GuardDuty
  - is a continuous security monitoring service that analyzes and processes the following data sources: VPC Flow Logs, AWS CloudTrail event logs, and DNS logs
  - It uses threat intelligence feeds, such as lists of malicious IPs and domains, and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment.

####  Amazon Inspector
  -  is a security vulnerability assessment service that helps improve the security and compliance of your AWS resources.
  - Amazon Inspector automatically assesses resources for vulnerabilities or deviations from best practices, and then produces a detailed list of security findings prioritized by level of severity. Amazon Inspector includes a knowledge base of hundreds of rules mapped to common security standards and vulnerability definitions that are regularly updated by AWS security researchers.

####  AWS Firewall Manager
  - AWS Firewall Manager simplifies your AWS WAF administration and maintenance tasks across multiple accounts and resources. With AWS Firewall Manager, you set up your firewall rules just once. The service automatically applies your rules across your accounts and resources, even as you add new resources.
  - AWS Firewall Manager simplifies your AWS WAF and AWS Shield Advanced administration and maintenance tasks across multiple accounts and resources

####  AWS Secrets Manager
  - AWS Secrets Manager helps you to securely encrypt, store, and retrieve credentials for your databases and other services. Instead of hardcoding credentials in your apps, you can make calls to Secrets Manager to retrieve your credentials whenever needed. Secrets Manager helps you protect access to your IT resources and data by enabling you to rotate and manage access to your secrets.

####  AWS Single Sign-On
  - AWS Single Sign-On is a cloud-based service that simplifies managing SSO access to AWS accounts and business applications. You can control SSO access and user permissions across all your AWS accounts in AWS Organizations. You can also administer access to popular business applications and custom applications that support Security Assertion Markup Language (SAML) 2.0. In addition, AWS SSO offers a user portal where your users can find all their assigned AWS accounts, business applications, and custom applications in one place.

####  Amazon Macie
  - Amazon Macie is a security service that uses machine learning to automatically discover, classify, and protect sensitive data in AWS. Amazon Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property, and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved.

####  AWS Resource Access Manager
  - AWS Resource Access Manager (AWS RAM) enables you to share your resources with any AWS account or organization in AWS Organizations. Customers who operate multiple accounts can create resources centrally and use AWS RAM to share them with all of their accounts to reduce operational overhead. AWS RAM is available at no additional charge.



## Database

### RDS
- managed service for below supported dbs
  - MySQL
  - MariaDB
  - PostgreSQL
  - Microsoft SQL Server
- support multi-az deployment - high availability/failover
- no shell access to db instance
-  Enhanced Monitoring to Identify Operating System Issues
- The high-availability feature is not a scaling solution for read-only scenarios; you cannot use a standby replica to serve read traffic
- DB instance storage
  - Magnetic
  - General Purpose (SSD)
  - Provisioned IOPS (PIOPS)
- Failover
  - primary DB instance switches over automatically to the standby replica if any of the following conditions occur:
    - An Availability Zone outage
    - The primary DB instance fails
    - The DB instance's server type is changed
    - the operating system of the DB instance is undergoing software patching
    - A manual failover of the DB instance was initiated using Reboot with failover
- RDS Read Replicas for read scalability
  - asynchronously (so eventually consistent)
  - scale-out
  - Cannot be auto-scaled.
  - Up to 5 Read Replicas
  - Within AZ, Cross AZ Application or Cross Region
  - Replicas can be promoted to their own DB
  - use connection string to leverage read replicas
- Promoting a Read Replica to Be a Standalone DB Instance
  - when  ?
    - Performing DDL operations (MySQL and MariaDB only)
    - Sharding
    - Implementing failure recovery
- Auto-scaling
  - For horizontal scaling: Not available for read replicas
  - For vertical scaling: RDS Storage Auto Scaling automatically scales storage capacity in response to growing database workloads, with zero downtime.
- RDS Backups
  - automatically enabled in RDS
  - 7 days retention (can be increased to 35 days)
- DB Snapshots
  - Manually triggered by the user
  - Retention of backup for as long as you want
- RDS Encryption
  - Encryption at rest capability with AWS KMS - AES-256 encryption
  - Can only be enabled when you first create the DB instance
  - You can however: unencrypted EB -> snapshot -> copy snapshot as encrypted -> create DB from snapshot
  - Transparent Data Encryption
    - TDE is an encryption type which encrypts the data and log files of a database.
      - It means encrypting databases both on the hard drive and consequently on backup media.
      - Enabling TDE also encrypts your tempdb.
  - SSL certificates to encrypt data to RDS in flight
  - To enforce SSL:
    - PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Paratemer Groups)
    - MySQL: Within the DB:GRANT USAGE ON . TO 'mysqluser'@'%' REQUIRE SSL;
- DB Parameter Groups
  - manage your DB engine configuration by associating your DB instances with parameter groups.
  - container for engine configuration values that are applied to one or more DB instances
  - Changing groups = recreate a new group & assign (you can copy existing)
- Storage
  - can't reduce the amount of storage for a DB instance after it has been allocated.
  - can change the type of storage for your DB instance
- MySQL & PostgreSQL engines
    - MyISAM (storage engine)
      - Use for intense, full-text search capability,
      - Not recommended overall
    - InnoDB is recommended (better crash recovery)
      - Compatible with Aurora

- **Multi AZ vs Read Replicas**

  | Attribute | Multi-AZ Deployments | Read Replicas |
  | --------- | -------------------- | ------------- |
  | **Replication** | Synchronous replication, Durable | Asynchronous replication, Scalable |
  | **Accessibility** |  Only primary instance is active |  Accessible and can be used for read scaling |
  | **Backups** |  Automated backups are taken from standby (no I/O on primary) | No backups configured by default |
  | **Zone** | Always span two Availability Zones within a single Region | Can be within an Availability Zone, Cross-AZ, or Cross-Region |
  | **DR** | Automatic failover to standby when a problem is detected | Can be manually promoted to a standalone database instance |

- Other important points
  -  If you want to copy database: Restore from a snapshot, the database will have schemas and ready & will be quicker than big create query.
  - When rebooting you can select to fail-over to other AZ.
  - Backtrack rewinds the DB cluster to the time you specify

### Aurora
- Compatible API for PostgreSQL / MySQL
- Automatic fail-over: Failover in Aurora is instantaneous. It's HA native.
- Auto-expanding: Automatically grows in increments of 10 GB, up to 64 TB
- Capacity types
  - Provisioned: Provision and manage the instance sizes
  - Serverless: Specify min and max resources and Aurora auto-scales
  - Multi-AZ deployment: Through replicas in different zone
- Can enable Enhanced monitoring for more metrics.
- Deletion protection: You can't delete the database (protects from accidental deletions)
- Pricing
  - Aurora costs more than RDS (20% more) but more efficient
  - Billed per IO.
    - Read IO: Every database page read operation is 1 IO.
    - Write IO: Pushing transaction logs to shared storage for high availability.
- Improve query performances
  - Hash Joins: If you need to join a large amount of data by using an equijoin.
  - Asynchronous key prefetch (AKP) Improve the performance of queries that join tables across indexes
- Aurora DB Cluster
  - Cluster-types
    - Multi-master clusters - All DB instances have read-write capability.
    - Has two instances types:
      - Single-master clusters
        - Primary DB instance: master
          - Each Aurora DB cluster has one primary DB instance.
          - You can use MySQL / PostgreSQL RDS as master in a cluster
        - Aurora Replica
          - Read-only replicas of master
  - Cluster endpoints
    - Reader endpoint: Read replicas
    - Writer endpoint: Master
      - If master fails, clients still talk to the endpoint and will be redirected to the right instance
    - Custom Endpoints: E.g. point analytics workload to higher memory capacity replicas
  - Best practice: connect writer endpoint to write and reader endpoint to read.
- Availability
  - Automated backups are always enabled.
    - use Backtrack to restore data at any point of time without using backups
    - You can also take snapshots & share with other AWS accounts without impacting performance.
  - Faster RTO than other RDS engines by making buffer cache of DB immediately available.
  - **Multi-AZ** by default with 6 copies (replicas) across 3 AZ:
  - You can have multi-region replicas
  - Self healing with peer-to-peer replication
    - Auto healing i.e. Aurora will recover if we lose one replica
  - One Aurora Instance takes writes (master)
  - Automated failover for master in less than 30 seconds
  - Failover behavior
    - You can failover to any replica -> Secondary becomes the master, replication breaks.
      - You can assign priority for replicas, or the one which has same size as primary will be more prioritized.
    - Single instance: Create a new DB in the same AZ, if it fails => try different AZ
      - RTO: 15 min
    - Multi replicas: Flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica
      - RTO: 15 seconds
  - Aurora Global databases
    - Span multiple regions (global)
    - One primary region, one DR region (can be used for lower latency read)
    - < 1 second replica lag on average
    - If not using Global Databases, you can create cross-region Read Replicas
      - Global databases are recommended: much faster replicas.
- Scalability
  - Vertical Scaling
    - Storage Scaling
      - No need to provision
      - Auto-scales up to 64 TB, in 10GB increments
      - Zero-impact to database performance when scaling.
    - Compute resources
      - Can happen during maintainance window
      - Or immediately (a few minutes of downtime)
  - Horizontal Scaling
    - Write Scalability
      - Multi-Master: create multiple read-write instances across multi-AZ.
    - Read Scalability
      - Aurora can have 15 replicas (+ master)
      - Support for Cross Region Replication
      - All replicas and master uses a shared storage that's auto expandable
      - Read Replicas can be global
      - You can use Aurora Replicas as failover targets.
      - Auto-scaling is done through **Auto Scaling policy**
  - Shared storage architecture
    - Makes your data independent from the DB instances in the cluster.
    - You can add a DB instance quickly because Aurora doesn't make a new copy of the table data
    - Storage is striped across 100s volumes
    - All read replicas connects to Reader Endpoint
- Security
  - Encryption at rest using KMS
  - Encryption in flight using SSL (same process as MySQL or Postgres)
  - Can use IAM authentication for Aurora MySQL and Postgres
- Aurora Serverless
  - No need to choose an instance size -> it auto-scales for you
  - Helpful when you can't predict the workload
  - DB cluster starts, shutdown and scales automatically based on CPU / connections
  - Can migrate from Aurora Cluster to Aurora Serverless and vice versa
    - Through restoring snapshots
  - Aurora Serverless usage is measured in ACU (Aurora Capacity Units)
    - Billed in 5 minutes increment of ACU
- Aurora parallel query
  - Ability to push down and distribute the computational load of a single query across thousands of CPUs in Aurora’s storage layer
  - Can be enabled/disabled globally and on session level with `aurora_pq` parameter.
  - Causes higher I/O costs as it scans all data
  - Not available for serverless, Backtrack-enabled instances, and non R-* instances.


### Elastic Cache
- Key-value store
- distributed in-memory databases
- Write Scaling using sharding
- Multi AZ with Failover Capability
- Patterns for ElastiCache
  - Lazy Loading: all the read data is cached, data can become stale in cache
  - Write Through: Adds or update data in the cache when written to a DB (no stale data)
  - Session Store: store temporary session data in a cache (using TTL features)
- None of the caches support IAM authentication
- Use case: Key/Value store, Frequent reads, less writes, cache results for DB queries (writethrough pattern), store session data for websites, cannot use SQL.
- Memcached
  - In-memory object store
  - Cache doesn't survive reboots
  - Memcached support SASL authentication
- Redis
  - Super low latency (sub ms)
  - Read Scaling using Read Replicas
  - Cache survive reboots by default (persistent)
  - Use-cases: User sessions, Leaderboard (for gaming) with Redis Sorted Sets, Distributed states, Relieve pressure on databases (such as RDS), messages with Redis Pub/Sub, recommendation data with Redis Hashes
  - Multi-AZ with Auto-Failover and enhanced robustness
  - Encryption at rest (KMS) and in-transit (using SSL)
  - Redis support Redis AUTH (username / password with --auth-token parameter)

### Redshift
- Redshift = Data warehouse = Analytics / BI
- Columnar database
  - Good for OLAP (online analytical processing)
  - Makes compression very easier
- Used to pull in very large and complex data sets.
- Heavily modified version of PostgreSQL
- Scaling
  - Can scale to PBs of data
  - From 1 node to 128 nodes, up to 160 GB of space per node
    - Leader node: for query planning, results aggregation
    - Compute node: for performing the queries, send results to leader
- Massively Parallel Query Execution (MPP) database.
- Good practice -> Create read replica from RDS and pull the data from the read replica and load it into Redshift
- Copy between regions: Take snapshot => Copy snapshot to new region => Create cluster from snapshot
- Redshift Spectrum: serverless service to perform queries.Efficiently query and retrieve structured and semistructured data from files in Amazon S3 without having to load the data into Amazon Redshift tables directly against S3 (no need to load)
- Redshift Enhanced VPC Routing: COPY / UNLOAD goes through VPC
- No multi AZ, only available in 1 AZ
  - Snapshots can be deployed to new AZ for e.g. DR.
- Backups are enabled by default, max 35 days.
- Have functionality for Automated Cross-Region Snapshot.
- Attempts to maintain at least three copies of your data.
  - Original, replica on the compute nodes, and a backup in S3.

### DynamoDB
- AWS proprietary technology, managed NoSQL database
- Serverless, provisioned capacity, auto scaling, on demand capacity
- schemaless
- TTL - allows you to delete expired items from tables automatically to help you reduce storage usage and the cost of storing data that is no longer relevant.
- Can replace ElastiCache as a key/value store
  - Advantage is that it's serverless e.g. no provisioning & maintenance
  - Not sub millisecond (as ElasticSearch) performance but single digit (1-9 millisecond) performance.
- Availability
  - Multi AZ in 3 data centers by default
  - Backup and Restore: Point in time restore like RDS without performance impact
- Read & Writes are decoupled: you can provision read and write capacity units
- Consistency
  - Reads can be eventually consistent or strongly consistent
    - Eventually consistent: accept stale data
    - Strongly consistent: get always newest data
  - (Optionally) Transactions for writes
- Can only query on primary key, sort key or indexes
- Use cases:
  - Serverless applications development (small documents 100s KB)
  - Distributed serverless cache
  - Doesn't have SQL query language available
  - Has transactions capability
  - Session store
- Best Practices
  - Keep item sizes small
  - Store more frequently and less frequently accessed data in separate tables
    - Allows you to set appropriate provisioning levels independently for the tables
  - If possible compress larger attribute values
  - Store objects larger than 400KB in S3 and use pointers (S3 Object ID) in DynamoDB
- Security
  - VPC Endpoints available to access DynamoDB without internet
  - VPC Endpoint = an endpoint network interface (ENI)
  - Access fully controlled by IAM
  - Encryption at rest using KMS
  - Encryption in transit using SSL / TTLS
- Scaling options
  - On-demand
    - Auto scales by Application Auto Scaling
    - Scaling policy
      - Whether to scale read or/and write capacity.
      - Minimum and maximum provisioned capacity unit
      - Supports setting target utilization with target tracking.
    - 2.5x more expensive than provisioned capacity (use with care!)
  - Provisioned RCU (read compute unit) & WCU (write compute unit)
    - good for predictable application traffic.
    - They're decoupled can be increased / decreased individually
    - Allows you to control costs for predictable workloads.
    - 1 RCU
        - 1 strongly consistent read request = 2 eventually consistent read request => read of 4KB
    - 1 WCU = 1 write of 1 KB
    - Throughput can be exceeded temporarily using burst credit
      - If burst credit are empty, you'll get ProvisionedThroughputException.
      - It's then advised to do an exponential back-off retry
- DynamoDB Tables
  - Maximum size of an item is 400 KB
  - Each table has a primary key
  - Local secondary index
    - = Partition key same as the base table + different sort key.
  - Global secondary index
    - = Partition key (optionally + sort key) that can be different from those on the base table.
  - All indexes requires unique partition key and a sort key
- DynamoDB Streams
  - Generates changelogs for every operation
  - Enables event driven programming by integrating to AWS Lambda.
  - Can implement cross region replication using Streams
  - Stream has 24 hours of data retention
- DAX: DynamoDB Accelerator
  - cache for DynamoDB
  - Micro second latency for cached reads & queries
  - Solves Hot Key problem (too many reads)
  - 5 minutes TTL for cache by default
  - Multi AZ
  - Writes goes through DAX to DynamoDB for caching
- Global Tables
  - You must enable first Dynamo Streams
  - Global Tables are multi region, fully replicated, high performance
  - If data is updated from multiple regions last writer wins to ensure eventual consistency


### DocumentDB
  - Document database with Mongo DB compatibility.
  - More hands on DynamoDB => Middle step between MongoDB and DynamoDB

### Neptune
  - Fully managed graph database
  - Highly available across 3 AZ (Multi-AZ) , with up to 15 read replicas (Clustering)
  - Point-in-time recovery, continuous backup to Amazon S3
  - Support for KMS encryption at rest + HTTPS
  - Use-cases: • High relationship data • Social networking: e.g. Users friends with Users, replied to comment on post of user and likes other comments • Knowledge graphs (e.g. Wikipedia)

### ElasticSearch
  - Indexes & allows you to search any field, even partially matches in unstructured data.
  - It's common to use ElasticSearch as a complement to another database
  - ElasticSearch also has some usage for Big Data applications
  - You can provision a cluster of instances
  - Built-in integrations: Amazon Kinesis Data Firehose, AWS IoT, and Amazon CloudWatch Logs for data ingestion
  - Security through Cognito & IAM, KMS encryption, SSL & VPC
  - Comes with Kibana (visualization) & Logstash (log ingestion) - ELK stack

## Compute

### EC2
- Pricing
  - On-Demand Instances
    - Pay for the instances that you use by the second, with no long-term commitments or upfront payments.Fixed rate by hour(or sec)
  - Reserved Instances
    - Make a low, one-time, up-front payment for an instance, reserve it for a one- or three-year term, and pay a significantly lower hourly rate for these instances.
    - Types
      - Standard Reserved Instance
        - Some attributes, such as instance size, can be modified during the term; however, the instance family cannot be modified. You cannot exchange a Standard Reserved Instance, only modify it
        - Can be sold in the Reserved Instance Marketplace.
      - Convertible Reserved Instance
        - Can be exchanged during the term for another Convertible Reserved Instance with new attributes including instance family, instance type, platform, scope, or tenancy.
    - Instance Attributes that determine pricing
      - Instance type
      - Region
      - Tenancy (shared / dedicated)
      - Platform (OS type)
  - Spot Instances
    - Request unused EC2 instances, which can lower your costs significantly.
  - Dedicated hosts
    - physical ec2 server dedicated for you.Can help you to reduce the cost by allowing you to use your existing server-bound software licenses
  - Dedicated Instances
    - Pay, by the hour, for instances that run on single-tenant hardware.
  - Savings Plans
    - Reduce your Amazon EC2 costs by making a commitment to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years.
  - Scheduled Instances
    - Purchase instances that are always available on the specified recurring schedule, for a one-year term.
  - Capacity Reservations
    - Reserve capacity for your EC2 instances in a specific Availability Zone for any duration.
- Billing
  - Billed only when in running state.
    - And stopping state only if it's hibernating.
  - Data transfer costs
    - Free between EC2 and other services.
    - Free between EC2 instances in same availability zone using private IP addresses only.
    - $0.01/GB for transfer between instances in different availability zones
- Elastic Network Interface (ENI)
  - Each EC2 has one default ENI.
  - Each ENI can optionally have one Elastic IP (static IP).
  - You can attach a network interface to an EC2 instance in the following ways:
    - **Hot attach**: When it's running
    - **Warm attach**: When it's stopped
    - **Cold attach**: When the instance is being launched
  - Termination with instance termination
    - Default interfaces are terminated with instance termination.
    - Manually added interfaces are not terminated by default
- You can enable termination protection (termination = deletion)
  - Prevents an instance from being accidentally terminated by requiring that you disable the protection before terminating the instance
- EC2 Instance Metadata
  - **Meta data** = Info about the EC2 instance at `http://169.254.169.254/latest/meta-data`
  - **User data** = launch/bootstrap script of the EC2 instance http://169.254.169.254/latest/user-data
- Enhanced Networking
  - can be enabled on specific instances with a driver to reduce networking overhead + faster networking.
  - Single root I/O virtualization (SR-IOV)
  - Utilizes Elastic Network Adapter (high Performance Network Interface for Amazon EC2)
- Launch templates
  - Store launch parameters so that you don't specify them every time you launch an instance.
- An instance may immediately terminate if:
  - You've reached your EBS volume limit
  - An EBS snapshot is corrupt
  - The root EBS volume is encrypted and you do not have permissions to access the KMS key for decryption
  - The instance store-backed AMI that you is missing a required part (an image.part.xx file)

- EC2 AMIs
  - Two virtualization types
    - HVM VM - Hardware virtual machine
      - Utilizes special hardware extensions as it's on bare metal hardware.
      - Only option for Windows.
    - PV VM - Paravirtual virtual machines, only available for Linux
    - HVM is recommended because same or or better performance than paravirtual guests.
  - Custom AMIs
    - AMI are built for a specific AWS region
    - An AMI created for a region can only be seen in that region.
      - Copying makes it available in different regions for e.g. disaster recovery.
- Common tasks
  - Copying an AMI
  - Create AMI from EC2
  - Cross account / cross region AMI Copy
    - Share AMI with another AWS account
    - Ensure you have read permissions for AMI storage:
      - Associated EBS snapshot (for an Amazon EBS-backed AMI)
      - Associated S3 bucket (for an instance store-backed AMI)
  - You can't copy:
   - Encrypted AMI shared from another account
    - Instead you can copy the snapshot (if it's shared with encryption key) and re-encrypt it & register as a new AMI you own.
  - AMI with an associated billingProduct shared from another account
    - They're Windows AMIs and AMIs from the aWS marketplace
    - You can launch an EC2 instance in your account using the shared AMI -> Create an AMI from the instance
  - By default, the backing snapshot of an AMI is copied with its original encryption status , Encrypted-to-unencrypted is not supported
- EC2 Auto Scaling
  - An Auto Scaling group can contain EC2 instances in one or more Availability Zones within the same Region.However, Auto Scaling groups cannot span multiple Regions.
 - attempts to distribute instances evenly between the - Availability Zones that are enabled for your Auto Scaling group
 - The following actions can lead to rebalancing activity:
    - You change the Availability Zones for your group.
    - You explicitly terminate or detach instances and the group becomes unbalanced.
    - An Availability Zone that previously had insufficient capacity recovers and has additional capacity available.
    - An Availability Zone that previously had a Spot market price above your Spot bid price now has a market price below your bid price.
  - Default Termination policy (Scale -in )
    - If there are uneven instances in multiple AZ -> select the AZ with most instances
    - then Select the instances with the oldest configuration
    - then select the instances closest to the next billing hour
    - then select random
  - Scaling Cooldowns
    - The cooldown period helps to ensure that your Auto Scaling group doesn't launch or terminate additional instances before the previous scaling activity takes effect. You can configure the length of time based on your instance warmup period or other application needs.
    - default time is 300 secs
  - Scaling policies
    - Types
      - Target tracking scaling - Increase or decrease the current capacity of the group based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home – you select a temperature and the thermostat does the rest.
      - Step scaling - Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
      - Simple scaling - Increase or decrease the current capacity of the group based on a single scaling adjustment.
    - If you are scaling based on a utilization metric that increases or decreases proportionally to the number of instances in an Auto Scaling group, then it is recommended that you use target tracking scaling policies. Otherwise, it is better to use step scaling policies instead.

- EC2 Placement Groups
  - Use to control how EC2 instances will be placed within AWS infrastructure.
  - Works only for certain types of instances
  -  You can't merge placement group
  - You can't move existing EC2 to placement group -> you can create AMI and launch new.
  - AWS recommends homogenous instances within placement groups e.g. using same instance types
  - Placement Group Strategies
    - Cluster
      - Clusters instances into a low-latency group in a single Availability Zone
      - Cannot span multiple AZ
      - High performance, high risk
      - Same rack & same AZ -> If the rack fails, all instances fails at the same time.
      - Use cases
        - Big Data job that needs to complete fast
        - Application that needs extremely low latency and high network throughput.
    - Spread
      - Spreads instances across underlying hardware (max 7 instances per group per AZ)
        - Ensures all instances are located in different hardware.
      - Can span across multi AZ.
      - Minimizes failure risks.
    - Partition
      - Spreads instances across many different partitions (which rely on different sets of racks) within an AZ.
      - Partitions are isolated from each other from hardware failures in different racks but not VMs
      - Can span across multi AZ.
      - Up to 7 partitions per AZ
      - EC2 instances get access to the partition information as metadata
        - Use cases: HDFS, HBase, Cassandra, Kafka

### Elastic Container Service
-  is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster of Amazon EC2 instances.
- Launch types
  - Fargate launch type - serverless
  - EC2 launch type - more control
- ALB Integration
  - Application Load Balancer (ALB) has a direct integration feature with ECS called "port mapping"
  - Allows you to run multiple instances of the same application on the same EC2 machine
  - Dynamic port mapping is not available in classic load balancer and allows the same port to be mapped to many.
E.g. containers on port 6789, 9586, 3748 can be exposed on port 80 /443 by application balancer.

### Lambda
- you can run code without provisioning or managing servers
- pay only for the compute time that you consume
- lambda takes care of scaling and availability
- AWS Lambda Limits
  - Concurrent executions : 1000
  - Function layers : 5
  - Deployment package : 50MB zipped, 250MP unzipped , 3 MB (console editor)
  - Memory allocation : 128 MB - 3008 MB ( 64 increments)
  - Max execution time : 15 mins
  - Disk capacity in the "function container" ( in /tmp) :512MB
- Timeout : default 3 secs , max 15 mins
- Allocated memory ( 128MB to 3GB) (64MB increments)
- Using AWS Lambda with CloudFront Lambda@Edge
  - Lambda@Edge lets you run Node.js Lambda functions to customize content that CloudFront delivers, executing the functions in AWS locations closer to the viewer
  - The functions run in response to CloudFront events, without provisioning or managing servers.
- Lambda authorizers
  - is a Lambda function that you provide to control access to your API.
  - Lambda function is invoked with a request context or an authorization token that are provided by the client application.

### AWS Batch
  - AWS Batch enables you to run batch computing workloads on the AWS Cloud. Batch computing is a common way for developers, scientists, and engineers to access large amounts of compute resources. AWS Batch removes the undifferentiated heavy lifting of configuring and managing the required infrastructure.
  - Enables you to run batch computing workloads on the AWS Cloud.
  - It is a regional service that simplifies running batch jobs across multiple AZs within a region.
  - There is no additional charge for AWS Batch


## Deployment


### Deployment method    
- Types
  - In-place
    - An in-place upgrade involves performing application updates on live Amazon EC2 instances
  - Disposable
    - involves rolling out a new set of EC2 instances by terminating older instances.
  - Blue-Green Method
    - Blue-green is a method in which you have two identical stacks of your application running in their own environments. You use various strategies to migrate the traffic from your current application stack (blue) to a new version of the application (green)
    - 0 downtime
    - new version deployed on new instances
    - Traffic shift
      - Canary: Traffic is sifted in two increments,eg. 90% to first version, 10% to second before 100% to second.
      - Linear: Traffic is shifted in equal increments with number of minutes e.g. 10% per minute.
      - All-at-once: All traffic is shifted at once.
  - All at once
    - deployment on existing instances
    - total downtime
  - Rolling
    - deployment on existing instances
    - downtime for batch of instances
  - Immutable
    - deployment on new instances
    - 0 downtime

### CodeDeploy
- not autoscalable
- useful for in-place deployment
- if you want to deploy code to infrastructure managed by yourself
- easily reuse existing configuration management scripts.

### OpsWork
- useful for in-place deployment
- managed service for Chef & Puppet
- leverages "Recipes" or "Manifests"
- OpsWorks stacks
  - Groups applications into layers depending on Chef recipes
  - No need to provision chef server
  - Uses the embedded Chef solo client that is installed on EC2 instances on your behalf

### AWS SSM (Systems Manager)
- Serverless & free & open-source
- Is built-in in AWS AMIs
- Can run Ansible, PowerShell DSC or any script from S3 through **Run command**.
- Can use AWS Systems Manager Automation

### ElasticBeanStalk
- quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications.
-  PaaS-like layer ontop of AWS's IaaS services which abstracts away the underlying EC2 instances, Elastic Load Balancers, auto scaling groups, etc.
- intended to make developers' lives easier.

### CloudFormation
- Infrastructure as code
- versioned
- json/yaml format
- Allows re-use the best practices around your company for configuration parameters.
- Templates
  - JSON or YAML text file that contains instructions for building out AWS environment
  - You can upload in S3 and then reference in CloudFormation
  - Update a template
    - Can't edit previous ones.
    - Have to re-upload a new version of the template to AWS.
- Building Blocks
  - Cloud formation template consists of components and helpers:
    - Template components
      - Resources: your AWS resources declared in the template (mandatory)
      - Parameters: the dynamic inputs for your template
      Allows you to parameterize & prompt inputs while deploying.
      - Mappings: the static variables for your template
      - Outputs: References to what has been created
      - Conditionals: List of conditions to perform resource creation
      - Metadata
    - Template helpers
      - References (Logical IDs are used to reference resources within the template)
      - Functions
- Stacks
  - The entire environment described by the template and created, updated, and deleted as a single unit
  - Deleting a stack -> deletes every artifact created by CloudFormation
  - Redeploying a stack deletes old instances.
  - Changing a stack using change sets
  - Drift: difference between the expected configuration values and actual deployed.
  - Stack policies prevents stack resources from unintentionally being updated or deleted during stack updates
- Change sets
  - Before making changes to your resources, you can generate a change set, which is a summary of your proposed changes, e.g. resource A will be deleted.


## Disaster Recovery Overview
- Terms
  - RPO: Recovery Point Objective (last checkpoint)
  - RTO: Recovery Time Objective (time to recover after disaster strikes)
  - RPO -> disaster -> RTO
  - time between disaster and RPO => data loss
  - time between RTO and disaster time => recovery time
- Disaster Recovery Strategies
  - **Backup and Restore** (Data backed up regularly and restored on disaster, $, RPO/RTO in Hrs)
  - **Pilot Light** (only critical components run in cloud, RPO/RTO in 10s of Mins, $$ )
  - **Warm Standby** (Full system is up and running, but at minimum size, RPO/RTO in Mins, $$$)
  - **Hot Site / Multi Site Approach**(Full Production Scale is running AWS and On Premise, RPO/RTO in real-time/secs, $$$$)
- Faster RTO
  - Hot Site / Multi Site Approach > Warm Standby > Pilot Light > Backup & restore


## Developer Tools
  - Cloud9
    - cloud-based integrated development environment (IDE) that you use to write, run, and debug code.
  - CodeBuild
    - fully managed build service that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy.
  - CodeCommit
    - version control service that enables you to privately store and manage Git repositories in the AWS cloud.
  - CodeDeploy
    - deployment service that enables developers to automate the deployment of applications to instances and to update the applications as required.
  - CodePipeline
    - continuous delivery service that enables you to model, visualize, and automate the steps required to release your software
  - CodeStar
    - lets you quickly develop, build, and deploy applications on AWS.
  - X-Ray
    - makes it easy for developers to analyze the behavior of their distributed applications by providing request tracing, exception collection, and profiling capabilities.



## Monitoring

### CloudWatch
  - Metrics
    - Up to 10 dimensions per metric
    - 14 days retention, Extended retention offering allows up to 15 months.
    - EC2 Detailed Monitoring
      - EC2 instance metrics have metrics "every 5 minutes"
        - With detailed monitoring (for a cost), you get data "every 1 minute"
      - Use detailed monitoring if you want to more prompt scale your ASG
      - EC2 memory usage is by default not pushed
        - Must be pushed from inside the instance as a custom metric with e.g. CloudWatch agent.
    - Custom Metrics
      - Metric resolution
        - Standard: 1 minute
        - High resolution: up to 1 second (StorageResolution API parameter)
          - Higher cost
        - Period (retention)  
          - 1 sec  - (for 3 hours)
          - 1 minute  - (for 15 days)
          - 5 minute  - (for 63 days)
          - 1 hour  - (for 15 months)
      - Use exponential back off in case of throttle errors    
  - Dashboards
    - Can create CloudWatch dashboard of metrics
    - Can be regional or global
  - Logs
    - Applications can send logs to CloudWatch using the SDK
    - CloudWatch can collect log from Elastic Beanstalk, ECS, AWS Lambda, VPC Flow Logs, API Gateway, CloudTrail, CloudWatch log agents, Route53 and more.
    - CloudWatch logs can go to:
      - Batch exporter to S3 for archival
      - Stream to ElasticSearch cluster for further analytics
      - Stream to Lambda
    - You need to store logs in 2 things:
      - Log groups: arbitrary name, usually representing an application
      - Log stream: instances within application / log files / containers
    - Can define log expiration policies
    - CloudWatch Logs Insights
  - Alarms
    - Alarms are used to trigger notifications for any metrics
    - Alarms invokes actions such as:
      - EC2 Actions: e.g. restart EC2.
      - SNS Notifications: email, SMS, etc.
      - Auto Scaling: triggers Auto Scaling policies.
  - Events
    - Event Rule
      - Types
        - Schedule: Notifications that'll be triggered on demand
        - Event Pattern: React to service doing something e.g. CodePipeline state changes.
      - Targets: e.g. lambda function, EC2 StopInstances API call, SNS, SQS, ECS Task, Event bus in another AWS account...
      - Triggers to Lambda functions, SQS/SNS/Kinesis Messages
      - CloudWatch Event creates a small JSON document to give information about the change

### CloudTrail
  - CloudTrail tracks api and reports on who made the change, when, and from which location.
  - Enabled by default
  - Encryption
    - A single KMS key can be used to encrypt log files for trails applied to all regions.
    - CloudTrail log files are encrypted using S3 Server Side Encryption (SSE)
    - You can also enable encryption SSE KMS for additional security
    - Can push to S3 (encrypted by default), CloudWatch Logs and SNS.
    - Has 90 days of retention
    -  If a resource is deleted in AWS, look into CloudTrail first!

### AWS Config
  - Assess, audit, and evaluate the configurations of your AWS resources.
    - Resources include RDS, subnets, DB snapshots, security groups, and event subscriptions.
  - Reports on what has changed
  - Tracks resource state
  - AWS Config is around compliance, Trusted Advisor is more around recommendations but they check same things for security.
  - Integrates with SNS to receive notifications.

### AWS Trusted Advisor
  - High level AWS account assessment, very useful to know whether our account is sticking to AWS' best practices.
  - Serverless global service
  - Analyzes your AWS accounts and provides AWS-defined recommendations based on core checks:
    - Cost Optimization
    - Performance
    - Security
    - Fault Tolerance
    - Service Limits

### AWS Budgets
- Gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.
- Used to budget costs **before** they have been incurred

### AWS Cost Explorer
- Easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time.
- Used to explore costs **after** they have been incurred
- also for forcasting costs



## Other services

### Simple Email Service (SES)
  - Bulk & secure transactional email-sending service.
  - for Third parties e.g. customers
  - Custom headers, MIME types supported

### IoT Core
  - AWS service that helps you manage IoT devices & harvest data from them to e.g. Kinesis.

###  Amazon QuickSight
  - BI visualization of data from/into RedShift, S3, Athena, Aurora, RDS, IAM

###  AWS Amplify
  - build serverless mobile applications quickly with e.g. authentication, lambda, analytics, offline data sync.

###  AWS Elastic Transcoder
  - Convert media files (video + music) stored in S3 into various formats for tablets, PC, Smartphone, TV, etc
  - Features: bit rate optimization, thumbnail, watermarks, captions, DRM, progressive download, encryption
  - Pay based on the minutes that you transcode an the resolution at which you transcode

###  AWS WorkSpaces
  - Managed, Secure Cloud Desktop
  - Eliminates management of on-premise VDI (Virtual Desktop Infrastructure)
  - Integrated with Microsoft Active Directory

###  AWS Glue
AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics. You can create and run an ETL job with a few clicks in the AWS Management Console.

###  AppSync
  - Store and sync data across mobile and web apps in real time
  - Makes use of GraphQL
  - Differences from **Cognito Sync**
    - Cognito Sync cannot do Graph QL
    - Offline data synchronization
  - Integrations with DynamoDB / Lambda
  - Real-time subscriptions

###  AWS Certificate Manager
  -  Creating and managing public SSL/TLS certificates for your AWS based websites and applications
  - Allows wildcard certificate
    - Public key certificate which can be used with multiple sub-domains of a domain
    - Protect multiple sub domain names e.g. *.domainname.com
    - For multiple root domain names you need SNI through CloudFront or ELB.

###  AWS Resource Groups
  - Can be based on tags or CloudFormation stacks
  - Good for
    - Bulk options e.g. updates/security patches, upgrading applications, opening/closing ports
    - IAM permissions can be assigned to RG.

###  AWS Backup
  - Centralize manage backups for AWS and on premises (using Storage Gateway) services.

###  AI
  - Amazon Lex: Build Conversation Bots
  - Amazon Polly: Text to speech.

###  Amazon AppStream
  -  fully managed application streaming service

###  AWS Device Farm
  - is an app testing service that lets you test and interact with your Android, iOS, and web apps on many devices at once, or reproduce issues on a device in real time
