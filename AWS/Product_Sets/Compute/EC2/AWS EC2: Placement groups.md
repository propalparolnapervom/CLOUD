# AWS EC2: PLACEMENT GROUPS


## OVERALL

The name you specify for a Placement Group has to be unique within your AWS account (includes both Clustered/Spread Placement Groups).

AWS recommends homogenous instances within placement groups (instances size and the family should be the same).

You CAN'T merge placement groups.

You CAN'T move an existing instance to a Placement Group. You CAN create an AMI from your existing instance and then launch a new instance from the AMI into a Placement Group.



## TYPES

### Clustered (Classic)

**Clustered Placement Group** is a grouping of instances within a single AZ. 

The oldest one.

Recommended for APP that need:
  - low network latency;
  - hight network throughput;
  - both.
  
Only certain instances can be launched into a Clustered Placement Groups (no t2micro, for instance - more like Compute Optimized, GPU, Memory Optimized, Storage Optimized, etc).


### Spread

**Spread Placement Group** is a group of instances that are each placed on distinct underlying hardware.

So its opposite to Clustered Placement Group: EC2 instances are spread across multiply devices and AZ.

Launched in 2017.

Recommended for APP that:
  - have a small number of critical instances that should be kept separate from each other.




























