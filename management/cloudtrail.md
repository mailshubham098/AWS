> #### [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

- WS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account
- Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail.
- CloudTrail is enabled on your AWS account when you create it. That is it is enabled by default
- Get an history of events/API calls made within your AWS account for
  - console
  - SDK
  - CLI
  - AWS services
- Can put logs from CloudTrail into CloudWatch Logs
- If a resource is deleted in AWS ,look into CloudTrail first
- You can easily view events in the CloudTrail console by going to Event history.
- Event history allows you to view, search, and download the past 90 days of activity in your AWS account.
- you can create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources
- A trail is a configuration that enables delivery of events to an Amazon S3 bucket that you specify.
- You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon CloudWatch Events
- By default, CloudTrail event log files are encrypted using Amazon S3 server-side encryption (SSE). You can also choose to encrypt your log files with an AWS Key Management Service (AWS KMS) key.
- You can store your log files in your bucket for as long as you want. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications.
- CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, API calls, and other AWS services. This event history simplifies security analysis, resource change tracking, and troubleshooting.


Types of Trail
- A trail that applies to all regions
  - CloudTrail records events in each region and delivers the CloudTrail event log files to an S3 bucket that you specify. If a region is added after you create a trail that applies to all regions, that new region is automatically included, and events in that region are logged.
  - this is the default
- A trail that applies to one region
  - When you create a trail that applies to one region, CloudTrail records the events in that region only.
  - It then delivers the CloudTrail event log files to an Amazon S3 bucket that you specify
- For both types of trails, you can specify an Amazon S3 bucket from any region.
- Organization trail
  - If you have created an organization in AWS Organizations, you can also create a trail that will log all events for all AWS accounts in that organization
  - Organization trails must be created in the master account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization.
  - By default, member accounts will not have access to the log files for the organization trail in the Amazon S3 bucket

How Does CloudTrail Relate to Other AWS Monitoring Services?
- CloudTrail adds another dimension to the monitoring capabilities already offered by AWS
- Amazon CloudWatch focuses on performance monitoring and system health. CloudTrail focuses on API activity.


Limits

| Resource | Default Limit | Comments |
| --- | --- | --- |
| Trails per region | 5 | This limit cannot be increased\. |
| Get, describe, and list APIs | 10 transactions per second \(TPS\) | The maximum number of operation requests you can make per second without being throttled\. The LookupEvents API is not included in this category\.This limit cannot be increased\. |
| All other APIs | 1 transaction per second \(TPS\) | The maximum number of operation requests you can make per second without being throttled\. This limit cannot be increased\. |
| Event selectors | 5 per trail | This limit cannot be increased\. |
| Data resources in event selectors | 250 across all event selectors in a trail | The total number of data resources cannot exceed 250 across all event selectors in a trail\. The limit of number of resources on an individual event selector is configurable up to 250\. This upper limit is allowed only if the total number of data resources does not exceed 250 across all event selectors\. Examples: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/WhatIsCloudTrail-Limits.html)This limit cannot be increased\. |
