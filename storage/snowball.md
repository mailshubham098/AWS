> #### [AWS Snowball]()
AWS Snowball is a service for customers who want to transport terabytes or petabytes of data to and from AWS, or who want to access the storage and compute power of the AWS Cloud locally and cost effectively in places where connecting to the internet might not be an option.

Overview
- AWS Snowball is a service that accelerates transferring large amounts of data into and out of AWS using physical storage devices, bypassing the Internet.
- This transport is done by shipping the data in the devices through a regional carrier. The devices are rugged shipping containers, complete with E Ink shipping labels.
- With a Snowball, you can transfer hundreds of terabytes or petabytes of data between your on-premises data centers and Amazon Simple Storage Service (Amazon S3).
- By shipping your data in Snowballs, you can transfer large amounts of data at a significantly faster rate than if you were transferring that data over the Internet, saving you time and money.
- Snowball is intended for transferring large amounts of data. If you want to transfer less than 10 terabytes of data between your on-premises data centers and Amazon S3, Snowball might not be your most economical choice.
- Snowball uses Snowball appliances shipped through your region's carrier. Each Snowball is protected by AWS Key Management Service (AWS KMS) and made physically rugged to secure and protect your data while the Snowball is in transit.
- In the US regions, Snowballs come in two sizes: 50 TB and 80 TB. All other regions have 80 TB Snowballs only.

Snowball Features
- You can import and export data between your on-premises data storage locations and Amazon S3.
- Snowball has an 80 TB model available in all regions, and a 50 TB model only available in the US regions.
- Encryption is enforced, protecting your data at rest and in physical transit.
- You don't have to buy or maintain your own hardware devices.
- You can manage your jobs through the AWS Snowball Management Console, or programmatically with the job management API.
- You can perform local data transfers between your on-premises data center and a Snowball. These transfers can be done through the Snowball client, a standalone downloadable client, or programmatically using Amazon S3 REST API calls with the downloadable Amazon S3 Adapter for Snowball.
- The Snowball is its own shipping container, and its E Ink display changes to show your shipping label when the Snowball is ready to ship.

AWS Snowball Use Case Differences

| Use case | Snowball | Snowball Edge |
| --- | --- | --- |
| Import data into Amazon S3 | ✓ | ✓ |
| Export from Amazon S3 | ✓ | ✓ |
| Durable local storage |  | ✓ |
| Local compute with AWS Lambda |  | ✓ |
| Amazon EC2 compute instances |  | ✓ |
| Use in a cluster of devices |  | ✓ |
| Use with AWS IoT Greengrass \(IoT\) |  | ✓ |
| Transfer files through NFS with a GUI |  | ✓ |

Storage capacities differences

| Storage capacity \(usable capacity\) | Snowball | Snowball Edge |
| --- | --- | --- |
|  50 TB \(42 TB\) \- US regions only  | ✓ |  |
|  80 TB \(72 TB\)  | ✓ |  |
|  100 TB \(83 TB\)  |  | ✓ |
|  100 TB Clustered \(45 TB per node\)  |  | ✓ |

Physical interfaces for management purposes

| Physical interface | Snowball | Snowball Edge |
| --- | --- | --- |
| E Ink display – used to track shipping information and configure your IP address\. | ✓ | ✓ |
| LCD display – used to manage connections and provide some administrative functions\. |  | ✓ |


Snowball Edge Tools
- Snowball client with Snowball Edge
  - must be downloaded from the AWS Snowball Edge Resources page and installed on a powerful workstation that you own.
  - Must be used to unlock the Snowball Edge or the cluster of Snowball Edge devices.
  - Can't be used to transfer data.
- Amazon S3 Adapter for Snowball with Snowball Edge
  - Is already installed on the Snowball Edge by default. It does not need to be downloaded or installed.
  - Can transfer data to or from the Snowball Edge.
  - Encrypts data on the Snowball Edge while the data is transferred to the device.
- File interface with Snowball Edge
  - Is already installed on the Snowball Edge by default.
  - Can transfer data by dragging and dropping files up to 150 GB in size from your computer to the buckets on the Snowball Edge through an easy-to-configure NFS mount point.
  - Encrypts data on the Snowball Edge while the data is transferred to the device.
- AWS IoT Greengrass console with Snowball Edge
  - With a Snowball Edge, you can use the AWS IoT Greengrass console to update your AWS IoT Greengrass group and the core running on the Snowball Edge.

Snowball Jobs
- Import into S3
  - The transfer of 80 TB or less of your data located in an on-premises data source, copied onto a single Snowball, and then moved into S3.
  - For import jobs, Snowballs and jobs have a one-to-one relationship, meaning that each job has exactly one Snowball associated with it. If you need additional Snowballs, you can create new import jobs or clone existing ones.
  - Each file or object that is imported must be less than or equal to 5 TB in size.
- Export from S3
  - The transfer of any amount of data located in S3, copied onto any number of Snowballs, and then moved one Snowball at a time into your on-premises data destination.
  - When you create an export job, it’s split into job parts. Each job part is no more than 80 TB in size, and each job part has exactly one Snowball associated with it.

Security
- All data transferred to a Snowball has two layers of encryption:
- A layer of encryption is applied in the memory of your local workstation.
  - This encryption uses AES GCM 256-bit keys, and the keys are cycled for every 60 GB of data transferred.
  - SSL encryption is a second layer of encryption for all data going onto or off of a standard Snowball.
- Snowball uses server side-encryption (SSE) to protect data at rest.
- Every Snowball job must be authenticated. You do this by creating and managing the IAM users in your account.

Data Validation
- When you copy a file from a local data source using the Snowball client or the Amazon S3 Adapter for Snowball, to the Snowball, a number of checksums are created. These checksums are used to automatically validate data as it’s transferred.

Pricing
- You pay a service fee per data transfer job (plus applicable shipping charges).
- Each job includes the use of a Snowball device for 10 days of onsite usage for free, and there is a small charge for extra onsite days. Shipping days, including the day the device is received and the day it is shipped back to AWS, are not counted toward the 10 free days.

Limits
- For security purposes, data transfers must be completed within 90 days of the Snowball being prepared.
- The default service limit for the number of Snowballs you can have at one time is 1.
- Individual Amazon S3 objects should be < 5TB size
- Currently, Snowball doesn't support  server-side encryption with AWS KMS–managed keys (SSE-KMS) or server-side encryption with customer-provided keys (SSE-C).Snowball does support server-side encryption with Amazon S3–managed encryption keys (SSE-S3).
