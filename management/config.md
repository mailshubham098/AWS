AWS Config

- AWS Config is a fully managed service that provides AWS resource inventory, configuration history, and configuration change notifications to enable security and governance
- It provides a detailed view of the configuration of AWS resources in the AWS account.
- It gives point-in-time and historical states and allows user to see changes visually in a timeline
- In cases where several configuration changes are made to a resource in quick succession (i.e., within a span of few minutes), AWS Config will only record the latest configuration of that resource; this represents the cumulative impact of that entire set of changes
- AWS Config does not cover all the AWS services and for the services unsupported the configuration management process can be automated using API and code and used to compare current and past data


AWS Config Use Case
- Security Analysis & Resource Administration
- Auditing & Compliance
- Change Management
- Troubleshooting
- Discovery


AWS Config Concepts


AWS Config Flow
- When AWS Config is turned on, it first discovers the supported AWS resources that exist in the account and generates a configuration item for each resource.
- AWS Config also generates configuration items when the configuration of a resource changes, and it maintains historical records of the configuration items of the resources from the time the configuration recorder is started.
- By default, AWS Config creates configuration items for every supported resource in the region, but can be customized to limited resource types.
- AWS Config keeps track of all changes to the resources by invoking the Describe or the List API call for each resource as well as related resources in the account
- Configuration items are delivered in a configuration stream to a S3 bucket
- AWS Config also tracks the configuration changes that were not initiated by the API. AWS Config examines the resource configurations periodically and generates configuration items for the configurations that have changed.
- AWS Config rules, if configured,
  - are evaluated continuously for resource configurations for desired settings.
  - Depending on the rule, resources are evaluated either in response to configuration changes or periodically.
  - When AWS Config evaluates the resources, it invokes the rule’s AWS Lambda function, which contains the evaluation logic for the rule.
  - Function returns the compliance status of the evaluated resources.
  - If a resource violates the conditions of a rule, the resource and the rule are flagged as noncompliant and a notification sent to SNS topic

AWS Config vs CloudTrail
- AWS Config reports on WHAT has changed, whereas CloudTrail reports on WHO made the change, WHEN, and from WHICH location.
- AWS Config focuses on the configuration of the AWS resources and reports with detailed snapshots on HOW the resources have changed, whereas CloudTrail focuses on the events, or API calls, that drive those changes. It focuses on the user, application, and activity performed on the system.
