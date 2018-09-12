# AWS EFS

## OVERALL

**Elastic File System (EFS)** is a file storage service for EC2 instances.

EFS is easy to use and provides a simple interface that allows you to create and configure FS quickly and easily.

With EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files, so your APP have the storage they need, when they needed.



## EFS FEATURES

  - Supports the Network File System version 4 (NFSv4) protocol;
  - You only pay for the storage you use (no pre-provision required);
  - Can scale up to the petabytes;
  - Can support thousands of concurrent NFS connections;
  - Data is sored across multiply AZ's withing a region;
 
  
  - **EFS vs S3**:
    - it's block-based storage (not like S3 object-based storage) - we can put files in it and share them with other EC2 instances;
    - Read After Write Consistency (like S3);
  











































