> #### [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
Amazon Virtual Private Cloud (Amazon VPC) enables you to launch Amazon Web Services (AWS) resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.

Concepts

  - VPCs
    - A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS Cloud.
    - You can specify an IP address range for the VPC, add subnets, associate security groups, and configure route tables.
    - Default VPC
      -  If your account supports the EC2-VPC platform only, it comes with a default VPC that has a default subnet in each Availability Zone
    - Nondefault VPC
      - You create your own vpc
    - CIDR not should overlap, and the max CIDR size in AWS is /16

  - Subnet
    - range of IP addresses in your VPC
    - public subnet
      - for resources that must be connected to the internet
    - private subnet
      - for resources that won't be connected to the internet
    - IPv4 CIDR block cannot be modified once created i.e. cannot increase or decrease the size of an existing CIDR block.
    - secondary CIDR blocks can be associated with the VPC to extend the VPC

  - Security
    - Security group
    - NACL

  - Accessing Internet
    - Internet gateway
    - NAT
    - Egress-Only Internet gateway

  - Accessing a Corporate or Home Network
    - IPsec AWS Site-to-Site VPN connection

  - Accessing Services Through AWS PrivateLink
    - AWS PrivateLink is a highly available, scalable technology that enables you to privately connect your VPC to supported AWS services, services hosted by other AWS accounts (VPC endpoint services), and supported AWS Marketplace partner services

Subnet Types
- Public subnet
  - If a subnet's traffic is routed to an internet gateway, the subnet is known as a public subnet.
- Private subnet
  - If a subnet doesn't have a route to the internet gateway, the subnet is known as a private subnet
- VPN-only subnet
  - If a subnet doesn't have a route to the internet gateway, but has its traffic routed to a virtual private gateway for a Site-to-Site VPN connection, the subnet is known as a VPN-only subnet.
- Each subnet maps to a single Availability Zone
- By default, all subnets can route between each other, whether they are private or public


Security group
- A security group acts as a virtual firewall for your instance to control inbound and outbound traffic
-  you can assign up to five security groups to the instance
- each instance in a subnet in your VPC could be assigned to a different set of security groups
- You have limits on the number of security groups that you can create per VPC, the number of rules that you can add to each security group, and the number of security groups you can associate with a network interface
- You can specify allow rules, but not d  eny rules.
- You can specify separate rules for inbound and outbound traffic.
- When you create a security group, it has no inbound rules. Therefore, no inbound traffic originating from another host to your instance is allowed until you add inbound rules to the security group.
- By default, a security group includes an outbound rule that allows all outbound traffic
- Security groups are stateful — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.
- Instances associated with a security group can't talk to each other unless you add rules allowing it (exception: the default security group has these rules by default)
- Security groups are associated with network interfaces. After you launch an instance, you can change the security groups associated with the instance, which changes the security groups associated with the primary network interface (eth0). You can also change the security groups associated with any other network interface
- When you specify a CIDR block as the source for a rule, traffic is allowed from the specified addresses for the specified protocol and port.
- When you specify a security group as the source for a rule, traffic is allowed from the elastic network interfaces (ENI) for the instances associated with the source security group for the specified protocol and port.
- Adding a security group as a source does not add rules from the source security group.
- After you launch an instance into a VPC, you can change the security groups that are associated with the instance. You can change the security groups for an instance when the instance is in the running or stopped state.
- You can delete a security group only if there are no instances assigned to it (either running or stopped).



Network ACL

- A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups in order to add an additional layer of security to your VPC.
- Your VPC automatically comes with a modifiable default network ACL. By default, it allows all inbound and outbound IPv4 traffic and, if applicable, IPv6 traffic.
- You can create a custom network ACL and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
- Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
- You can associate a network ACL with multiple subnets; however, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.
- A network ACL contains a numbered list of rules that we evaluate in order, starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL. The highest number that you can use for a rule is 32766. We recommend that you start by creating rules in increments (for example, increments of 10 or 100) so that you can insert new rules where you need to later on.
- A network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic
- Network ACLs are stateless; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).
- Each network ACL includes a default rule whose rule number is an asterisk. This rule ensures that if a packet doesn't match any of the other rules, it's denied. You can't modify or remove this rule.


Comparison of Security Groups and Network ACLs

| Security Group | Network ACL  |
| :------------- | :------------- |
| Operates at the instance level       | Operates at the subnet level   |
|Supports allow rules only|Supports allow rules and deny rules|
|Is stateful: Return traffic is automatically allowed, regardless of any rules|Is stateless: Return traffic must be explicitly allowed by rules|
|We evaluate all rules before deciding whether to allow traffic|We process rules in number order when deciding whether to allow traffic|
|Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on|Automatically applies to all instances in the subnets it's associated with (therefore, you don't have to rely on users to specify the security group)|

VPC Flow Logs
- VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC
- Flow log data can be published to Amazon CloudWatch Logs and Amazon S3. After you've created a flow log, you can retrieve and view its data in the chosen destination.
- You can create a flow log for a VPC, a subnet, or a network interface
- Flow log data for a monitored network interface is recorded as flow log records, which are log events consisting of fields that describe the traffic flow


Route table
- A route table contains a set of rules, called routes, that are used to determine where network traffic is directed.
- Each subnet in your VPC must be associated with a route table;the table controls the routing for the subnet
- A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same route table.
- Your VPC automatically comes with a main route table that you can modify.
- You can create additional custom route tables for your VPC.
- CIDR blocks for IPv4 and IPv6 are treated separately.
- When you add an Internet gateway, an egress-only Internet gateway, a virtual private gateway, a NAT device, a peering connection, or a VPC endpoint in your VPC, you must update the route table for any subnet that uses these gateways or connections.
- There is a limit on the number of route tables you can create per VPC, and the number of routes you can add per route table.
- You cannot delete the main route table, but you can replace the main route table with a custom table that you've created (so that this table is the default table each new subnet is associated with).
- Route Priority: We use the most specific route in your route table that matches the traffic to determine how to route the traffic (longest prefix match).
- when creating a new vpc , a security group , routing table and network ACL are provisioned automatically
- The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use, and cannot be assigned to an instance.For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:
   - 10.0.0.0: Network address.
   - 10.0.0.1: Reserved by AWS for the VPC router.
   - 10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see Amazon DNS Server.
   - 10.0.0.3: Reserved by AWS for future use.
   - 10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.
- You can have a multiple igw in a vpc
- A route table is associated with a specific vpc
- A route table specifies how packets are forwarded between the subnets within your VPC, the internet, and your VPN connection.
- Route Tables for an Internet Gateway : You can make a subnet a public subnet by adding a route to an Internet gateway.
- Route Tables for a NAT Device : To enable instances in a private subnet to connect to the Internet, you can create a NAT gateway or launch a NAT instance in a public subnet, and then add a route for the private subnet that routes IPv4 Internet traffic (0.0.0.0/0) to the NAT device.
- Route Tables for a Virtual Private Gateway : You can use an AWS Site-to-Site VPN connection to enable instances in your VPC to communicate with your own network
- Route Tables for a VPC Peering Connection : A VPC peering connection is a networking connection between two VPCs that allows you to route traffic between them using private IPv4 addresses. Instances in either VPC can communicate with each other as if they are part of the same network.
- Route Tables for ClassicLink: ClassicLink is a feature that enables you to link an EC2-Classic instance to a VPC, allowing communication between the EC2-Classic instance and instances in the VPC using private IPv4 addresses
- Route Tables for a VPC Endpoint ; A VPC endpoint enables you to create a private connection between your VPC and another AWS service.
- Route Tables for an Egress-Only Internet Gateway : You can create an egress-only Internet gateway for your VPC to enable instances in a private subnet to initiate outbound communication to the Internet, but prevent the Internet from initiating connections with the instances
- Routing Options
  - Route Tables for an Internet Gateway
  - Route Tables for a NAT Device
  - Route Tables for a Virtual Private Gateway
  - Route Tables for a VPC Peering Connection
  - Route Tables for ClassicLink
  - Route Tables for a VPC Endpoint
  - Route Tables for an Egress-Only Internet Gateway
  - Route Tables for Transit Gateways

Internet Gateways
- An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet
- An internet gateway serves two purposes:
  - to provide a target in your VPC route tables for internet-routable traffic
  - to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses.
- To use an internet gateway, your subnet's route table must contain a route that directs internet-bound traffic to the internet gateway.
- If your subnet is associated with a route table that has a route to an internet gateway, it's known as a **public subnet**.
- To enable communication over the internet for IPv4, your instance must have a public IPv4 address or an Elastic IP address that's associated with a private IPv4 address on your instance. Your instance is only aware of the private (internal) IP address space defined within the VPC and subnet. The internet gateway logically provides the one-to-one NAT on behalf of your instance, so that when traffic leaves your VPC subnet and goes to the internet, the reply address field is set to the public IPv4 address or Elastic IP address of your instance, and not its private IP address. Conversely, traffic that's destined for the public IPv4 address or Elastic IP address of your instance has its destination address translated into the instance's private IPv4 address before the traffic is delivered to the VPC  

Egress-Only Internet Gateways
- An egress-only Internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows outbound communication over IPv6 from instances in your VPC to the Internet, and prevents the Internet from initiating an IPv6 connection with your instances.
- An egress-only Internet gateway is for use with IPv6 traffic only. To enable outbound-only Internet communication over IPv4, use a NAT gateway instead.
- IPv6 addresses are globally unique, and are therefore public by default
- If you want your instance to be able to access the Internet, but you want to prevent resources on the Internet from initiating communication with your instance, you can use an egress-only Internet gateway.
- An egress-only Internet gateway is stateful: it forwards traffic from the instances in the subnet to the Internet or other AWS services, and then sends the response back to the instances.
- An egress-only Internet gateway has the following characteristics:  
  - You cannot associate a security group with an egress-only Internet gateway. You can use security groups for your instances in the private subnet to control the traffic to and from those instances.
  - You can use a network ACL to control the traffic to and from the subnet for which the egress-only Internet gateway routes traffic.

NAT
- NAT device to enable instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating connections with the instances
- NAT device forwards traffic from the instances in the private subnet to the internet or other AWS services, and then sends the response back to the instances.
- When traffic goes to the internet, the source IPv4 address is replaced with the NAT device’s address and similarly, when the response traffic goes to those instances, the NAT device translates the address back to those instances’ private IPv4 addresses.
- NAT devices are not supported for IPv6 traffic—use an egress-only Internet gateway instead.

  - NAT gateways
    -  must also specify an Elastic IP address to associate with the NAT gateway when you create it
    - The Elastic IP address cannot be changed once you associate it with the NAT Gateway
    - Preffered by the enterprise
    - Scale automatically upto 10Gpbs
    - A NAT gateway supports the following protocols: TCP, UDP, and ICMP.
    - No need to patch
    - Not associated with security groups
    - Remember to update the route tables and point them to  NAT gateway
    - No need to disable the source/destination check
    - More secure than NAT instance
    - A NAT gateway cannot be accessed by a ClassicLink connection associated with your VPC.
    - To avoid data processing charges for NAT gateways when accessing Amazon S3 and DynamoDB, set up a gateway endpoint and route the traffic through the gateway endpoint instead of the NAT gateway.

  - NAT instances
    - When creating a NAT instance ,Disable the source/destination check on the instance
    - NAT instances must be in a public subnet
    - There must be a route out of the private subnet to the NAT instance ,inorder for this to work
    - The amount of traffic that NAT instances can support depends on the instance size.If you are bottlenecking ,then increase the instance size
    - You can create high Availabilityusing autoscaling groups,
    multiple subnets in different AZs,and a script to automate failover
    - NAT instances are behind a security group

Comparison of NAT Instances and NAT Gateways

| Attribute | NAT gateway | NAT instance |
| --- | --- | --- |
| Availability | Highly available\. NAT gateways in each Availability Zone are implemented with redundancy\. Create a NAT gateway in each Availability Zone to ensure zone\-independent architecture\. | Use a script to manage failover between instances\. |
| Bandwidth | Can scale up to 45 Gbps\. | Depends on the bandwidth of the instance type\. |
| Maintenance | Managed by AWS\. You do not need to perform any maintenance\. | Managed by you, for example, by installing software updates or operating system patches on the instance\. |
| Performance | Software is optimized for handling NAT traffic\. | A generic Amazon Linux AMI that's configured to perform NAT\. |
| Cost | Charged depending on the number of NAT gateways you use, duration of usage, and amount of data that you send through the NAT gateways\. | Charged depending on the number of NAT instances that you use, duration of usage, and instance type and size\. |
| Type and size | Uniform offering; you don’t need to decide on the type or size\.  | Choose a suitable instance type and size, according to your predicted workload\. |
| Public IP addresses | Choose the Elastic IP address to associate with a NAT gateway at creation\. | Use an Elastic IP address or a public IP address with a NAT instance\. You can change the public IP address at any time by associating a new Elastic IP address with the instance\. |
| Private IP addresses | Automatically selected from the subnet's IP address range when you create the gateway\. | Assign a specific private IP address from the subnet's IP address range when you launch the instance\. |
| Security groups | Cannot be associated with a NAT gateway\. You can associate security groups with your resources behind the NAT gateway to control inbound and outbound traffic\. | Associate with your NAT instance and the resources behind your NAT instance to control inbound and outbound traffic\. |
| Network ACLs | Use a network ACL to control the traffic to and from the subnet in which your NAT gateway resides\. | Use a network ACL to control the traffic to and from the subnet in which your NAT instance resides\. |
| Flow logs | Use flow logs to capture the traffic\. | Use flow logs to capture the traffic\. |
| Port forwarding | Not supported\. | Manually customize the configuration to support port forwarding\. |
| Bastion servers | Not supported\. | Use as a bastion server\. |
| Traffic metrics | View [CloudWatch metrics for the NAT gateway](vpc-nat-gateway-cloudwatch.md)\. | View CloudWatch metrics for the instance\. |
| Timeout behavior | When a connection times out, a NAT gateway returns an RST packet to any resources behind the NAT gateway that attempt to continue the connection \(it does not send a FIN packet\)\. | When a connection times out, a NAT instance sends a FIN packet to resources behind the NAT instance to close the connection\. |
| IP fragmentation | Supports forwarding of IP fragmented packets for the UDP protocol\. Does not support fragmentation for the TCP and ICMP protocols\. Fragmented packets for these protocols will get dropped\.  | Supports reassembly of IP fragmented packets for the UDP, TCP, and ICMP protocols\. |

DNS Resolution in VPC
- enableDnsSupport (= DNS resolution setting)
  - Default = true
  - Helps decide if DNS resolution is supported for VPC
  - If true , queries the AWS DNS server at 169.254.169.253
- enableDnsHostname (= DNS hostname setting)
  - Default = false
  - Won't do anything unless enableDnsSupport=true
  - if true, assigns public hostname to EC2 instance if it has a public ip
- If you use custom DNS domain names in a private zone in Route53, you must set both these attributes to true


VPC Endpoints
- A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection
- Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.
- Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.
- There are two types of VPC endpoints:
  - Interface Endpoints (Powered by AWS PrivateLink)
    - An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service.
  - Gateway Endpoints
    - A gateway endpoint is a gateway that is a target for a specified route in your route table, used for traffic destined to a supported AWS service.
    - The following AWS services are supported:
      - Amazon S3
      - DynamoDB

VPN Connections

You can connect your Amazon VPC to remote networks and users using the following VPN connectivity options\.


| VPN connectivity option | Description |
| --- | --- |
| AWS Site\-to\-Site VPN | You can create an IPsec VPN connection between your VPC and your remote network\. On the AWS side of the Site\-to\-Site VPN connection, a virtual private gateway provides two VPN endpoints \(tunnels\) for automatic failover\. You configure your customer gateway on the remote side of the Site\-to\-Site VPN connection\. |
| AWS Client VPN | AWS Client VPN is a managed client\-based VPN service that enables you to securely access your AWS resources in your on\-premises network\. With AWS Client VPN, you configure an endpoint to which your users can connect to establish a secure TLS VPN session\. This enables clients to access resources in AWS or an on\-premises from any location using an OpenVPN\-based VPN client\.  |
| AWS VPN CloudHub | If you have more than one remote network \(for example, multiple branch offices\), you can create multiple AWS Site\-to\-Site VPN connections via your virtual private gateway to enable communication between these networks\. |
| Third party software VPN appliance | You can create a VPN connection to your remote network by using an Amazon EC2 instance in your VPC that's running a third party software VPN appliance\. AWS does not provide or maintain third party software VPN appliances; however, you can choose from a range of products provided by partners and open source communities\. |

- You can also use AWS Direct Connect to create a dedicated private connection from a remote network to your VPC


Bastion host
- Bastion hosts are instances that sit within your public subnet and are typically accessed using SSH or RDP. Once remote connectivity has been established with the bastion host, it then acts as a ‘jump’ server, allowing you to use SSH or RDP to log in to other instances (within private subnets) deeper within your VPC.
- When properly configured through the use of security groups and Network ACLs (NACLs), the bastion essentially acts as a bridge to your private instances via the internet.
- AWS suggests that you implement either Remote Desktop Gateway (for connecting to Windows instances) or SSH-agent forwarding (for Linux instances). Both of these solutions eliminate the need for storing private keys on the bastion host.
- The best way to implement a bastion host is to create a small EC2 instance which should only have a security group from a particular IP address for maximum security. We use a small instance rather than a large one because this host will only act as a jump server to connect to other instances in your VPC and nothing else. Hence, there is no point of allocating a large instance simply because it doesn't need that much computing power to process SSH (port 22) or RDP (port 3389) connections.

Other points
- A Route Origin Authorization (ROA) is a document that you can create through your Regional internet registry (RIR), such as the American Registry for Internet Numbers (ARIN) or Réseaux IP Européens Network Coordination Centre (RIPE). It contains the address range, the ASNs that are allowed to advertise the address range, and an expiration date.
- The ROA authorizes Amazon to advertise an address range under a specific AS number. However, it does not authorize your AWS account to bring the address range to AWS. To authorize your AWS account to bring an address range to AWS, you must publish a self-signed X509 certificate in the RDAP remarks for the address range. The certificate contains a public key, which AWS uses to verify the authorization-context signature that you provide. You should keep your private key secure and use it to sign the authorization-context message.
- VPC now allows customers to expand their VPCs by adding secondary IPv4 address ranges (CIDRs) to their VPCs. Customers can add the secondary CIDR blocks to the VPC directly from the console or by using the CLI after they have created the VPC with the primary CIDR block.

Deleting Your VPC
- you must terminate all instances in the VPC first
-  When you delete a VPC using the VPC console, we delete all its components, such as subnets, security groups, network ACLs, route tables, internet gateways, VPC peering connections, and DHCP options.
- If you have a AWS Site-to-Site VPN connection, you don't have to delete it or the other components related to the VPN (such as the customer gateway and virtual private gateway).
- It will detach VPN connection and its components such as virtual private gateway etc
- VPC won't be deleted if it has running NAT instance

#### VPC peering
- Overview
  - A networking connection between two VPCs that enables you to route traffic between them privately using private IPv4 addresses or IPv6 addresses. Instances in either VPC can communicate with each other as if they are within the same network.
  - You can create a VPC peering connection between your own VPCs, with a VPC in another AWS account, or with a VPC in a different AWS Region (also called Inter-Region VPC Peering).
  - A VPC peering connection is neither a gateway nor a AWS Site-to-Site VPN connection, and does not rely on a separate piece of physical hardware. There is no single point of failure for communication or a bandwidth bottleneck.
  - VPC Peering does NOT support edge-to-edge routing. You can create multiple VPC peering connections for each VPC that you own, but transitive peering relationships are not supported.
  - Establishing A Peering Connection
    - The owner of the requester VPC sends a request to the owner of the accepter VPC to create the VPC peering connection. The accepter VPC cannot have a CIDR block that overlaps with the requester VPC’s CIDR block.
    - To enable the flow of traffic between the VPCs using private IP addresses, the owner of each VPC in the VPC peering connection must manually add a route to one or more of their VPC route tables that points to the IP address range of the other VPC (the peer VPC).
    - Update the security group rules that are associated with your instance to ensure that traffic to and from the peer VPC is not restricted.
    - By default, if instances on either side of a VPC peering connection address each other using a public DNS hostname, the hostname resolves to the instance’s public IP address. To change this behavior, enable DNS hostname resolution for your VPC connection. This will allow the DNS hostname to resolve to the instance’s private IP address.
- Limitations
  - You cannot create a VPC peering connection between VPCs that have matching or overlapping IPv4 or IPv6 CIDR blocks.
  - You cannot have more than one VPC peering connection between the same two VPCs at the same time.
  - Unicast reverse path forwarding in VPC peering connections is not supported.
  - If the VPCs are in the same region, you can enable the resources on either side of a VPC peering connection to communicate with each other over IPv6.
  - For inter-region peering, you cannot create a security group rule that references a peer VPC security group. Communication over IPv6 is not supported as well.

- VPC Peering Scenarios
  - Peering Two or More VPCs to Provide Full Access to Resources
  - Peering to One VPC to Access Centralized Resources
  - Peering with ClassicLink

- Unsupported VPC Peering Configurations ([refer](https://docs.aws.amazon.com/vpc/latest/peering/invalid-peering-configurations.html#edge-to-edge-vgw))
  - Overlapping CIDR Blocks ( applies to both IPv4 & IPv6)
  - Transitive Peering
  - Edge to Edge Routing Through a Gateway or Private Connection
    - If either VPC in a peering relationship has one of the following connections, you cannot extend the peering relationship to that connection:
      - A VPN connection or an AWS Direct Connect connection to a corporate network
      - An internet connection through an internet gateway
      - An internet connection in a private subnet through a NAT device
      - A VPC endpoint to an AWS service; for example, an endpoint to Amazon S3.
      - (IPv6) A ClassicLink connection. You can enable IPv4 communication between a linked EC2-Classic instance and instances in a VPC on the other side of a VPC peering connection. However, IPv6 is not supported in EC2-Classic, so you cannot extend this connection for IPv6 communication.




- Pricing
  - If the VPCs in the VPC peering connection are within the same region, the charges for transferring data within the VPC peering connection are the same as the charges for transferring data across Availability Zones. If the VPCs are in different regions, inter-region data transfer costs apply.

- VPC scenarios diagram
  https://docs.google.com/document/d/1IOVCoerLwDVrNbdMZM6Uk7xUTZkxDV30ah0dZlKvHkk/

- Other imp points
  - S3 & DynamoDB are the only two AWS services that has Gateway endpoint , others have interface endpoints as a VPC endpoint
  - Currently, Amazon VPC supports five (5) IP address ranges, one (1) primary and four (4) secondary for IPv4. Each of these ranges can be between /28 (in CIDR notation) and /16 in size. The IP address ranges of your VPC should not overlap with the IP address ranges of your existing network.For IPv6, the VPC is a fixed size of /56 (in CIDR notation). A VPC can have both IPv4 and IPv6 CIDR blocks associated to it.
  - Monitoring VPN tunnels using Amazon CloudWatch  -> metric - TunnelState
