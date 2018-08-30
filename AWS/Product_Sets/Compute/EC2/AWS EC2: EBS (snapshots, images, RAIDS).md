# AWS EC2: EBS (SNAPSHOTS, IMAGES, RAIDS)


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

To move EBS from one AZ/Region to another:
  - create a snapshot/image of the EBS, which is in the old AZ/Region;
  - copy it to the new AZ/Region;
  - create a new EBS from the just copied snapshot/image;
  
To move EC2 instance from one Region to another:
  - create a snapshot of the EBS, currently used by the EC2 instance;
  - copy snapshot to a new Region;
  - create an image of the copied snapshot;
  - boot it as new EC2 instance;
  
  

## EBS MODIFYING

You can modify (change) attributes of storage:
  - size
  - storage type (except for Standart Magnetic)

It can be done without downtime (though some performance issues might appear).


## SNAPSHOTS

**Snapshot** is point in time (copy) of Volume.

Snapshots exist on S3. You don't see it, but it is so.

Snashots are incremental: only the blocks that have changed since your last snapshot are moved to S3.

If you do want to make a snapshot for root Volume, you should prior stop the instance (hovewer, technically it can be done while instance is running).

Snapshots of encrypted volumes are encrypted automatically. Volumes restored from encrypted snapshots are also encrypted automatically.

You can share snapshots, but only if they are not encrypted: encryption has strong connection with AWS accounts.


## RAIDS

If you reached volume IO limit and you still need more, you can add more Volumes and put them in the Raids


**RAID** - Redundant Array of Independent Disks.
  - **RAID 0** - Stripped, No Redundency, Good Performance;     **<== Typical for AWS**
  - **RAID 1** - Mirrored, Redundency;
  - **RAID 5** - Good for reads, Bad for writes;     **<== AWS doesn't recommend ever putting RAID 5 on EBS**
  - **RAID 10** - 1+0 - Stripped & Mirrored, Good Redundency, Good Performance.     **<== Typical for AWS**




## RAIDS AND SNAPSHOT

**Problem**

A snapshot excludes data held in the cache by APP and OS.

It doesn't matter on a single Volume, but using multiple Volumes in a RAID array, this could be a problem due to interdependencies of the array. 

**Solution**

Take an APP consistent snapshot.


**Taking APP consistent snapshot**

What we need to do:
  - Stop the APP from writing to the disk
  - Flush all caches to the disk
  
How we can do it - use one of the following:
  - Freeze the FS;
  - Unmount the RAID array;
  - Shutting down the associated EC2 instance and take a snapshot   <== the easiest way, that most people do




## ROOT DEVICE TYPE


Storage for the root device (Root Device Volume):
  - **Instance Store (EPHEMERAL STORAGE)**
    - you CAN'T stop the Instance;
      - if underlaying host fails, you will lose your data (That's why it called EPHEMERAL STORAGE);
    - you CAN'T detach the Volume;
    - you CAN reboot, no data lost;
    - root volume will be deleted on termination by default, you CAN'T tell AWS to keep it;
  - **EBS Backed Volumes**
    - you CAN stop the Instance;
      - if underlaying host stops, you will not lose your data;
    - you CAN detach the Volume (and potentially to attach it another Instance);
    - you CAN reboot, no data lost;
    - root volume will be deleted on termination by default, you CAN tell AWS to keep it;

__________________________


**EBS Backed Volumes** appeared after **Instance Storage**, as AWS was critisized for inconsistance of data;
__________________________

**Instance Store Volume**

The *root device for the Instance* launched from the AMI is an *Instance Store Volume* created from a *template stored in Amazon S3*.

**EBS Volumes**

The *root device for the Instance* launched from the AMI is an *Amazon EBS volume* created from an *Amazon EBS snapshot*.

For this difference provision of the *EBS Volumes* is faster.














































