> #### [Elastic Load Balancing]()

- Elastic Load Balancing automatically distributes your incoming application traffic across multiple targets, such as EC2 instances. It monitors the health of registered targets and routes traffic only to the healthy targets. Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers.

Elastic Load Balancing supports three types of load balancers
- Application Load Balancers
- Network Load Balancers
- Classic Load Balancers

Availability Zones and Load Balancer Nodes
- When you enable an Availability Zone for your load balancer, Elastic Load Balancing creates a load balancer node in the Availability Zone
- If you register targets in an Availability Zone but do not enable the Availability Zone, these registered targets do not receive traffic
- recommend that you enable multiple Availability Zones
- After you disable an Availability Zone, the targets in that Availability Zone remain registered with the load balancer, but the load balancer will not route traffic to them.


Cross-Zone Load Balancing
- The nodes for your load balancer distribute requests from clients to registered targets. When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones
- When cross-zone load balancing is disabled, each load balancer node distributes traffic across the registered targets in its Availability Zone only.

Interface VPC Endpoints
- You can establish a private connection between your virtual private cloud (VPC) and the Elastic Load Balancing API by creating an interface VPC endpoint. You can use this connection to call the Elastic Load Balancing API from your VPC without sending traffic over the Internet

X-Forwarded Headers
- X-Forwarded-For
  - helps you identify the IP address of a client when you use an HTTP or HTTPS load balancer.
- X-Forwarded-Proto
  -  helps you identify the protocol (HTTP or HTTPS) that a client used to connect to your load balancer.
- X-Forwarded-Port
  - eader helps you identify the destination port that the client used to connect to the load balancer.

#### Application Load Balancer
 - functions at the application layer, the seventh layer of the Open Systems Interconnection (OSI) model

- Subnets for Your Load Balancer
  - When you create a load balancer, you must specify one public subnet from at least two Availability Zones. You can specify only one public subnet per Availability Zone.
  - To ensure that your load balancer can scale properly, verify that each subnet for your load balancer has a CIDR block with at least a /27 bitmask (for example, 10.0.0.0/27) and has at least 8 free IP addresses. Your load balancer uses these IP addresses to establish connections with the targets.
- Load Balancer Security Groups
  - A security group acts as a firewall that controls the traffic allowed to and from your load balancer.
  - The rules for the security groups associated with your load balancer security group must allow traffic in both directions on both the listener and the health check ports.
- Load Balancer State
  - provisioning
  - active
  - failed
- IP Address Type
  - ipv4
  - dualstack
- Deletion Protection
  - To prevent your load balancer from being deleted accidentally, you can enable deletion protection. By default, deletion protection is disabled for your load balancer.
- Connection Idle Timeout
  - For each request that a client makes through a load balancer, the load balancer maintains two connections
    - front-end connection - between a client and the load balancer
    - back-end connection - between the load balancer and a target
  - The load balancer manages an idle timeout that is triggered when no data is sent over a front-end connection for a specified time period. If no data has been sent or received by the time that the idle timeout period elapses, the load balancer closes the connection
  - default 60 secs
  - For back-end connections, we recommend that you enable the HTTP keep-alive option for your EC2 instances.
  -  If you enable HTTP keep-alive, the load balancer can reuse back-end connections until the keep-alive timeout expires.
  - We also recommend that you configure the idle timeout of your application to be larger than the idle timeout configured for the load balancer
- Listener Configuration
  - Listeners support the following protocols and ports:
    - Protocols: HTTP, HTTPS
    - Ports: 1-65535
  - Listener Rules
    - Each listener has a default rule, and you can optionally define additional rules.
    - Each rule has a priority. Rules are evaluated in priority order, from the lowest value to the highest value. The default rule is evaluated last.
    - Each rule action has a type, an order, and the information required to perform the action
    - Rule Action Types
      - authenticate-cognito
      - authenticate-oidc
      - fixed-response
      - forward
      - redirect
- Target Groups
  - Each target group is used to route requests to one or more registered targets
  - Target groups support the following protocols and ports:
    - Protocols: HTTP, HTTPS
    - Ports: 1-65535
  - Target Type
    - instance
    - ip
    - lambda
  - You can't specify publicly routable IP addresses.
- Slow Start Mode
  - By default, a target starts to receive its full share of requests as soon as it is registered with a target group and passes an initial health check. Using slow start mode gives targets time to warm up before the load balancer sends them a full share of requests. After you enable slow start for a target group, targets enter slow start mode when they are registered with the target group and exit slow start mode when the configured slow start duration period elapses. The load balancer linearly increases the number of requests that it can send to a target in slow start mode. After a target exits slow start mode, the load balancer can send it a full share of requests.
- Sticky Sessions
  - Sticky sessions are a mechanism to route requests to the same target in a target group.
  - This is useful for servers that maintain state information in order to provide a continuous experience to clients
  - To use sticky sessions, the clients must support cookies.
  - WebSockets connections are inherently sticky. If the client requests a connection upgrade to WebSockets, the target that returns an HTTP 101 status code to accept the connection upgrade is the target used in the WebSockets connection. After the WebSockets upgrade is complete, cookie-based stickiness is not used.
  - You enable sticky sessions at the target group level. You can also set the duration for the stickiness of the load balancer-generated cookie, in seconds.

Health Checks for Your Target Groups
- Application Load Balancer periodically sends requests to its registered targets to test their status. These tests are called health checks.
- Health checks do not support WebSockets.
- Target Health Status
  - initial
  - healthy
  - unhealthy
  - unused
  - draining

Lambda Functions as Targets
- can register your Lambda functions as targets and configure a listener rule to forward requests to the target group for your Lambda function.
- Limits
  -  Lambda function and target group must be in the same account.
  - maximum size of the request body that you can send to a Lambda function is 1 MB.
  - maximum size of the response JSON that the Lambda function can send is 1 MB.
  - WebSockets are not supported. Upgrade requests are rejected with an HTTP 400 code.

Monitor Your Application Load Balancers
- CloudWatch metrics
- Access logs
- Request tracing
- CloudTrail logs


#### Network Load Balancer
- functions at the fourth layer of the Open Systems Interconnection (OSI) model.
- Load Balancer State
  - provisioning
  - active
  - failed
- Load Balancer Attributes
  - deletion_protection.enabled ( default false )
  - load_balancing.cross_zone.enabled  ( default false )
- Availability Zones
  - you enable one or more Availability Zones for your load balancer when you create it. You cannot enable or disable Availability Zones for a Network Load Balancer after you create it. If you enable multiple Availability Zones for your load balancer, this increases the fault tolerance of your applications.
- Cross-Zone Load Balancing
  - same as ALB
- Deletion Protection
  - same as ALB
- Connection Idle Timeout
  -  If no data is sent through the connection by either the client or target for longer than the idle timeout, the connection is closed
  - idle timeout value is set too 350 seconds
  - You cannot modify this value.The targets of a TCP listener can use TCP keepalive packets to reset the idle timeout.
  - TCP keepalive packets are not supported for the targets of a TLS listener.
- Listener Configuration
  - Listeners support the following protocols and ports:  
    - Protocols: TCP, TLS
    - Ports: 1-65535
  - Listener Rules
    - same as ALB
- Target Groups for Your Network Load Balancers
  - target group is used to route requests to one or more registered targets
  - You define health check settings for your load balancer on a per target group basis
  - You can create different target groups for different types of requests
  - Routing Configuration
    - Target groups for Network Load Balancers support the following protocols and ports:
      - Protocols: TCP, TLS
      - Ports: 1-65535
    - Target Type
      - instance
      - ip
      - You can't specify publicly routable IP addresses.
- Proxy Protocol
  - Network Load Balancers use Proxy Protocol version 2 to send additional connection information such as the source and destination.
- Health Checks for Your Target Groups
  - same as ALB
- Monitor Your Network Load Balancers
  - same as ALB


#### Comparison chart

|Feature|Application Load Balancer|Network Load Balancer|Classic Load Balancer
|-------|-------------------------|---------------------|------------------
|Protocols|HTTP, HTTPS|TCP, TLS| TCP, SSL/TLS, HTTP, HTTPS|
|Platforms|VPC|VPC|EC2-Classic, VPC
|Health checks|Y|Y|Y
|CloudWatch metrics|Y|Y|Y
|Logging|Y|Y|Y
|Zonal fail-over|Y|Y|Y
|Connection draining (deregistration delay)|Y|Y|Y|
|Cross-zone load balancing|Y|Y|Y|
|SSL offloading|Y|Y|Y|
|Back-end server encryption|Y|Y|Y|
|Resource-based IAM permissions|Y|Y|Y|
|Load Balancing to multiple ports on the same instance|Y|Y| |
|WebSockets|Y|Y||
|IP addresses as targets|Y|Y||
|Lambda functions as targets|Y|Y||
|Load balancer deletion protection|Y|Y||
|Tag-based IAM permissions|Y|Y||
|Sticky sessions|Y||Y|
|Configurable idle connection timeout|Y||Y|
|Path-based routing|Y|||
|Host-based routing|Y|||
|HTTP header-based routing|Y|||
|HTTP method-based routing|Y|||
|Query string parameter-based routing|Y|||
|Source IP address CIDR-based routing|Y|||
|Native HTTP/2|Y|||
|Server Name Indication (SNI)|Y|||
|Slow start|Y|||
|User authentication|Y|||
|Redirects|Y|||
|Fixed response|Y|||
|Static IP||Y||
|Elastic IP address||Y||
|Preserve Source IP address||Y||
|Custom security policies|||Y|


##### Security features
- Perfect Forward Secrecy
  - is a feature that provides additional safeguards against the eavesdropping of encrypted data, through the use of a unique random session key. This prevents the decoding of captured data, even if the secret long-term key is compromised.
- Server Order Preference
  - lets you configure the load balancer to enforce cipher ordering, providing more control over the level of security used by clients to connect with your load balancer.
- Predefined Security Policy
  - simplifies the configuration of your load balancer by providing a recommended cipher suite that adheres to AWS security best practices. The policy includes the latest security protocols (TLS 1.1 and 1.2), enables Server Order Preference, and offers high security ciphers such as those used for Elliptic Curve signatures and key exchanges

##### Other important points
- you can privately access Elastic Load Balancing APIs from your Amazon Virtual Private Cloud (VPC) by creating VPC endpoints. With VPC endpoints, the routing between the VPC and Elastic Load Balancing APIs is handled by the AWS network without the need for an Internet gateway, NAT gateway, or VPN connection.

- We are pleased to announce support for multiple SSL certificates on Application Load Balancers using Server Name Indication (SNI). You can now host multiple secure (HTTPS) applications, each with its own SSL certificate, behind one load balancer. This greatly simplifies application management as many secure applications or multi-tenant SaaS applications can run behind the same load balancer.
Prior to this launch, Application Load Balancers supported only one certificate for a standard HTTPS listener (port 443) and you had to use Wildcard or Multi-Domain (SAN) certificates to host multiple secure applications behind the same load balancer. The potential security risks with Wildcard certificates and the operational overhead of managing Multi-Domain certificates presented challenges. With SNI support you can associate multiple certificates with a listener and each secure application behind a load balancer can use its own certificate.  
Application Load Balancers also support a smart certificate selection algorithm with SNI. If the hostname indicated by a client matches multiple certificates, the load balancer determines the best certificate to use based on multiple factors including the capabilities of the client.  
