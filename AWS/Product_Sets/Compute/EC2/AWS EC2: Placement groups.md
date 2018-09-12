# AWS EC2: PLACEMENT GROUPS

[Official Docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html)

## OVERALL

You can launch or start instances in a **Placement Group**, which determines how instances are placed on underlying hardware.

There is no charge for creating a placement group.

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

If you stop an instance in a placement group and then start it again, it still runs in the placement group. However, the start fails if there isn't enough capacity for the instance.

If you receive a capacity error when launching an instance in a placement group that already has running instances, stop and start all of the instances in the placement group, and try the launch again. Restarting the instances may migrate them to hardware that has capacity for all the requested instances.



### Spread

**Spread Placement Group** is a group of instances that are each placed on distinct underlying hardware.

So its opposite to Clustered Placement Group: EC2 instances are spread across multiply devices and AZ.

Launched in 2017.

Recommended for APP that:
  - have a small number of critical instances that should be kept separate from each other.

A spread placement group can span multiple AZ, and you can have a maximum of 7 running instances per AZ per group.

If you start or launch an instance in a spread placement group and there is insufficient unique hardware to fulfill the request, the request fails. Amazon EC2 makes more distinct hardware available over time, so you can try your request again later. 


## RULES AND LIMITATIONS

Before you use placement groups, be aware of the following rules:
  - The name you specify for a placement group must be unique within your AWS account for the region.
  - You can't merge placement groups.
  - An instance can be launched in one placement group at a time; it cannot span multiple placement groups.
  - Reserved Instances provide a capacity reservation for EC2 instances in a specific Availability Zone. The capacity reservation can be used by instances in a placement group. However, it is not possible to explicitly reserve capacity for a placement group.
  - Instances with a tenancy of host cannot be launched in placement groups.
























