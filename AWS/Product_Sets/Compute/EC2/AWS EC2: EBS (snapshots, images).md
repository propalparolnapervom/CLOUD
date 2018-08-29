# AWS EC2: EBS (SNAPSHOTS, IMAGES)


## OVERALL

**Elastic Block Storage (EBS)** allows you to create storage volumes and attach them to EC2 instances. Once attached, you can create a file system on top of those volumes, run a DB, or use them in any other way you would use a block device. EBS volumes are placed in a specific AZ, where they are automatically replicated to protect you from the failure of the single component.

Think of **EC2** as Virtual Machine in the cloud, and **EBS** as its Virtual Disk.

**Snapshots** usually used for backups.

**Images** usually used to boot EC2 images from it.

## EBS VOLUMES TYPES

- **SSD**:
  - **General Purpoce SSD (GP2)** - up to 10,000 IOPS - General purpose, balanced price and performance
  - **Provisioned IOPSSSD (I01)** - 10,000-20,000 IOPS - Large RDBMS, NoSQL DB - highest performance for mission-critical low-latency or high-throughput workloads 
  
- **Magnetic**:
  - **Throughtput Optimized HDD (ST1)** - low cost, frequently accessed, troughput-intensive workloads - Big data, data warehouses, log processing - *CAN'T BE A ROOT VOLUME*
  - **Cold HDD (SC1)** - Lowest cost, less frequently accessed workloads - File server, *CAN'T BE A ROOT VOLUME*
  - **Magnetic (Standard)** - previous generation - data is accessed infrequently, lowest storage cost is important - *IS BOOTABLE* (Test/Dev envs, probably)



## EC2, EBS AND AZ


EC2 instance and EBS has to be in the same AZ (think about computer architecture: you don't want to have you HDD in 50 miles as latency will be way to big).

To move EBS from one AZ to another:
  - create a snapshot of the EBS, which is in the old AZ;
  - create a new EBS from the snapshot, in necessary (new) AZ;
  
To move EC2 instance from one Region to another:
  - create a snapshot of the EBS, currently used by the EC2 instance;
  - copy snapshot to a new Region;
  - create an image of the copied snapshot;
  - boot it as new EC2 instance;
  
  

## EBS MODIFYING

You can modify (change) any storage, except Standart Magnetic one.

It can be done without downtime (though some performance issues might appear).








































