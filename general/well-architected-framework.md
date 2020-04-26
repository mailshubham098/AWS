The pillars of the AWS Well-Architected Framework
- **Operational Excellence** - The ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures.
- **Security** - The ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies.
- **Reliability** - The ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues.
- **Performance Efficiency** - The ability to use computing resources efficiently
to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.
- **Cost Optimization** - The ability to run systems to deliver business value at the lowest price point.

General Design Principles
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
- Drive architectures using data
- Improve through game days


Operational Excellence

- Design Principles
  - Perform operations as code
  - Annotate documentation
  - Make frequent, small, reversible changes
  - Refine operations procedures frequently
  - Anticipate failure
  - Learn from all operational failures

There are three best practice areas for operational excellence in the cloud:
- Prepare
- Operate
- Evolve

Key AWS service
- The AWS service that is essential to Operational Excellence is AWS **CloudFormation**, which you can use to create templates based on best practices. This enables you
to provision resources in an orderly and consistent fashion from your development through production environments.
- Prepare - **AWSConfig** and AWSConfig rules can be used to create standards for workloads and to determine if environments are compliant with those standards before being put into production.
- Operate - Amazon **cloudwatch** allows to monitor the operational health of a workload
- Evolve - Amazon **Elasticsearch** Service(AmazonES) allows you to analyze your log data to gain actionable insights quickly and securely.



- AWS X-Ray, CloudWatch, CloudTrail, and VPC Flow Logs enabling the identification of workload issues in support of root cause analysis and remediation.
