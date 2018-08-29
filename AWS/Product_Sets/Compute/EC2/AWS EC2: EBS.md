# AWS EC2: EBS


## OVERALL

**Elastic Block Storage (EBS)** allows you to create storage volumes and attach them to EC2 instances. Once attached, you can create a file system on top of those volumes, run a DB, or use them in any other way you would use a block device. EBS volumes are placed in a specific AZ, where they are automatically replicated to protect you from the failure of the single component.

Think of **EC2** as Virtual Machine in the cloud, and **EBS** as its Virtual Disk.


## EBS VOLUMES TYPES

- **SSD**:
  - **General Purpoce SSD (GP2)** - up to 10,000 IOPS - General purpose, balanced price and performance
  - **Provisioned IOPSSSD (I01)** - 10,000-20,000 IOPS - Large RDBMS, NoSQL DB - highest performance for mission-critical low-latency or high-throughput workloads 
  
- **Magnetic**:
  - **Throughtput Optimized HDD (ST1)** - low cost, frequently accessed, troughput-intensive workloads - Big data, data warehouses, log processing - *CAN'T BE A ROOT VOLUME*
  - **Cold HDD (SC1)** - Lowest cost, less frequently accessed workloads - File server, *CAN'T BE A ROOT VOLUME*
  - **Magnetic (Standard)** - previous generation - data is accessed infrequently, lowest storage cost is important - *IS BOOTABLE* (Test/Dev envs, probably)












































