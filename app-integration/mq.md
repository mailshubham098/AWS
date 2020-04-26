> ##### [Amazon MQ](https://docs.aws.amazon.com/amazon-mq/?id=docs_gateway)
Amazon MQ is a managed message broker service for Apache ActiveMQ that makes it easy to set up and operate message brokers in the cloud. Amazon MQ provides interoperability with your existing applications and services. Amazon MQ works with your existing applications and services without the need to manage, operate, or maintain your own messaging system.

- manages Apache ActiveMQ
- doesn't scale as much as SQS/SNS
- runs on a dedicated machine, can run in HA with failover
- has both queue feature (~SQS) and topic features (~SNS)

How Is Amazon MQ Different from Amazon SQS or Amazon SNS?
- Amazon MQ is a managed message broker service that provides compatibility with many popular message brokers. We recommend Amazon MQ for migrating applications from existing message brokers that rely on compatibility with APIs such as JMS or protocols such as AMQP, MQTT, OpenWire, and STOMP.
- Amazon SQS and Amazon SNS are queue and topic services that are highly scalable, simple to use, and don't require you to set up message brokers. We recommend these services for new applications that can benefit from nearly unlimited scalability and simple APIs.
