> #### [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

- Amazon CloudWatch provides a reliable, scalable, and flexible monitoring solution that you can start using within minutes. You no longer need to set up, manage, and scale your own monitoring systems and infrastructure.

Amazon CloudWatch Concepts
- Namespaces
  - is a container for CloudWatch metrics
  - isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics.
- Metrics
- Time Stamps
- Metrics Retention
  CloudWatch retains metric data as for data points follows:
  - period < 60 secs data points -> available for 3 hours
  - period of 1 min -> 15 days
  - period of 5 min -> 63 days (default)
  - period of 1 hr -> 15 months
- Dimensions - key-value to identify the metric
- Statistics - min/max/avg etc
- Aggregation
- Alarm

CloudWatch Dashboards

- All dashboards are global
- Dashboards can include graphs from different regions
- you can set timezone and time range of the dashboards
- you can setup automatic refresh ( 10s, 1m, 2m, 15m)
- Pricing
  - 3 dashboards(upto 500 metrics) for free
  - $3/dashboards/month afterwards


Metrics
- CloudWatch has available Amazon EC2 Metrics for you to use for monitoring
  - CPU Utilization
  - Network Utilization
  -  Disk i/O
- You need to prepare a custom metric using CloudWatch Monitoring Scripts which is written in Perl. You can also install CloudWatch Agent to collect more system-level metrics from Amazon EC2 instances. Here's the list of custom metrics that you can set up:
  - Memory utilization
  - Disk swap utilization
  - Disk space utilization
  - Page file utilization
  - Log collection


CloudWatch Logs
- Applications can send logs to CloudWatch using SDK
- CloudWatch logs can be go to
  - Batch export to S2 for archival
  - Stream to ElasticSearch cluster for further analysis
- Log architecture
  - Log groups: arbitary name, usually representing an application
  - Log stream: instances with app/log files/containers
- Can define log expiration policies
- Using CLI we can tail CloudWatch logs
- To send logs to CloudWatch, make sure IAM permissions are correct
- can use filter expressions
  - eg. specific Ip inside log
  - Metric filters can be used to trigger alarms
- CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards

CloudWatch Alarms
- used to trigger notifications for any metric
- can go to Auto Scaling , EC2 Actions , SNS notifications
- various options ( sampling , % max, avg etc)
- Alarm states
  - Ok
  - INSUFFICIENT_DATA
  - ALARM
- period
  - length of time isn secs to evaluate the metric
  - for high resolution custom metrics : can only choose 10 secs or 30 secs

CloudWatch Events
- Schedule: Cron jobs
- Event pattern: Event rules to react to a service doing something eg. CodePipeline state changes
- Triggers to Lambda functions : SQS/SNS/Kinesis messages
- CloudWatch events creates a small json document to give info about the change

Other points
- Enable detailed monitoring for less than 5 mins ( standard monitoring )
- The number of instances in an ASG cannot go below the minimum configured, even if the alarm would in theory trigger any instance termination
