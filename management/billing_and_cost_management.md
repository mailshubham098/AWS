##### AWS Billing and Cost Management

- Cost Explorer tracks and analyzes your AWS usage. It is free for all accounts.
- Use Budgets to manage budgets for your account.
- Use Bills to see details about your current charges.
- Use Payment History to see your past payment transactions.
- AWS Billing and Cost Management closes the billing period at midnight on the last day of each month and then calculates your bill.
At the end of a billing cycle or at the time you choose to incur a one-time fee, -  AWS charges the credit card you have on file and issues your invoice as a downloadable PDF file.
- With CloudWatch, you can create billing alerts that notify you when your usage of your services exceeds thresholds that you define.
- Use cost allocation tags to track your AWS costs on a detailed level. AWS provides two types of cost allocation tags, an AWS generated tags and user-defined tags.

AWS Cost and Usage Reports
- The AWS Cost and Usage report provides information about your use of AWS resources and estimated costs for that usage.
- The AWS Cost and Usage report is a .csv file or a collection of .csv files that is stored in an S3 bucket. Anyone who has permissions to access the specified S3 bucket can see your billing report files.
- You can use the Cost and Usage report to track your Reserved Instance  utilization, charges, and allocations.
- Report can be automatically uploaded into AWS Redshift and/or AWS QuickSight for analysis.


AWS Cost Explorer

- Cost Explorer includes a default report that helps you visualize the costs and usage associated with your TOP FIVE cost-accruing AWS services, and gives you a detailed breakdown on all services in the table view.
- You can view data for up to the last 13 months, forecast how much you’re likely to spend for the next three months, and get recommendations for what Reserved Instances to purchase.
- Cost Explorer must be enabled before it can be used. You can enable it only if you’re the owner of the AWS account and you signed in to the account with your root credentials.
- If you’re the owner of a master account in an organization, enabling Cost Explorer enables Cost Explorer for all of the organization accounts. You can’t grant or deny access individually.
- You can create forecasts that predict your AWS usage and define a time range for the forecast.
- Other default reports available are:
  - The EC2 Monthly Cost and Usage report lets you view all of your AWS costs over the past two months, as well as your current month-to-date costs.
  - The Monthly Costs by Linked Account report lets you view the distribution of costs across your organization.
  - The Monthly Running Costs report gives you an overview of all of your running costs over the past three months, and provides forecasted numbers for the coming month with a corresponding confidence interval.

AWS Budgets
- Set custom budgets that alert you when your costs or usage exceed or are forecasted to exceed your budgeted amount.
- With Budgets, you can view the following information:
  - How close your plan is to your budgeted amount or to the free tier limits
  - Your usage to date, including how much you have used of your Reserved Instances
  - Your current estimated charges from AWS and how much your predicted usage will incur in charges by the end of the month
  - How much of your budget has been used
- Budget information is updated up to three times a day.
- Types of Budgets:
  - Cost budgets – Plan how much you want to spend on a service.
  - Usage budgets – Plan how much you want to use one or more services.
  - RI utilization budgets – Define a utilization threshold and receive alerts when your RI usage falls below that threshold.
  - RI coverage budgets – Define a coverage threshold and receive alerts when the number of your instance hours that are covered by RIs fall below that threshold.
- Budgets can be tracked at the monthly, quarterly, or yearly level, and you can customize the start and end dates.
- Budget alerts can be sent via email and/or Amazon SNS topic.
  First two budgets created are free of charge.
