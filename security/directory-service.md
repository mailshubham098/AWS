> #### [AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html#choosing_an_option)

- AWS Directory Service provides multiple ways to set up and run Amazon Cloud Directory, Amazon Cognito, and Microsoft AD with other AWS services
- Provides below directory services
  - Amazon Cloud Directory
  - Amazon Cognito
  - Microsoft AD
  - AD Connector
  - Simple AD



#### AWS Managed Microsoft AD


-  AWS Directory Service for Microsoft Active Directory is powered by an actual Microsoft Windows Server Active Directory (AD), managed by AWS in the AWS Cloud
- enables you to migrate a broad range of Active Directory–aware applications to the AWS Cloud.
- works with Microsoft SharePoint, Microsoft SQL Server Always On Availability Groups, and many .NET applications
-  is approved for applications in the AWS Cloud that are subject to U.S. Health Insurance Portability and Accountability Act (HIPAA) or Payment Card Industry Data Security Standard (PCI DSS) compliance when you enable compliance for your directory.
- All compatible applications work with user credentials that you store in AWS Managed Microsoft AD, or you can connect to your existing AD infrastructure with a trust and use credentials from an Active Directory running on-premises or on EC2 Windows
-  If you join EC2 instances to your AWS Managed Microsoft AD, your users can access Windows workloads in the AWS Cloud with the same Windows single sign-on (SSO) experience as when they access workloads in your on-premises network
- also supports federated use cases using Active Directory credentials.
-  With AWS Single Sign-On, you can also obtain short-term credentials for use with the AWS SDK and CLI, and use preconfigured SAML integrations to sign in to many cloud applications.
- By adding Azure AD Connect, and optionally Active Directory Federation Service (AD FS), you can sign in to Microsoft Office 365 and other cloud applications with credentials stored in AWS Managed Microsoft AD.
- can also enable multi-factor authentication (MFA) for AWS Managed Microsoft AD to provide an additional layer of security when users access AWS applications from the Internet.
- Because Active Directory is an LDAP directory, you can also use AWS Managed Microsoft AD for Linux Secure Shell (SSH) authentication and for other LDAP-enabled applications.
- AWS Managed Microsoft AD is available in two editions:
  - Standard Edition: AWS Managed Microsoft AD (Standard Edition) is optimized to be a primary directory for small and midsize businesses with up to 5,000 employees. It provides you enough storage capacity to support up to 30,000* directory objects, such as users, groups, and computers.
  - Enterprise Edition: AWS Managed Microsoft AD (Enterprise Edition) is designed to support enterprise organizations with up to 500,000* directory objects.

- Once your directory is created, you can use it for a variety of tasks:
  - Manage users and groups
  - Provide single sign-on to applications and services
  - Create and apply group policy
  - Securely connect to Amazon EC2 Linux and Windows instances
  - Simplify the deployment and management of cloud-based Linux and Microsoft Windows workloads
  - You can use AWS Managed Microsoft AD to enable multi-factor authentication by integrating with your existing RADIUS-based MFA infrastructure to provide an additional layer of security when users access AWS applications.

- You can integrate AWS managed microsoft AD easily with your existing Acive Directory by using Active Directory trust relationship

When to use
- AWS Managed Microsoft AD is your best choice if you need actual Active Directory features to support AWS applications or Windows workloads, including Amazon Relational Database Service for Microsoft SQL Server.
- It's also best if you want a standalone AD in the AWS Cloud that supports Office 365 or you need an LDAP directory to support your Linux applications.


Prerequisites
-  At least two subnets. Each of the subnets must be in a different Availability Zone.
- Several ports needs to be open b/w the two subnets
- The VPC must have default hardware tenancy.
- cannot create a AWS Managed Microsoft AD in a VPC using addresses in the 198.18.0.0/15 address space.
- AWS Directory Service does not support using Network Address Translation (NAT) with Active Directory. Using NAT can result in replication errors.

Key Concepts
- Active Directory Schema
  - Schema Elements
    - Attributes
    - Classes
    - Object identifier (OID)
    - Schema linked attributes

Use cases
- Sign In to AWS Applications and Services with AD Credentials
- Manage Amazon EC2 Instances
- Provide Directory Services to Your AD-Aware Workloads
- SSO to Office 365 and Other Cloud Applications
- Extend Your On-Premises AD to the AWS Cloud
- Share Your Directory to Seamlessly Join Amazon EC2 Instances to a Domain Across AWS Accounts


Seamless Domain Joins
- it is a feature that allows you to join your Amazon EC2 for windows Server instances seamlessly to a domain , at the time of the launch and from AWS console .You can join instances to AWS managed microsoft AD that you launh in the  AWS cloud
- you cannot use this feature from AWS console for the existing EC2 for windows server instances , but you can join these instances to a domain using the EC2 API or by using the power shell on the instance

Security and Monitoring
- both HIPAA and PCI DSS compliant
- manages both users and devices by using native Active directory group policy objects (GPOs)
- uses same kerberos-based authentication as active directory to deliver SSO
- supports federation for users and groups to the AWS management console
- Amazon EBS volumes used in the directory service are encrypted


Pricing
- You pay only for the type and size of the managed directory that you use
- AWS managed Microsoft AD allows you to use a directory in one account and share it with multiple accounts and VPCs.There is an additioned sharing charge  for each additional account to which you share a directory

#### AD Connector
-  is a proxy service that provides an easy way to connect compatible AWS applications, such as Amazon WorkSpaces, Amazon QuickSight, and Amazon EC2 for Windows Server instances, to your existing on-premises Microsoft Active Directory
- eliminates the need of directory synchronization or the cost and complexity of hosting a federation infrastructure.
- When you add users to AWS applications such as Amazon QuickSight, AD Connector reads your existing Active Directory to create lists of users and groups to select from.
- When users log in to the AWS applications, AD Connector forwards sign-in requests to your on-premises Active Directory domain controllers for authentication.
- AD Connector works with many AWS applications and services including Amazon WorkSpaces, Amazon WorkDocs, Amazon QuickSight, Amazon Chime, Amazon Connect, and Amazon WorkMail.
- You can also join your EC2 Windows instances to your on-premises Active Directory domain through AD Connector using seamless domain join
-    allows your users to access the AWS Management Console and manage AWS resources by logging in with their existing Active Directory credentials.
- not compatible with RDS SQL Server.
- can also use AD Connector to enable multi-factor authentication (MFA) for your AWS application users by connecting it to your existing RADIUS-based MFA infrastructure.
- With AD Connector, you continue to manage your Active Directory as you do now.

When to use
- AD Connector is your best choice when you want to use your existing on-premises directory with compatible AWS services.

Prerequisites
- same as AWS managed Microsoft AD


#### Simple AD

-  is a Microsoft Active Directory–compatible directory from AWS Directory Service that is powered by Samba 4.
- supports basic Active Directory features such as user accounts, group memberships, joining a Linux domain or Windows based EC2 instances, Kerberos-based SSO, and group policies.
-  provides monitoring, daily snap-shots, and recovery as part of the service.
-  is a standalone directory in the cloud, where you create and manage user identities and manage access to applications.
- can use many familiar Active Directory–aware applications and tools that require basic Active Directory features. Simple AD is compatible with the following AWS applications: Amazon WorkSpaces, Amazon WorkDocs, Amazon QuickSight, and Amazon WorkMail.
- an also sign in to the AWS Management Console with Simple AD user accounts and to manage AWS resources.
- does not support multi-factor authentication (MFA), trust relationships, DNS dynamic update, schema extensions, communication over LDAPS, PowerShell AD cmdlets, or FSMO role transfer
- not compatible with RDS SQL Server

When to use
- You can use Simple AD as a standalone directory in the cloud to support Windows workloads that need basic AD features, compatible AWS applications, or to support Linux workloads that need LDAP service.

#### Amazon Cloud Directory
- is a cloud-native directory that can store hundreds of millions of application-specific objects with multiple relationships and schemas.
- Use Amazon Cloud Directory if you need a highly scalable directory store for your application’s hierarchical data.

When to use
- is a great choice when you need to build application directories such as device registries, catalogs, social networks, organization structures, and network topologies.


#### Amazon Cognito
- is a user directory that adds sign-up and sign-in to your mobile app or web application using Amazon Cognito User Pools.
- supports sign-in with social identity providers like facebook, google ,amazon and enterprise identity providers via SAML 2.0

When to use
- You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory.
- This fully managed service scales to support hundreds of millions of users.


#### When to use what
Simple AD

- Simple AD is a Microsoft Active Directory–compatible directory from AWS Directory Service that is powered by Samba 4
- Simple AD **supports basic Active Directory features** such as user accounts, group memberships, joining a Linux domain or Windows based EC2 instances, Kerberos-based SSO, and group policies. AWS provides monitoring, daily snap-shots, and recovery as part of the service.
- Simple AD is a standalone directory in the cloud, where you create and manage user identities and manage access to applications

Amazon Cognito
- Amazon Cognito is a user directory that adds sign-up and sign-in to your mobile app or web application using Amazon Cognito User Pools.
- Use Amazon Cognito if you develop high-scale SaaS applications and need a scalable directory to manage and authenticate your subscribers and that works with social media identities.
- You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory. This fully managed service scales to support hundreds of millions of users.

Amazon Cloud Directory
- Amazon Cloud Directory is a cloud-native directory that can store hundreds of millions of application-specific objects with multiple relationships and schemas. Use Amazon Cloud Directory if you need a highly scalable directory store for your application’s hierarchical data.
- Use Amazon Cloud Directory if you need a cloud-scale directory to share and control access to hierarchical data between your applications.
- Amazon Cloud Directory is a great choice when you need to build application directories such as device registries, catalogs, social networks, organization structures, and network topologies.

AD Connector
- AD Connector is a proxy service that provides an easy way to connect compatible AWS applications, such as Amazon WorkSpaces, Amazon QuickSight, and Amazon EC2 for Windows Server instances, to your existing on-premises Microsoft Active Directory.
- Use AD Connector if you only need to allow your on-premises users to log in to AWS applications and services with their Active Directory credentials. You can also use AD Connector to join Amazon EC2 instances to your existing Active Directory domain.

AWS Managed Microsoft AD
- Select AWS Directory Service for Microsoft Active Directory if you need an actual Microsoft Active Directory in the AWS Cloud that supports Active Directory–aware workloads, or AWS applications and services such as Amazon WorkSpaces and Amazon QuickSight, or you need LDAP support for Linux applications.
- AWS Managed Microsoft AD is your best choice if you need actual Active Directory features to support AWS applications or Windows workloads, including Amazon Relational Database Service for Microsoft SQL Server. It's also best if you want a standalone AD in the AWS Cloud that supports Office 365 or you need an LDAP directory to support your Linux applications.
