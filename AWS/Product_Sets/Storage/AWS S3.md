# AWS S3

## OVERALL

**Simple Storage Service (S3)** provides developers and IT teams with secure, durable, highly-scalable object strage.

It has simple web services interface to store and retrieve any amount of data from anywhere on the web.

## BASICS

  - S3 is object-based (stores files, without installation of OS, DB, etc - which is BLOB-storage)
  - files can be from 0 to 5TB
  - there's unlimited storage
  - files are stored in buckets
  - S3 is universal namespace (names should be unique globally)
  - when you upload file to S3, you will receive a HTTP 200 code if the upload successfull
  
  
  **Bucket** - thing of it as just folder in the cloud where you can group your files.
  Why it's not really folder - because each bucket has its domain address, so it has to be unique globally.
  
  Example of the Bucket name: [https://s3.eu-central-1.amazonaws.com/xburser](https://s3.eu-central-1.amazonaws.com/xburser)



## DATA CONSISTENCY MODEL

  - Read after Write consistency for PUTS of new object (you initially upload your file - you can see it in milliseconds)
  - Eventual Consistency for overwrite PUTS and DELETES (if you change already uploaded file, it can take some time to see changes - as files can be spread among several AZ it can take some time to change all of them)


## KEY-VALUE STORE

Object consist of
  
  - key (object name)
  - value (object data)
  - version id
  - metadata (when it was initially uploaded, etc)
  - subresources:
    - ACL
    - torrent


## STORAGE TIERS/CLASSES

  - **S3 Standard**: durable, immediately available, frequently accessed
  - **S3 - IA**:  ~same, but infrequently accessed - so cheaper, but there's a fee for each retrieve
  - **S3 One Zone - IA**: more cheaper, but only in one AZ
  - **Glacier**: cheapest, but for archived data only, when you can wait hour for retrieve
    - expedited: restored in few minutes
    - standard: restored in 3-5 hours
    - bulk: restored in 5-12 hours


## VERSIONING

Once you've enable versioning for bucket, you can't turn off it, only suspend.


## CROSS-REGION REPLICATION

Creation of replication bucket allows to get only new file added to the original bucket.

To copy all existing files from original bucket use AWS CLI.


## SECURITY

### MANAGING ACCESS

[Access Control](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html)
  
  - Using Bucket Policies and User Policies
  - Managing Access with ACLs
  
### ENCRYPTION

[Protecting Data using Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html)

- During transition data from you to bucket:
  - SSL/TLS
  
- When data already in the bucket:
  - Client side encryption
  - Server side encryption
    - S3 Managed Keys - **SSE-S3**
    - AWS Key Management Services - **SSE-KMS**
    - Customer-Provided keys - **SSE-C**


## S3 TRANSFER ACCELERATION

[S3 Transfer Acceleration Docs](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html)

**S3 Transfer Acceleration** utilities the CloudFront Edge Network to accelerate your uploads to S3.

Instead of uploading directly to your S3 bucket, you can use distinct URL to uload directly to an edge location which will then transfer that file to S3.

You will get a distinct URL to upload to:  [<your_bucket_name>.s3-accelerate.amazonaws.com](https://<your_bucket_name>.s3-accelerate.amazonaws.com)



## S3 STATIC WEBSITE HOSTING

Address looks like: [http://<your_bucket_name>.s3-website.eu-central-1.amazonaws.com](http://<your_bucket_name>.s3-website.eu-central-1.amazonaws.com)























