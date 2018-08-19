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



## Data consistency model

  - Read after Write consistency for PUTS of new object (you initially upload your file - you can see it in milliseconds)
  - Eventual Consistency for overwrite PUTS and DELETES (if you change already uploaded file, it can take some time to see changes - as files can be spread among several AZ it can take some time to change all of them)


## Key-value store

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






































