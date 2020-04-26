> #### [EC2 (Elastic Compute Cloud)](https://aws.amazon.com/ec2/):


Pricing for Amazon EC2

- **On-Demand Instances** : Pay for the instances that you use by the **second**, with no long-term commitments or upfront payments.Fixed rate by hour(or sec)
  - On-Demand Instance limits are managed in terms of the number of virtual central processing units (vCPUs) that your running On-Demand Instances are using, regardless of the instance type.
- **Reserved Instances** : Make a low, one-time, up-front payment for an instance, reserve it for a **one- or three-year term**, and pay a significantly lower hourly rate for these instances.
  - Types
    - Standard Reserved Instance
      - Some attributes, such as instance size, can be modified during the term; however, the instance family cannot be modified. You cannot exchange a Standard Reserved Instance, only modify it
      - Can be sold in the Reserved Instance Marketplace.
    - Convertible Reserved Instance
      - Can be exchanged during the term for another Convertible Reserved Instance with new attributes including instance family, instance type, platform, scope, or tenancy. For more information, see Exchanging Convertible Reserved Instances. You can also modify some attributes of a Convertible Reserved Instance
  - Instance Attributes that determine pricing
    - Instance type
    - Region
    - Tenancy (shared / dedicated)
    - Platform (OS type)
  - Scope of RI
    - Zonal
    - Regional

    - Cannot be sold in the Reserved Instance Marketplace.
- **Spot Instances** : Request unused EC2 instances, which can lower your costs significantly.
- **Dedicated hosts** : physical ec2 server dedicated for you.Can help you to reduce the cost by allowing you to use your existing server-bound software licenses
- **Dedicated Instances** : Pay, by the **hour**, for instances that run on **single-tenant hardware**.
- **Savings Plans** : Reduce your Amazon EC2 costs by making a commitment to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years.
- **Scheduled Instances** : Purchase instances that are always available on the specified **recurring schedule**, for a **one-year term**.
  - good choice for workloads that do not run continuously, but do run on a regular schedule.
- **Capacity Reservations** : Reserve capacity for your EC2 instances in a specific Availability Zone for any duration.

EC2 instance Types

|Family | Speciality |Use cases|
|-------|------------|---------|
|D2|Dense storage|Fileservers/Datawarehousing/Hadoop|
|R4|Memory optimised|Memory intensive apps/DBs|
|M4|General purpose|Application servers|
|C4|Compute optimised|CPU intensive apps/DBs|
|G2|Graphics intensive|Vedio encoding/3d application streaming|
|I2|High speed storage|NoSQL DBs,Datawarehousing etc|
|F1|Field programable gate array | Hardware acceleration for your code|
|T2|Lowest cost, General purpose|Web servers/ Small DBs|
|P2|Graphics/General purpose GPU|Machine learning, Bitcoin mining etc|
|X1|Memory Optimized|SAP HANA/Apache spark etc|


Storage for the Root Device
- All AMIs are categorized as either backed by Amazon EBS or backed by instance store. The former means that the root device for an instance launched from the AMI is an Amazon EBS volume created from an Amazon EBS snapshot. The latter means that the root device for an instance launched from	the AMI is an instance store volume created from a template stored in Amazon S3

Linux AMI Virtualization Types
- paravirtual (PV)
- hardware virtual machine (HVM) : can take advantage of special hardware extensions (CPU, network, and storage) for better performance.


Instance

 - root device for your instance contains the image used to boot the instance
- You can choose between AMIs backed by Amazon EC2 instance store and AMIs backed by Amazon EBS. We recommend that you use AMIs backed by Amazon EBS, because they launch faster and use persistent storage.

Root Device Storage Concepts

- Instance Store-backed Instances
  -  instance store volumes persists as long as the instance is running, but this data is deleted when the instance is terminated
  - instance store-backed instances do not support the Stop action
- Amazon EBS-backed Instances
  - EBS-backed instance can be stopped and later restarted without affecting data stored in the attached volumes


- A Linux instance has no password; you use a key pair to log in to your instance securely.
- If you plan to launch instances in multiple regions, you'll need to create a key-pair in each region.
- Security groups act as a firewall for associated instances, controlling both inbound and outbound traffic at the instance level
- You must add rules to a security group that enable you to connect to your instance from your IP address using SSH
- If you plan to launch instances in multiple regions, you'll need to create a security group in each region
- Security groups are specific to a region, so you should select the same region in which you created your key pair.

Instance Purchasing Options
- **On-Demand Instances** – Pay, by the second, for the instances that you launch.
- **Reserved Instances** – Purchase, at a significant discount, instances that are always available, for a term from one to three years.
- **Scheduled Instances** – Purchase instances that are always available on the specified recurring schedule, for a one-year term.
- **Spot Instances** – Request unused EC2 instances, which can lower your Amazon EC2 costs significantly.
- **Dedicated Hosts** – Pay for a physical host that is fully dedicated to running your instances, and bring your existing per-socket, per-core, or per-VM software licenses to reduce costs.
- **Dedicated Instances** – Pay, by the hour, for instances that run on single-tenant hardware.
- **Capacity Reservations** – Reserve capacity for your EC2 instances in a specific Availability Zone for any duration.

Network and Security

- Amazon EC2 Key Pairs
- Amazon EC2 Security Groups for Linux Instances
  - security group acts as a virtual firewall that controls the traffic for one or more instances
  - By default, security groups allow all outbound traffic.
  - Security group rules are always permissive; you can't create rules that deny access.
  - Security groups are stateful — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. For VPC security groups, this also means that responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.
  - You can add and remove rules at any time. Your changes are automatically applied to the instances associated with the security group.
  - When you associate multiple security groups with an instance, the rules from each security group are effectively aggregated to create one set of rules

- Controlling Access to Amazon EC2 Resources
- Amazon EC2 Instance IP Addressing
  - Private IPv4 Addresses
    - IP address that's not reachable over the Internet
    -  communication between instances in the same VPC
    - A private IPv4 address remains associated with the network interface when the instance is stopped and restarted, and is released when the instance is terminated.
  -  internal DNS hostname
    - Each instance is also given an internal DNS hostname that resolves to the primary private IPv4 address
  - Public IPv4 Addresses
    - reachable from the Internet
    - released when the instance is stopped or terminated
    - new one is assigned on restart of instance
    - released when you associate an Elastic IP with an instance and when you disassociate an EIP a new public IP is issued
    - If the public IP address of your instance in a VPC has been released, it will not receive a new one if there is more than one network interface attached to your instance
    - If your instance's public IP address is released while it has a secondary private IP address that is associated with an Elastic IP address, the instance does not receive a new public IP address.
    - If you require a persistent public IP address that can be associated to and from instances as you require, use an Elastic IP address instead.

  - external DNS hostname :
    - Each instance that receives a public IP address is also given an external DNS hostname
    - If you use dynamic DNS to map an existing DNS name to a new instance's public IP address, it might take up to 24 hours for the IP address to propagate through the Internet.To solve this problem, use an Elastic IP address.   
  - Multiple IP Addresses :
    - You can specify multiple private IPv4 and IPv6 addresses for your instances. The number of network interfaces and private IPv4 and IPv6 addresses that you can specify for an instance depends on the instance type
    - It can be useful to assign multiple IP addresses to an instance in your VPC to do the following:
      - Host multiple websites on a single server by using multiple SSL certificates on a single server and associating each certificate with a specific IP address.
      - Operate network appliances, such as firewalls or load balancers, that have multiple IP addresses for each network interface.
      - Redirect internal traffic to a standby instance in case your instance fails, by reassigning the secondary IP address to the standby instance.

- Bring Your Own IP Addresses (BYOIP)
  - You can bring part or all of your public IPv4 address range from your on-premises network to your AWS account. You continue to own the address range, but AWS advertises it on the internet. After you bring the address range to AWS, it appears in your account as an address pool. You can create an Elastic IP address from your address pool and use it with your AWS resources, such as EC2 instances, NAT gateways, and Network Load Balancers.

- Elastic IP Addresses
  - An Elastic IP address is a static IPv4 address designed for dynamic cloud computing. An Elastic IP address is associated with your AWS account. With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
  - An Elastic IP address is a public IPv4 address, which is reachable from the internet
  - When you associate an Elastic IP address with an instance or its primary network interface, the instance's public IPv4 address (if it had one) is released back into Amazon's pool of public IPv4 addresses.
  - You can disassociate an Elastic IP address from a resource, and reassociate it with a different resource
  - A disassociated Elastic IP address remains allocated to your account until you explicitly release it.
  - To ensure efficient use of Elastic IP addresses, we impose a small hourly charge if an Elastic IP address is not associated with a running instance, or if it is associated with a stopped instance or an unattached network interface. While your instance is running, you are not charged for one Elastic IP address associated with the instance, but you are charged for any additional Elastic IP addresses associated with the instance.
  - An Elastic IP address is for use in a specific region only.
  - When you associate an Elastic IP address with an instance that previously had a public IPv4 address, the public DNS hostname of the instance changes to match the Elastic IP address.
  - We resolve a public DNS hostname to the public IPv4 address or the Elastic IP address of the instance outside the network of the instance, and to the private IPv4 address of the instance from within the network of the instance.
  - When you allocate an Elastic IP address from an IP address pool that you have brought to your AWS account, it does not count toward your Elastic IP address limits.
- Elastic Network Interfaces
  - An elastic network interface is a logical networking component in a VPC that represents a virtual network card
  - A network interface can include the following attributes:
    - A primary private IPv4 address from the IPv4 address range of your VPC
    - One or more secondary private IPv4 addresses from the IPv4 address range of your VPC
    - One Elastic IP address (IPv4) per private IPv4 address
    - One public IPv4 address
    - One or more IPv6 addresses
    - One or more security groups
    - A MAC address
    - A source/destination check flag
    - A description
  - Every instance in a VPC has a default network interface, called the primary network interface (eth0)
  - You cannot detach a primary network interface from an instance.
  - You can attach a network interface to an EC2 instance in the following ways:
    - When it's running (hot attach)
    - When it's stopped (warm attach)
    - When the instance is being launched (cold attach).
- Enhanced Networking on Linux
  - Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces. Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking.
  - Enhanced Networking Types :
    - Elastic Network Adapter (ENA) - supports network speeds of up to 100 Gbps for supported instance types.
    - Intel 82599 Virtual Function (VF) interface - supports network speeds of up to 10 Gbps for supported instance types.

- Elastic Fabric Adapter
  - Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instances to accelerate High Performance Computing (HPC) applications. EFA enables you to achieve the application performance of an on-premises HPC cluster, with the scalability, flexibility, and elasticity provided by the AWS Cloud.
  - An EFA is an Elastic Network Adapter (ENA) with added capabilities.
  - It provides all of the functionality of an ENA, with an additional OS-bypass functionality. OS-bypass is an access model that allows HPC applications to communicate directly with the network interface hardware to provide low-latency, reliable transport functionality.
  - Elastic Network Adapters (ENAs) provide traditional IP networking features that are required to support VPC networking. EFAs provide all of the same traditional IP networking features as ENAs, and they also support OS-bypass capabilities. OS-bypass enables HPC applications to bypass the operating system kernel and to communicate directly with the EFA device.
  - You can attach only one EFA per instance.
  - EFA OS-bypass traffic is limited to a single subnet.
  - EFA OS-bypass traffic is not routable. Normal IP traffic from the EFA remains routable.
  - The EFA must be a member of a security group that allows all inbound and outbound traffic to and from the security group itself.

- Placement Groups
  - Cluster
    - logical grouping of instances within a single Availability Zone.
    - can span peered VPCs in the same Region
    - enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.

  - Partition
    - spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions, each partition within a placement group has its own set of racks
    - typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
    - help reduce the likelihood of correlated hardware failures for your application.
    - can have partitions in multiple Availability Zones in the same Region
    - maximum of seven partitions per Availability Zone.

  - Spread
    -  group of instances that are each placed on distinct racks, with each rack having its own network and power source.
    - strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.
    - can span multiple Availability Zones in the same Region.
    - can have a maximum of seven running instances per Availability Zone per group.

- Network Maximum Transmission Unit (MTU) for Your EC2 Instance
  - The maximum transmission unit (MTU) of a network connection is the size, in bytes, of the largest permissible packet that can be passed over the connection. The larger the MTU of a connection, the more data that can be passed in a single packet
  - Ethernet frames can come in different formats, and the most common format is the standard Ethernet v2 frame format. It supports 1500 MTU, which is the largest Ethernet packet size supported over most of the Internet. The maximum supported MTU for an instance depends on its instance type. All Amazon EC2 instance types support 1500 MTU, and many current instance sizes support 9001 MTU, or jumbo frames.

- Virtual Private Clouds
- EC2-Classic
  - With EC2-Classic, your instances run in a single, flat network that you share with other customers. With Amazon VPC, your instances run in a virtual private cloud (VPC) that's logically isolated to your AWS account.
  - If your account does not support EC2-Classic, we create a default VPC for you
  - If you created your AWS account after 2013-12-04, it does not support EC2-Classic, so you must launch your Amazon EC2 instances in a VPC.
  - multiple IP addresses are not supported.
  - An Elastic IP is disassociated from your instance when you stop it.
  - IPv6 addressing is not supported.

  - **ClassicLink** :
    - ClassicLink allows you to link EC2-Classic instances to a VPC in your account, within the same region. If you associate the VPC security groups with a EC2-Classic instance, this enables communication between your EC2-Classic instance and instances in your VPC using private IPv4 addresses.
    - ClassicLink removes the need to make use of public IPv4 addresses or Elastic IP addresses to enable communication between instances in these platforms.
  - Migrating from a Linux Instance in EC2-Classic to a Linux Instance in a VPC  

Storage

- Amazon Elastic Block Store
- Amazon EC2 Instance Store
- Amazon Elastic File System (Amazon EFS)
- Amazon Simple Storage Service (Amazon S3)


Instance Lifecycle

- Launch
- connect
- Stop and Start
- Hibernate
- Reboot
- Retire
- Terminate
  - Amazon EBS-backed instance supports the **InstanceInitiatedShutdownBehavior** attribute :  controls whether the instance stops or terminates when you initiate shutdown from within the instance itself (for example, by using the shutdown command on Linux)
  -  Amazon EBS volume supports the **DeleteOnTermination** attribute, which controls whether the volume is deleted or preserved when you terminate the instance it is attached to. The default is to delete the root device volume and preserve any other EBS volumes.
- Recover


Differences Between Reboot, Stop, Hibernate, and Terminate

| Characteristic     | Reboot    | Stop/start (Amazon EBS-backed instances only)    | Hibernate (Amazon EBS-backed instances only)     | Terminate     |
| :------------- | :------------- | :------------- | :------------- |:------------- |
|Host computer|The instance stays on the same host computer|In most cases, we move the instance to a new host computer. Your instance may stay on the same host computer if there are no problems with the host computer.|In most cases, we move the instance to a new host computer. Your instance may stay on the same host computer if there are no problems with the host computer.|None|
|Private and public IPv4 addresses|These addresses stay the same|The instance keeps its private IPv4 address. The instance gets a new public IPv4 address, unless it has an Elastic IP address, which doesn't change during a stop/start.|The instance keeps its private IPv4 address. The instance gets a new public IPv4 address, unless it has an Elastic IP address, which doesn't change during a stop/start.|None|
|Elastic IP addresses (IPv4)|The Elastic IP address remains associated with the instance|The Elastic IP address remains associated with the instance|The Elastic IP address remains associated with the instance|The Elastic IP address is disassociated from the instance|
|IPv6 address|The address stays the same|The instance keeps its IPv6 address|The instance keeps its IPv6 address|None|
|Instance store volumes|The data is preserved|The data is erased|The data is erased|The data is erased|
|Root device volume|The volume is preserved|The volume is preserved|The volume is preserved|The volume is deleted by default|
|RAM (contents of memory)|The RAM is erased|The RAM is erased|The RAM is saved to a file on the root volume|The RAM is erased|
|Billing|The instance billing hour doesn't change.|You stop incurring charges for an instance as soon as its state changes to stopping. Each time an instance transitions from stopped to running, we start a new instance billing period, billing a minimum of one minute every time you restart your instance.|You incur charges while the instance is in the stopping state, but stop incurring charges when the instance is in the stopped state. Each time an instance transitions from stopped to running, we start a new instance billing period, billing a minimum of one minute every time you restart your instance.|You stop incurring charges for an instance as soon as its state changes to shutting-down.|


Block Device Mapping
- A block device is a storage device that moves data in sequences of bytes or bits (blocks). These devices support random access and generally use buffered I/O. Examples include hard disks, CD-ROM drives, and flash drives. A block device can be physically attached to a computer or accessed remotely as if it were physically attached to the computer.
- Amazon EC2 supports two types of block devices:
  - Instance store volumes (virtual devices whose underlying hardware is physically attached to the host computer for the instance)
  - EBS volumes (remote storage devices)
- A block device mapping defines the block devices (instance store volumes and EBS volumes) to attach to an instance. You can specify a block device mapping as part of creating an AMI so that the mapping is used by all instances launched from the AMI. Alternatively, you can specify a block device mapping when you launch an instance, so this mapping overrides the one specified in the AMI from which you launched the instance. Note that all NVMe instance store volumes supported by an instance type are automatically enumerated and assigned a device name on instance launch; including them in your block device mapping has no effect.    

Launching an EC2 Fleet
- An EC2 Fleet contains the configuration information to launch a fleet—or group—of instances
- You can specify an unlimited number of instance types per EC2 Fleet
- EC2 Fleet is available only through the API or AWS CLI.
- An EC2 Fleet request can't span Regions. You need to create a separate EC2 Fleet for each Region.
- An EC2 Fleet request can't span different subnets from the same Availability Zone.


Amazon Elastic Inference
- Amazon Elastic Inference (EI) is a resource you can attach to your Amazon EC2 instances to accelerate your deep learning (DL) inference workloads.
- Amazon EI accelerators are available to all EC2 instance types
-  An Amazon EI accelerator is not part of the hardware that makes up your instance. Instead, the accelerator is attached through the network using an AWS PrivateLink endpoint service
- Before you launch an instance with an Amazon EI accelerator, you must create an AWS PrivateLink endpoint service. Each Availability Zone requires only a single endpoint service to connect instances with Amazon EI accelerators


Status Checks for Your Instances
  - System Status Checks : Monitor the AWS systems on which your instance runs.
  - examples of problems that can cause system status checks to fail:
    - Loss of network connectivity
    - Loss of system power
    - Software issues on the physical host
    - Hardware issues on the physical host that impact network reachability
  - Instance Status Checks: Monitor the software and network configuration of your individual instance.
  - examples of problems that can cause instance status checks to fail:
    - Failed system status checks
    - Incorrect networking or startup configuration
    - Exhausted memory
    - Corrupted file system
    - Incompatible kernel


Instance Metadata and User Data
- Instance metadata is data about your instance that you can use to configure or manage the running instance.
- Instance metadata contains lots of info such as

```
[ec2-user ~]$ curl http://169.254.169.254/latest/meta-data/    
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
events/
hostname
iam/
instance-action
instance-id
instance-type
local-hostname
local-ipv4
mac
metrics/
network/
placement/
profile
public-hostname
public-ipv4
public-keys/
reservation-id
security-groups
services/
```


Copying AMI
- use CopyImage action
- can copy both Amazon EBS-backed AMIs and instance-store-backed AMIs
- can copy AMIs with encrypted snapshots and also change encryption status during the copy process.
- Copying a source AMI results in an identical but distinct target AMI with its own unique identifier
- no charges for copying an AMI. However, standard storage and data transfer rates apply
- does not copy launch permissions, user-defined tags, or Amazon S3 bucket permissions from the source AMI to the new AMI
- Cross-Region Copying
  - Benifits
    - Consistent global deployment
    - Scalability
    - Performance
    - High availability
- Cross-Account Copying
  -  can share an AMI with another AWS account.
  - does not affect the ownership of the AMI.
  - owning account is charged for the storage in the Region
  - can't copy an AMI with an associated billingProduct code that was shared with you from another account
- Encryption and Copying
  - By default, the backing snapshot of an AMI is copied with its original encryption status

|Scenario|Description|Supported|
|--------|-----------|---------|
|1|Unencrypted-to-unencrypted|Yes|
|2|Encrypted-to-encrypted|Yes|
|3|Unencrypted-to-encrypted|Yes|
|4|Encrypted-to-unencrypted|No|



Other points

- By default, your instance is enabled for basic monitoring. You can optionally enable detailed monitoring. After you enable detailed monitoring, the Amazon EC2 console displays monitoring graphs with a 1-minute period for the instance. The following table describes basic and detailed monitoring for instances.
  - Basic - Data is available automatically in **5-minute**periods at no charge.
  - Detailed - Data is available in **1-minute** periods for an additional cost. To get this level of data, you must specifically enable it for the instance. For the instances where you've enabled detailed monitoring, you can also get aggregated data across groups of similar instances.
- only Convertible Reserved Instances can be exchanged for other Convertible Reserved Instances

#### [EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2)


- An Auto Scaling group can contain EC2 instances in one or more Availability Zones within the same Region.However, Auto Scaling groups cannot span multiple Regions.
- attempts to distribute instances evenly between the Availability Zones that are enabled for your Auto Scaling group
- The following actions can lead to rebalancing activity:
  - You change the Availability Zones for your group.
  - You explicitly terminate or detach instances and the group becomes unbalanced.
  - An Availability Zone that previously had insufficient capacity recovers and has additional capacity available.
  - An Availability Zone that previously had a Spot market price above your Spot bid price now has a market price below your bid price.

Default Termination policy (Scale -in )
- The default termination policy is designed to help ensure that your network architecture spans Availability Zones evenly. With the default termination policy, the instance termination priority by auto scaling is as follows
  - If there are uneven instances in multiple AZ -> select the AZ with most instances
  - then Select the instances with the oldest configuration
  - then select the instances closest to the next billing hour
  - then select random

Scaling Cooldowns
- The cooldown period helps to ensure that your Auto Scaling group doesn't launch or terminate additional instances before the previous scaling activity takes effect. You can configure the length of time based on your instance warmup period or other application needs.
- default time is 300 secs


Types of scaling policies
- Types
  - Target tracking scaling - Increase or decrease the current capacity of the group based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home – you select a temperature and the thermostat does the rest.
  - Step scaling - Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
  - Simple scaling - Increase or decrease the current capacity of the group based on a single scaling adjustment.
- If you are scaling based on a utilization metric that increases or decreases proportionally to the number of instances in an Auto Scaling group, then it is recommended that you use target tracking scaling policies. Otherwise, it is better to use step scaling policies instead.
