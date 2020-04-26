> #### [Amazon Simple Notification Service  SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)

- Amazon Simple Notification Service (Amazon SNS) is a web service that enables applications, end-users, and devices to instantly send and receive notifications from the cloud.
- Can be used for System-to-System Messaging
- Pub - sub model
- upto 10,000,0000 subscriptions per topic
- upto 10,000 topics limit
- Amazon SNS provides in-transit encryption by default. Enabling server-side encryption adds at-rest encryption to your topic.

- Topic
  - placeholder where events are published to or subscribed from
  - logical access point and communication channel
  - supports SSE(server-side encryption) when enabled
  - Can enab le Server-Side Encryption (SSE) for an Amazon SNS Topic with an Encrypted Amazon SQS Queue Subscribed
- Publisher
  - Publishers communicate asynchronously with subscribers by producing and sending a message to a topic, which is a logical access point and communication channel.
- Subscriber
  - Subscribers (i.e., web servers, email addresses, Amazon SQS queues, AWS Lambda functions) consume or receive the message or notification over one of the supported protocols (i.e., Amazon SQS, HTTP/S, email, SMS, Lambda) when they are subscribed to the topic.
    - Lambda
    - email , email-json
    - HTTP/S
    - SQS
    - SMS
    - platform application endpoint (notification to mobile)
- To accelerate the development of your event-driven applications, you can subscribe event-handling pipelines—powered by AWS Event Fork Pipelines—to Amazon SNS topics. AWS Event Fork Pipelines is a suite of open-source nested applications, based on the AWS Serverless Application Model (AWS SAM), which you can deploy directly from the AWS Event Fork Pipelines suite

Common Amazon SNS Scenarios
- Fanout
  - The "fanout" scenario is when an Amazon SNS message is sent to a topic and then replicated and pushed to multiple Amazon SQS queues, HTTP endpoints, or email addresses.
  - This allows for parallel asynchronous processing
- Application and System Alerts
- Push Email and Text Messaging
- Mobile Push Notifications
- Message Durability
  - stores the message in multiple isolated AZ

Amazon SNS Message Delivery Status
- Amazon SNS provides support to log the delivery status of notification messages sent to topics with the following Amazon SNS endpoints:
  - Application
  - HTTP
  - Lambda
  - SQS
- After you configure the message delivery status attributes, log entries will be sent to CloudWatch Logs for messages sent to a topic subscribed to an Amazon SNS endpoint. Logging message delivery status helps provide better operational insight, such as the following:
  - Knowing whether a message was delivered to the Amazon SNS endpoint.
  - Identifying the response sent from the Amazon SNS endpoint to Amazon SNS.
  - Determining the message dwell time (the time between the publish timestamp and just before handing off to an Amazon SNS endpoint).

Amazon SNS Message Attributes
- Message attributes allow you to provide structured metadata items (such as time stamps, geospatial data, signatures, and identifiers) about the message.
-  Message attributes are optional and separate from, but sent along with, the message body.
- This information can be used by the receiver of the message to help decide how to handle the message without having to first process the message body
- To use message attributes with Amazon SQS endpoints, you must set the subscription attribute, Raw Message Delivery, to True.
- Message attributes are sent only when the message structure is String, not JSON.

Message Attribute Data Types
  - String
  - String.Array
  - Number
  - Binary

Amazon SNS Message Filtering
- A filter policy is a simple JSON object containing attributes that define which messages the subscriber receives.

Monitoring and Logging Amazon SNS Topics
- Monitor Amazon SNS Topics Using CloudWatch
- Log Amazon Simple Notification Service API Calls Using AWS CloudTrail

Imp points
- Amazon Simple Queue Service (SQS) and Amazon SNS are both messaging services within AWS, which provide different benefits for developers. Amazon SNS allows applications to send time-critical messages to multiple subscribers through a “push” mechanism, eliminating the need to periodically check or “poll” for updates. Amazon SQS is a message queue service used by distributed applications to exchange messages through a polling model, and can be used to decouple sending and receiving components. Amazon SQS provides flexibility for distributed components of applications to send and receive messages without requiring each component to be concurrently available.
- SNS + SQS => FAN OUT
