# dojo - s3

## 加密,性能(搭配應用情境）


1. S3 encryption technique:(Client-side encryption)
    – Use an AWS KMS-managed customer master key.
    – Use a client-side master key.
  
2. a low-cost storage system in AWS
    - Use Lifecycle Policies in S3 to move obsolete data to Glacier.
  
3. [storage capacity]For the first month, all of these files will be accessed frequently but after that, they will rarely be accessed at all. cost-effective solution?
    - Store the files in S3 then after a month, change the storage class of the tutorialsdojo-finance prefix to One Zone-IA while the remaining go to Glacier using lifecycle policy.

4. By using Versioning and enabling MFA (Multi-Factor Authentication) Delete, you can secure and recover your S3 objects from accidental deletion or overwrite.

5.  aggregate big data at different country in the fastest way.
    - Enable Transfer Acceleration in the destination bucket and upload the collected data using Multipart Upload.

6. server side encrypt
    - Enable Server-Side Encryption on an S3 bucket to make use of AES-256 encryption.
    - Before sending the data to Amazon S3 over HTTPS, encrypt the data locally first using your own encryption keys.
    - Use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
    - Use Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)
    - Use Server-Side Encryption with Customer-Provided Keys (SSE-C)


7. Content Delivery Network (CDN)
    - Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds, all within a developer-friendly environment. CloudFront is integrated with AWS – both physical locations that are directly connected to the AWS global infrastructure, as well as other AWS services.



### ???
q: Category: CSAA – Design High-Performing Architectures
A Solutions Architect created a new Standard-class S3 bucket to store financial reports that are not frequently accessed but should immediately be available when an auditor requests them. To save costs, the Architect changed the storage class of the S3 bucket from Standard to Infrequent Access storage class.

In Amazon S3 Standard – Infrequent Access storage class, which of the following statements are true? (Select TWO.)

a: 
(o)It is designed for data that is accessed less frequently.
It automatically moves data to the most cost-effective access tier without any operational overhead.
Ideal to use for data archiving.
(o)It is designed for data that requires rapid access when needed.
It provides high latency and low throughput performance.


Amazon S3 Standard – Infrequent Access (Standard – IA)
  - high durability, throughput, and low latency 
  - ideal for long-term storage, backups, and as a data store for disaster recovery. 
