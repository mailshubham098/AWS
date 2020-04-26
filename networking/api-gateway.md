> #### [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
Amazon API Gateway enables you to create and deploy your own REST and WebSocket APIs at any scale. You can create robust, secure, and scalable APIs that access AWS or other web services, as well as data that’s stored in the AWS Cloud. You can create APIs to use in your own client applications, or you can make your APIs available to third-party app developers.

Features
- is an AWS service for creating, publishing, maintaining, monitoring, and securing REST and WebSocket APIs at any scale
- developers can create APIs that access AWS or other web services as well as data stored in the AWS Cloud
- API Gateway creates REST APIs that:
  - Are HTTP-based.
  - Adhere to the REST protocol, which enables stateless client-server communication.
  - Implement standard HTTP methods such as GET, POST, PUT, PATCH and DELETE.
  - Support for generating SDKs and creating API documentation using API Gateway extensions to OpenAPI
  - Throttling of HTTP requests
- API Gateway creates WebSocket APIs that:
  - Adhere to the WebSocket protocol, which enables stateful, full-duplex communication between client and server.
  - Route incoming messages and based on message content.
  - Monitoring and throttling of connections and messages
  - Using AWS X-Ray to trace messages as they travel through the APIs to backend services
  - Easy integration with HTTP/HTTPS endpoints
- Support for stateful (WebSocket) and stateless (REST) APIs.
- Multiple options for controlling access to your REST and WebSocket APIs.
- Developer portal for publishing your APIs.
- Canary release deployments for safely rolling out changes.
- CloudTrail logging and monitoring of API usage and API changes.
- CloudWatch access logging and execution logging, including the ability to set alarms.
- Ability to use AWS CloudFormation templates to enable API creation.
- Support for custom domain names.
- Integration with AWS WAF for protecting your APIs against common web exploits.
- Integration with AWS X-Ray for understanding and triaging performance latencies.
- Together with AWS Lambda, API Gateway forms the app-facing part of the AWS serverless infrastructure.
- To enable serverless applications, API Gateway supports streamlined proxy integrations with AWS Lambda and HTTP endpoints.

Concepts
- API deployment – a point-in-time snapshot of your API Gateway API resources and methods. To be available for clients to use, the deployment must be associated with one or more API stages.
- API endpoints – host names APIs in API Gateway, which are deployed to a specific region and of the format: rest-api-id.execute-api.region.amazonaws.com
- API key – An alphanumeric string that API Gateway uses to identify an app developer who uses your API.
- API stage – A logical reference to a lifecycle state of your API. API stages are identified by API ID and stage name.
- Model – Data schema specifying the data structure of a request or response payload.
- Private API – An API that is exposed through interface VPC endpoints and isolated from the public internet
- Private integration – An API Gateway integration type for a client to access resources inside a customer’s VPC through a private API endpoint without exposing the resources to the public internet.
- Proxy integration – You can set up a proxy integration as an HTTP proxy integration type or a Lambda proxy integration type.
  - For the HTTP proxy integration, API Gateway passes the entire request and response between the frontend and an HTTP backend.
  - For the Lambda proxy integration, API Gateway sends the entire request as an input to a backend Lambda function.
- Usage plan – Provides selected API clients with access to one or more deployed APIs. You can use a usage plan to configure throttling and quota limits, which are enforced on individual client API keys.

API Endpoint Types
- Edge-optimized API endpoint
  - The default host name of an API Gateway API that is deployed to the specified region while using a CloudFront distribution to facilitate client access typically from across AWS regions. API requests are routed to the nearest CloudFront Point of Presence.
- Regional API endpoint
  - The host name of an API that is deployed to the specified region and intended to serve clients, such as EC2 instances, in the same AWS region. API requests are targeted directly to the region-specific API Gateway without going through any CloudFront distribution.
  - You can apply latency-based routing on regional endpoints to deploy an API to multiple regions using the same regional API endpoint configuration, set the same custom domain name for each deployed API, and configure latency-based DNS records in Route 53 to route client requests to the region that has the lowest latency.
- Private API endpoint
  - Allows a client to securely access private API resources inside a VPC. Private APIs are isolated from the public Internet, and they can only be accessed using VPC endpoints for API Gateway that have been granted access.


Publishing Your API Gateway APIs
- Use the Serverless Developer Portal to Catalog Your API Gateway APIs
- Sell Your API Gateway APIs through AWS Marketplace
- Set up Custom Domain Name for an API in API Gateway
- Export a REST API from API Gateway
- Set up an API Gateway Canary Release Deployment

Monitoring
- use **AWS Config** to record configuration changes made to your API Gateway API resources and send notifications based on resource changes
- AWS Config can track changes to:
  - API stage configuration, such as:
    - cache cluster settings
    - throttle settings
    - access log settings
    - the active deployment set on the stage
  - API configuration, such as:
    - endpoint configuration
    - version
    - protocol
    - tags
        
Pricing
- You pay only for the API calls you receive and the amount of data transferred out.
- API Gateway also provides optional data caching charged at an hourly rate that varies based on the cache size you select.
- Usage plan-throttled requests are not charged when rate limits or quota exceed the preconfigured limits.


Compliance
- PCI DSS (Payment Card Industry , Data Security Standard
- HIPAA (Health Insurance Portability and Accountability Act of 1996 )




Limits

- API Gateway does not support wildcard subdomain names (of the *.domain form). However, it does support wildcard certificates (certificates for wildcard subdomain names).
- API Gateway does not support sharing a custom domain name across REST and WebSocket APIs.
- The /ping and /sping paths are reserved for the service health check


| Resource or Operation | Default Limit | Can Be Increased |
| --- | --- | --- |
| Throttle limit per region across REST APIs, WebSocket APIs, and WebSocket callback APIs | 10,000 requests per second \(RPS\) with an additional burst capacity provided by the [token bucket algorithm](https://en.wikipedia.org/wiki/Token_bucket), using a maximum bucket capacity of 5,000 requests\.  The burst limit is determined by the API Gateway service team based on the overall RPS limits for the account\. It is not a limit that a customer can control or request changes to\.  | Yes |
| Regional APIs | 600 | No |
| Edge\-Optimized APIs | 120 | No |

The following limits apply to configuring and running a WebSocket API in Amazon API Gateway\.


| Resource or Operation | Default Limit | Can Be Increased |
| --- | --- | --- |
| New connections per second per account \(across all WebSocket APIs\) per region | 500 | No |
| AWS Lambda authorizers per API | 10 | Yes |
| Routes per API | 300 | Yes |
| Integrations per API | 300 | Yes |
| Stages per API | 10 | Yes |
| WebSocket frame size | 32 KB | No |
| Message payload size | 128 KB  Because of the WebSocket frame\-size limit of 32 KB, a message larger than 32 KB must be split into multiple frames, each 32 KB or smaller\. If a larger message \(or larger frame size\) is received, the connection is closed with code 1009\.  | No |
| Connection duration for WebSocket API | 2 hours | No |
| Idle Connection Timeout | 10 minutes | No |

The following limits apply to configuring and running a REST API in Amazon API Gateway\.


| Resource or Operation | Default Limit | Can Be Increased |
| --- | --- | --- |
| Private APIs per account per region | 600 | No |
| Length, in characters, of API Gateway resource policy | 8092 | Yes |
| API keys per account per region | 500 | Yes |
| Client certificates per account per region | 60 | Yes |
| Authorizers per API \(AWS Lambda and Amazon Cognito\) | 10 | Yes |
| Documentation parts per API | 2000 | Yes |
| Resources per API | 300 | Yes |
| Stages per API | 10 | Yes |
| Usage plans per account per region | 300 | Yes |
| Usage plans per API key | 10 | Yes |
| Per\-method throttling limit settings per API stage | 20 | Yes |
| VPC links per account per region | 5 | Yes |
| API caching TTL | 300 seconds by default and configurable between 0 and 3600 by an API owner\. | Not for the upper bound \(3600\) |
| Cached response size | 1048576 Bytes\. Cache data encryption may increase the size of the item that is being cached\. | No |
| Integration timeout | 50 milliseconds \- 29 seconds for all integration types, including Lambda, Lambda proxy, HTTP, HTTP proxy, and AWS integrations\. | Not for the lower or upper bounds\. |
| Header value size | 10240 Bytes | No |
| Payload size | 10 MB | No |
| Tags per stage | 50 | No |
| Number of iterations in a \#foreach \.\.\. \#end loop in mapping templates | 1000 | No |
| ARN length of a method with authorization | 1600 bytes | No |
