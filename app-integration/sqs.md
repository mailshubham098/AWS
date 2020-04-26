> #### [Amazon Simple Queue Service SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

- Amazon Simple Queue Service (Amazon SQS) is a fully managed message queuing service that makes it easy to decouple and scale microservices, distributed systems, and serverless applications. Amazon SQS moves data between distributed application components and helps you decouple these components.

How Is Amazon SQS Different from Amazon MQ or Amazon SNS?
- Amazon SQS and Amazon SNS are queue and topic services that are highly scalable, simple to use, and don't require you to set up message brokers. We recommend these services for new applications that can benefit from nearly unlimited scalability and simple APIs.
- Amazon MQ is a managed message broker service that provides compatibility with many popular message brokers. We recommend Amazon MQ for migrating applications from existing message brokers that rely on compatibility with APIs such as JMS or protocols such as AMQP, MQTT, OpenWire, and STOMP.

Types of Queue
- Standard Queue
  - Unlimited Throughput
  - At-Least-Once Delivery
  - Best-Effort Ordering
  - max 256K messages
- FIFO Queue
  - High Throughput but less than standard ,Up to 300 messages per seconds (for receive, delete & send)
  - Exactly-Once Processing
  - First-In-First-Out Delivery
  - support up to 3,000 messages per second with batching
  - support up to 300 messages per second, per action (SendMessage, ReceiveMessage, or DeleteMessage) without batching.
  - Name of the queue must end in .fifo
  - No per message delay (only per queue delay)
  - ability to do content based de-duplication
  - 5 min interval de-duplication using "Deduplication ID"
  - Message groups
    - possibility to group msgs for FIFO ordering using  "message groupID"
    - one one worker can be assigned per msg group
    - msg group is just an extra tag in the msg

- Queue attributes ( mostly same for both )

|Attribute| Range of values | default |
|---------|-----------------|---------|
|Visibility Timeout | 0 secs - 12 hrs| 30 secs|
|Msg Retention period| 1 min - 14 day| 4 days|
|Max Msg size| 1 - 256KB| 256KB|
|Delivery delay| 0 secs - 15 mins| 0 secs|
|Receive msg wait time | 0 secs - 20 secs| 0 secs|

Message timers
- Message timers let you specify an initial invisibility period for a message added to a queue. For example, if you send a message with a 45-second timer, the message isn't visible to consumers for its first 45 seconds in the queue. The default (minimum) invisibility period for a message is 0 seconds. The maximum is 15 minutes.

Purge queue
  - If you don't want to delete an Amazon SQS queue but need to delete all the messages from it, you can purge the queue
  - When you purge a queue, you can't retrieve any messages deleted from it.
  - The message deletion process takes up to 60 seconds. We recommend waiting for 60 seconds regardless of your queue's size.
  - By default, a queue retains a message for four days after it is sent. You can configure a queue to retain messages for up to 14 days
  - If you don't use the queue then you can delete the queue instead of purging


Server-Side Encryption (SSE) for an Existing Amazon SQS Queue
  - All requests to queues with SSE enabled must use HTTPS and Signature Version 4.
  - When you disable SSE, messages remain encrypted.

Long Polling for an Amazon SQS Queue
  - Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses and false empty responses and decreases the number of API calls
  - preferred over short polling
  - Set Receive Message Wait Time, type a number between 1 and 20 secs
  - Setting the value to 0 configures short polling
  - By default, Amazon SQS uses short polling, querying only a subset of its servers (based on a weighted random distribution), to determine whether any messages are available for a response.
  - enabled at the queue level or at API level using WaitTimeSeconds

Amazon SQS Dead-Letter Queue
  - A dead-letter queue is a queue that other (source) queues can target for messages that can't be processed (consumed) successfully
  - We can set a threshold of how many times a msg can go back to the SQS queue (also called redrive policy) after which it will be send to the designated Deal letter queue
  - When you designate a queue to be a source queue, a dead-letter queue is not created automatically. You must first create a normal standard or FIFO queue before designating it a dead-letter queue
  - The dead-letter queue of a FIFO queue must also be a FIFO queue.
  - Similarly, the dead-letter queue of a standard queue must also be a standard queue.
  - Make sure to process the messages in the DLQ before they expire

Visibility Timeout for an Amazon SQS Queue
  - Immediately after a message is received, it remains in the queue. To prevent other consumers from processing the message again, Amazon SQS sets a visibility timeout, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message.
  - default visibility timeout for a message is 30 seconds
  - min - 0 sec , max - 12 hrs
  - Always remember that the messages in the SQS queue will continue to exist even after the EC2 instance has processed it, until you delete that message. You have to ensure that you delete the message after processing to prevent the message from being received and processed again once the visibility timeout expires.
  - ChangeMessageVisibility - API to change the visibility while processing a message
  - DeleteMessage - API to tell SQL that the message was successfully processed and should be deleted
  - Set between 0 seconds and 12 hours (default: 30 seconds)
  - If too low (e.g. 30 seconds): Message can be processed more than once before one consumer is done.

Amazon SQS Delay Queue
  - Delay queues let you postpone the delivery of new messages to a queue for a number of seconds
  - If you create a delay queue, any messages that you send to the queue remain invisible to consumers for the duration of the delay period.
  - default - 0sec, max -15 min
  - For standard queues, the per-queue delay setting is not retroactive—changing the setting doesn't affect the delay of messages already in the queue.
  - For FIFO queues, the per-queue delay setting is retroactive—changing the setting affects the delay of messages already in the queue.
  - At queue level: Can set a default (default is 0 seconds -> visible right away)
  - At message level: Can override the default using the DelaySeconds parameter.

Retention time
  -  The default message retention period is 4 days.
  - you can set the message retention period to a value from 60 seconds to 14 days

Other points

- Can subscribe Amazon SQS Queue to an Amazon SNS Topic
- Amazon SNS isn't currently compatible with FIFO queues.
- Can configure Messages Arriving in an Amazon SQS Queue to Trigger an AWS Lambda Function
- A single Amazon SQS message queue can contain an unlimited number of messages. However, there is a 120,000 limit for the number of inflight messages for a standard queue and 20,000 for a FIFO queue. Messages are inflight after they have been received from the queue by a consuming component, but have not yet been deleted from the queue.

Amazon SQS Security
- Authentication and Access Control for Amazon SQS
- Protecting Amazon SQS Data Using Server-Side Encryption (SSE) and AWS KMS
- Amazon Virtual Private Cloud Endpoints for Amazon SQS
