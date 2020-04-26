> #### [AWS Key management service - KMS]()
AWS Key Management Service (KMS) is an encryption and key management service scaled for the cloud. KMS keys and functionality are used by other AWS services, and you can use them to protect data in your own applications that use AWS.

Overview

- KMS is integrated with CloudTrail, which provides you the ability to audit who used which keys, on which resources, and when.
- Customer master keys (CMKs) are used to control access to data encryption keys that encrypt and decrypt your data.
- You can choose to have KMS automatically rotate master keys created within KMS once per year without the need to re-encrypt data that has already been encrypted with your master key.
- To help ensure that your keys and your data is highly available, KMS stores multiple copies of encrypted versions of your keys in systems that are designed for 99.999999999% durability.

Concepts

- Customer Master Keys (CMKs)
  - The primary resources in AWS KMS are customer master keys (CMKs). You can use a CMK to encrypt and decrypt up to 4 KB (4096 bytes) of data. Typically, you use CMKs to generate, encrypt, and decrypt the data keys that you use outside of AWS KMS to encrypt your data. This strategy is known as envelope encryption.
  - CMKs are created in AWS KMS and never leave AWS KMS unencrypted.
  - To use or manage your CMK, you access them through AWS KMS.
  - AWS KMS does not store, manage, or track your data keys.
  - There are three types of CMKs in AWS accounts:
    - Customer managed CMKs
      - Customer managed CMKs are CMKs in your AWS account that you create, own, and manage. You have full control over these CMKs, including establishing and maintaining their key policies, IAM policies, and grants, enabling and disabling them, rotating their cryptographic material, adding tags, creating aliases that refer to the CMK, and scheduling the CMKs for deletion.
      - You can use your customer managed CMKs in cryptographic operations and audit their use in AWS CloudTrail logs.
      - Customer managed CMKs incur a monthly fee and a fee for use in excess of the free tier. They are counted against the AWS KMS limits for your account
    - AWS managed CMKs
      - AWS managed CMKs are CMKs in your account that are created, managed, and used on your behalf by an AWS service that integrates with AWS KMS
      - You can identify AWS managed CMKs by their aliases, which have the format aws/service-name, such as aws/redshift.
      - You can view the AWS managed CMKs in your account, view their key policies, and audit their use in AWS CloudTrail logs. However, you cannot manage these CMKs or change their permissions
      -  And, you cannot use AWS managed CMKs in cryptographic operations directly; the service that creates them uses them on your behalf.
      - You do not pay a monthly fee for AWS managed CMKs. They can be subject to fees for use in excess of the free tier, but some AWS services cover these costs for you.
      - AWS managed CMKs do not count against limits on the number of CMKs in each region of your account, but when they are used on behalf of a principal in your account, they count against request rate limits.  
    - AWS owned CMKs
      - AWS owned CMKs are not in your AWS account. They are part of a collection of CMKs that AWS owns and manages for use in multiple AWS accounts.
      - AWS services can use AWS owned CMKs to protect your data.
      - You cannot view, manage, or use AWS owned CMKs, or audit their use. However, you do not need to do any work or change any programs to protect the keys that encrypt your data
      - You are not charged a monthly fee or a usage fee for use of AWS owned CMKs and they do not count against AWS KMS limits for your account.

| Type of CMK | Can view | Can manage | Used only for my AWS account |
| --- | --- | --- | --- |
| [Customer managed CMK](#customer-cmk) | Yes | Yes | Yes |
| [AWS managed CMK](#aws-managed-cmk) | Yes | No | Yes |
| [AWS owned CMK](#aws-owned-cmk) | No | No | No |     


- Data Keys
  - Data keys are encryption keys that you can use to encrypt data, including large amounts of data and other data encryption keys.
  - You can use AWS KMS customer master keys (CMKs) to generate, encrypt, and decrypt data keys.
  - AWS KMS does not store, manage, or track your data keys, or perform cryptographic operations with data keys. ou must use and manage data keys outside of AWS KMS.

- Envelope Encryption
  - When you encrypt your data, your data is protected, but you have to protect your encryption key. One strategy is to encrypt it. Envelope encryption is the practice of encrypting plaintext data with a data key, and then encrypting the data key under another key.
  - You can even encrypt the data encryption key under another encryption key, and encrypt that encryption key under another encryption key. But, eventually, one key must remain in plaintext so you can decrypt the keys and your data. This top-level plaintext key encryption key is known as the **master key**.
  - Benefits of envelop encryption
    - Protecting data keys
    - Encrypting the same data under multiple master keys
    - Combining the strengths of multiple algorithms

- Encryption Context
  - All AWS KMS cryptographic operations (the Encrypt, Decrypt, ReEncrypt, GenerateDataKey, and GenerateDataKeyWithoutPlaintext) accept an encryption context, an optional set of key–value pairs that can contain additional contextual information about the data. AWS KMS uses the encryption context as additional authenticated data (AAD) to support authenticated encryption.
- Key Policies
  - When you create a CMK, you determine who can use and manage that CMK. These permissions are contained in a document called the key policy.

- Grants
  - A grant is another mechanism for providing permissions, an alternative to the key policy. You can use grants to give long-term access that allows AWS principals to use your customer managed CMKs

- Grant Tokens
  - When you create a grant, the permissions specified in the grant might not take effect immediately due to eventual consistency. If you need to mitigate the potential delay, use the grant token that you receive in the response to your CreateGrant API request. You can pass the grant token with some AWS KMS API requests to make the permissions in the grant take effect immediately

- Auditing CMK Usage
  - You can use AWS CloudTrail to audit key usage. CloudTrail creates log files that contain a history of AWS API calls and related events for your account

- Key Management Infrastructure
  - A common practice in cryptography is to encrypt and decrypt with a publicly available and peer-reviewed algorithm such as AES (Advanced Encryption Standard) and a secret key. One of the main problems with cryptography is that it's very hard to keep a key secret. This is typically the job of a key management infrastructure (KMI). AWS KMS operates the KMI for you. AWS KMS creates and securely stores your master keys, called CMKs.

Importing Keys
- A CMK contains the key material used to encrypt and decrypt data. When you create a CMK, by default AWS KMS generates the key material for that CMK. But you can create a CMK without key material and then import your own key material into that CMK.
- When you import key material, you can specify an expiration date. When the key material expires, KMS deletes the key material and the CMK becomes unusable. You can also delete key material on demand.

Deleting Keys
- Deleting a CMK deletes the key material and all metadata associated with the CMK and is irreversible. You can no longer decrypt the data that was encrypted under that CMK, which means that data becomes unrecoverable.
- You can temporarily disable keys so they cannot be used by anyone.
KMS supports custom key stores backed by AWS CloudHSM clusters. A key store is a secure location for storing cryptographic keys.
- You can connect directly to AWS KMS through a private endpoint in your VPC instead of connecting over the internet. When you use a VPC endpoint, communication between your VPC and AWS KMS is conducted entirely within the AWS network.

Pricing

- Each customer master key that you create in KMS, regardless of whether you use it with KMS-generated key material or key material imported by you, costs you until you delete it.
- For a CMK with key material generated by KMS, if you opt-in to have the CMK automatically rotated each year, each newly rotated version will raise the cost of the CMK per month.


Limits


| Resource | Default Limit | Applies To |
| --- | --- | --- |
| [Customer Master Keys \(CMKs\)](#customer-master-keys-limit) | 10,000 | Customer managed CMKs |
| [Aliases](#aliases-limit) | 10,000 | Customer created aliases |
| [Key policy document size](#key-policy-limit) | 32 KB \(32,768 bytes\) | Customer managed CMKsAWS managed CMKs |
| [Grants per CMK](#grants-per-key) | 10,000 | Customer managed CMKs |
| [Grants for a given principal per CMK](#grants-per-principal-per-key) | 500 | Customer managed CMKsAWS managed CMKs |
| [Requests per second](#requests-per-second) | Varies by API operation; see [table](#requests-per-second-table)\. | Customer managed CMKsAWS managed CMKs |
