# AWS EC2: EBS


## EBS VOLUMES TYPES

- **SSD**:
  - **General Purpoce SSD (GP2)** - up to 10,000 IOPS - General purpose, balanced price and performance
  - **Provisioned IOPSSSD (I01)** - 10,000-20,000 IOPS - Large RDBMS, NoSQL DB - highest performance for mission-critical low-latency or high-throughput workloads 
  
- **Magnetic**:
  - **Throughtput Optimized HDD (ST1)** - low const, frequently accessed, troughput-intensive workloads - Big data, data warehouses, log processing - *CAN'T BE A ROOT VOLUME*
  - **Cold HDD (SC1)** - Lowest cost, less frequently accessed workloads - File server, *CAN'T BE A ROOT VOLUME*
  - **Magnetic (Standard)** - previous generation - data is accessed infrequently, lowest storage cost is important - *IS BOOTABLE* (Test/Dev envs, probably)












































