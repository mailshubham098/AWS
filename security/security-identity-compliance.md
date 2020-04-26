> #### [Identity and access management IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)


IAM Features
- Shared access to your AWS account
- Granular permissions
- Secure access to AWS resources for applications that run on Amazon EC2
- Multi-factor authentication (MFA)
- Identity federation
- Identity information for assurance
- PCI DSS Compliance
- Integrated with many AWS services
- Eventually Consistent
- Free to use

-- IAM Users, Roles, Policy and Users and Groups
--Users :- Using IAM, We can create and Manage AWS users, and use permissions to allow and deny their access to AWS resources.

--Groups:- The Users created can also be divided among groups, and then the roles and policies that apply on the group, apply
	          on the user level as well.
--Roles :- 	Roles are access permissions from One aws service to other aws service and cannot be assigned to users and groups on the   other hand permissions are related to group and user permissions to roles, groups and objects.		

Used to delegate access to AWS Resources, Can be assigned to an AWS Resource (ie EC2 instance) or third party accounts (ie. another AWS account or SAML 2.0). Up to 10 policies can be assigned to a role (this is new, it used to be 2)
Roles CANNOT be assigned to Users or Groups.

--Policy :- To assign permission to a user, groups, Roles and resources, you create a policy, which is a document that explicitly
	    lists permissions.

Applied to Roles, Groups or Objects (ie S3) Uses JSON format Describe the level of access applied to an AWS resource by an AWS resource or user, To be more specific Policies either grant or deny the ability to call a specific method on a specific resource in the AWS API.

--Users & Groups
Users are individual accounts with usernames & passwords; and Access Key ID's and Access Keys Groups are collections of users accounts ie. Developers, Admins, DevOps Policies can either be applied to Groups or to individual accounts Up to 10 policies per group.

The AWS recommend security practice is to use Roles wherever possible to handle authentication for applications. So for example if you have a web server which needs to access RDS instances, and S3 buckets; instead of creating a user for authentication to those resources, and have the AWS Access Key's for that user saved on the web server in a config file somewhere; you would create a role with the relevant policies applied and assign the role to the EC2 instance used by the web server.		


Terms

- Identity : "Who is that user?" ( Authentication )
- Federating Existing Users - user external corp directory /internaer Identity/ SSO to federate the Users

- Federated Users and Roles : Federated users don't have permanent identities in your AWS account the way that IAM users do. To assign permissions to federated users, you can create an entity referred to as a role and define permissions for the role.
- Identity-based policies control what actions the identity can perform, on which resources, and under what conditions
  - Managed policies
    - AWS managed policies
    - Customer managed policies
  - Inline policies
- Resource-based policies control what actions a specified principal can perform on that resource and under what conditions. Resource-based policies are inline policies, and there are no managed resource-based policies. To enable cross-account access, you can specify an entire account or IAM entities in another account as the principal in a resource-based policy.
- The IAM service supports only one type of resource-based policy called a role trust policy, which is attached to an IAM role. Because an IAM role is both an identity and a resource that supports resource-based policies, you must attach both a trust policy and an identity-based policy to an IAM role. Trust policies define which principal entities (accounts, users, roles, and federated users) can assume the role.

 - access key : You need an access key if you want to make AWS requests using the AWS SDKs, the AWS Command Line Tools, or the API operations.

 IAM Roles
 - An IAM role is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS. However, a role does not have any credentials (password or access keys) associated with it. Instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. An IAM user can assume a role to temporarily take on different permissions for a specific task. A role can be assigned to a federated user who signs in by using an external identity provider instead of IAM. AWS uses details passed by the identity provider to determine which role is mapped to the federated user.


 Types of aws access :
 - Access for users under your AWS account
 - Non-AWS user access via identity federation between your authorization system and AWS
 - Cross-account access between AWS accounts


 - Delegate Access Across AWS Accounts Using Roles ( STS )

 IAM Best practices
- Lock Away Your AWS Account Root User Access Keys
- Create Individual IAM Users
- Use Groups to Assign Permissions to IAM Users
- Grant Least Privilege
- Get Started Using Permissions With AWS Managed Policies
- Use Customer Managed Policies Instead of Inline Policies
- Use Access Levels to Review IAM Permissions
- Configure a Strong Password Policy for Your Users
- Enable MFA for Privileged Users
- Use Roles for Applications That Run on Amazon EC2 Instances
- Use Roles to Delegate Permissions
- Do Not Share Access Keys
- Rotate Credentials Regularly
- Remove Unnecessary Credentials
- Use Policy Conditions for Extra Security
- Monitor Activity in Your AWS Account
- Video Presentation About IAM Best Practices


When to Create an IAM User (Instead of a Role)
- You created an AWS account and you're the only person who works in your account.
- Other people in your group need to work in your AWS account, and your group is using no other identity mechanism.
- You want to use the command-line interface (CLI) to work with AWS

When to Create an IAM Role (Instead of a User)
- You're creating an application that runs on an Amazon Elastic Compute Cloud (Amazon EC2) instance and that application makes requests to AWS.
- You're creating an app that runs on a mobile phone and that makes requests to AWS.
- Users in your company are authenticated in your corporate network and want to be able to use AWS without having to sign in again—that is, you want to allow users to federate into AWS.

Roles Terms and Concepts

- Roles can be used by the following:
  - An IAM user in the same AWS account as the role
  - An IAM user in a different AWS account than the role
  - A web service offered by AWS such as Amazon Elastic Compute Cloud (Amazon EC2)
  - An external user authenticated by an external identity provider (IdP) service that is compatible with SAML 2.0 or OpenID Connect, or a custom-built identity broker.

- AWS service role
- AWS service role for an EC2 instance
- AWS service-linked role
- Role chaining

Common Scenarios for Temporary Credentials
- Identity Federation
  - Enterprise identity federation
    - Custom federation broker
    - Federation using SAML 2.0
  - Web identity federation
- Roles for Cross-account Access
- Roles for Amazon EC2

Policy
- Policy summary (list of services) -> Service summary (list of actions) -> Action summary (List of resources)

- Identity-based policies
	- control what actions the identity can perform, on which resources, and under what conditions.
	- Managed policies
		- AWS managed policies
		- Customer managed policies
	- Inline policies
- Resource-based policies
 	- control what actions a specified principal can perform on that resource and under what conditions. Resource-based policies are inline policies, and there are no managed resource-based policies. To enable cross-account access, you can specify an entire account or IAM entities in another account as the principal in a resource-based policy.

Identity federation
- AWS supports identity federation with SAML 2.0, an open standard that many identity providers (IdPs) use. This feature enables federated single sign-on (SSO), so users can log into the AWS Management Console or call the AWS APIs without you having to create an IAM user for everyone in your organization. By using SAML, you can simplify the process of configuring federation with AWS, because you can use the IdP's service instead of writing custom identity proxy code.
- Web identity federation is primarily used to let users sign in via a well-known external identity provider (IdP), such as Login with Amazon, Facebook, Google. It does not utilize Active Directory


- Groups cannot be nested
- An IAM user can be member of max 10 Groups
- Default IAM user is with no permission
- Maximum number of MFA devices in use per AWS account - 1
-  Although a role is usually assigned to an EC2 instance when you launch it, a role can also be assigned to an EC2 instance that is already running.You can also change the permissions on the IAM role associated with a running instance, and the updated permissions take effect almost immediately.
- You can assign a role to an EC2 instance that is already running
- You can only associate one IAM role with an EC2 instance at this time. This limit of one role per instance cannot be increased.
- If you delete the role of a running ec2 instance , Any application running on the instance that is using the role will be denied access immediately.
- Managed policies can only be attached to IAM users, groups, or roles. You cannot use them as resource-based policies.
-

[FAQs](https://aws.amazon.com/iam/faqs/)

Q: Which permissions are required to launch EC2 instances with an IAM role?

You must grant an IAM user two distinct permissions to successfully launch EC2 instances with roles:
- Permission to launch EC2 instances.
- Permission to associate an IAM role with EC2 instances.

Q: What is a service-linked role?

A service-linked role is a type of role that links to an AWS service (also known as a linked service) such that only the linked service can assume the role. Using these roles, you can delegate permissions to AWS services to create and manage AWS resources on your behalf.

Q: Can I assume a service-linked role?

No. A service-linked role can be assumed only by the linked service. This is the reason why the trust policy of a service-linked role cannot be modified.


#### [AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/what-is-aws-artifact.html)
- AWS Artifact provides on-demand downloads of AWS security and compliance documents, such as AWS ISO certifications, Payment Card Industry (PCI), and Service Organization Control (SOC) reports.
- You can submit the security and compliance documents (also known as audit artifacts) to your auditors or regulators to demonstrate the security and compliance of the AWS infrastructure and services that you use
- use these documents as guidelines to evaluate your own cloud architecture and assess the effectiveness of your company's internal controls
- You can also use AWS Artifact to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA)
- follows Shared Responsibility Model (aws and customers)

#### [AWS WAF - web application firewall](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)
- AWS WAF is a web application firewall that lets you monitor web requests that are forwarded to Amazon CloudFront distributions or an Application Load Balancer. You can also use AWS WAF to block or allow requests based on conditions that you specify, such as the IP addresses that requests originate from or values in the requests.
- use AWS WAF to control how API Gateway, Amazon CloudFront or an Application Load Balancer responds to web requests
- Create and Configure a Web Access Control List (Web ACL), fine-grained control over the web requests that your Amazon API Gateway API, Amazon CloudFront distribution or Application Load Balancer responds to

- **Conditions**
  - Conditions define the basic characteristics that you want AWS WAF to watch for in web requests:
    -  cross-site scripting.
    - IP addresses or address ranges that requests originate from
    - Country or geographical location that requests originate from
    - Length of specified parts of the request, such as the query string
    - SQL injection.
    - Strings that appear in the request eg User-agent
- **Rules**
  - You combine conditions into rules to precisely target the requests that you want to allow, block, or count. AWS WAF provides two types of rules:
    - **Regular rule**
    - **Rate-based rule**

#### [AWS Shield](https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
- AWS provides AWS Shield Standard and AWS Shield Advanced for protection against DDoS (distributed denial of service ) attacks
- AWS Shield Advanced provides expanded protection against many types of attacks
  - User Datagram Protocol (UDP) reflection attacks
  - SYN flood
  - DNS query flood
  - HTTP flood/cache-busting (layer 7) attacks

#### [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)

- Amazon GuardDuty is a continuous security monitoring service that analyzes and processes the following data sources: VPC Flow Logs, AWS CloudTrail event logs, and DNS logs
- It uses threat intelligence feeds, such as lists of malicious IPs and domains, and machine learning to identify unexpected and potentially unauthorized and malicious activity within your AWS environment.
- This can include issues like escalations of privileges, uses of exposed credentials, or communication with malicious IPs, URLs, or domains. For example, GuardDuty can detect compromised EC2 instances serving malware or mining bitcoin.
- GuardDuty extracts various fields from these logs for profiling and anomaly detection, and then discards the logs.

#### [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html)
- Amazon Inspector is a security vulnerability assessment service that helps improve the security and compliance of your AWS resources. Amazon Inspector automatically assesses resources for vulnerabilities or deviations from best practices, and then produces a detailed list of security findings prioritized by level of severity. Amazon Inspector includes a knowledge base of hundreds of rules mapped to common security standards and vulnerability definitions that are regularly updated by AWS security researchers.
- Concept
  - Amazon Inspector agent
  - Assessment run
  - Assessment target
  - Assessment template
  - Finding
  - Rule
  - Rules package
  - Telemetry

#### [AWS Firewall Manager](https://docs.aws.amazon.com/waf/latest/developerguide/fms-chapter.html)
- AWS Firewall Manager simplifies your AWS WAF administration and maintenance tasks across multiple accounts and resources. With AWS Firewall Manager, you set up your firewall rules just once. The service automatically applies your rules across your accounts and resources, even as you add new resources.
-  AWS Firewall Manager simplifies your AWS WAF and AWS Shield Advanced administration and maintenance tasks across multiple accounts and resources
- With Firewall Manager, you set up your AWS WAF firewall rules and Shield Advanced protections just once. The service automatically applies the rules and protections across your accounts and resources, even as you add new resources.
- Firewall Manager provides these benefits:
  - Helps to protect resources across accounts
  - Helps to protect all resources of a particular type, such as all Amazon CloudFront distributions
  - Helps to protect all resources with specific tags
  - Automatically adds protection to resources that are added to your account
  - Allows you to subscribe all member accounts in an AWS Organizations organization to AWS Shield Advanced, and automatically subscribes new in-scope accounts that join the organization
  - Lets you use your own custom rules, or purchase managed rules from AWS Marketplace

#### [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
- AWS Secrets Manager helps you to securely encrypt, store, and retrieve credentials for your databases and other services. Instead of hardcoding credentials in your apps, you can make calls to Secrets Manager to retrieve your credentials whenever needed. Secrets Manager helps you protect access to your IT resources and data by enabling you to rotate and manage access to your secrets.
- Features of Secrets Manager
  - Programmatically Retrieve Encrypted Secret Values at Runtime Instead of Storing Them
  - Store Just About Any Kind of Secret
  - Encrypt Your Secret Data
    - Secrets Manager associates every secret with an AWS KMS CMK. It can be either the default CMK for Secrets Manager for the account, or a customer-created CMK.
  - Automatically Rotate Your Secrets
    - You can configure Secrets Manager to automatically rotate your secrets
    - You define and implement rotation with an AWS Lambda function
  - Control Who Can Access Secrets
    - You can attach AWS Identity and Access Management (IAM) permission policies to your users, groups, and roles that grant or deny access to specific secrets, and restrict what they can do with those secrets
    - Alternatively, you can attach a resource-based policy directly to the secret to grant permissions that specify who is allowed to read or modify the secret and its versions.

#### [AWS Single Sign-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)
- AWS Single Sign-On is a cloud-based service that simplifies managing SSO access to AWS accounts and business applications. You can control SSO access and user permissions across all your AWS accounts in AWS Organizations. You can also administer access to popular business applications and custom applications that support Security Assertion Markup Language (SAML) 2.0. In addition, AWS SSO offers a user portal where your users can find all their assigned AWS accounts, business applications, and custom applications in one place.

#### [Amazon Macie](https://docs.aws.amazon.com/macie/latest/userguide/what-is-macie.html)
- Amazon Macie is a security service that uses machine learning to automatically discover, classify, and protect sensitive data in AWS. Amazon Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property, and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved.   
- Features of Amazon Macie
  - **Data Discovery and Classification**
  - Amazon Macie enables you to identify business-critical data and analyze access patterns and user behavior as follows:
    - Continuously monitor new data in your AWS environment
    - Use artificial intelligence to understand access patterns of historical data
    - Automatically access user activity, applications, and service accounts
    - Use natural language processing (NLP) methods to understand data
    - Intelligently and accurately assign business value to data and prioritize business-critical data based on your unique organization
    - Create your own security alerts and custom policy definitions
  - **Data Security**
  - Amazon Macie enables you to be proactive with security compliance and achieve preventive security as follows:
    - Identify and protect various data types, including PII, PHI, regulatory documents, API keys, and secret keys
    - Verify compliance with automated logs that allow for instant auditing
    - Identify changes to policies and access control lists
    - Observe changes in user behavior and receive actionable alerts
    - Receive notifications when data and account credentials leave protected zones
    - Detect when large quantities of business-critical documents are shared internally and externally

  #### [AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)
  - AWS Resource Access Manager (AWS RAM) enables you to share your resources with any AWS account or organization in AWS Organizations. Customers who operate multiple accounts can create resources centrally and use AWS RAM to share them with all of their accounts to reduce operational overhead. AWS RAM is available at no additional charge.  
