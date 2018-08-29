# AWS EC2 OVERALL

## OVERALL


**Elastic Compute Cloud (EC2)** - web service that provides resizable compute capacity in the cloud.

**Elastic Block Storage (EBS)** allows you to create storage volumes and attach them to EC2 instances. Once attached, you can create a file system on top of those volumes, run a DB, or use them in any other way you would use a block device. EBS volumes are placed in a specific AZ, where they are automatically replicated to protect you from the failure of the single component.

Think of **EC2** as Virtual Machine in the cloud, and **EBS** as its Virtual Disk.

**Security Group** - think of it as Virtual Firewall.

## CONNECT

### LINUX

```
ssh ec2-user@<public_ip> -i <you_private_key.pem>
```

### WINDOWS


### PUBLIC DNS

An example of the EC2 instance public DNS:

[http://ec2-52-59-237-102.eu-central-1.compute.amazonaws.com](http://ec2-52-59-237-102.eu-central-1.compute.amazonaws.com)


## EC2 PRICING

**1) On-Demand**

You pay for compute capacity by per hour or per second depending on which instances you run. You can increase or decrease your compute capacity depending on the demands of your application and only pay the specified per hourly rates for the instance you use.

Recommended for:

  - Users that prefer the low cost and flexibility of Amazon EC2 without any up-front payment or long-term commitment
  - Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted
  - Applications being developed or tested on Amazon EC2 for the first time

**2) Spot Instances**

Allow you to request spare Amazon EC2 computing capacity for up to 90% off the On-Demand price. 

Recommended for:

  - Applications that have flexible start and end times
  - Applications that are only feasible at very low compute prices
  - Users with urgent computing needs for large amounts of additional capacity

**3) Reserved Instances**

Reserved Instances provide you with a significant discount (up to 75%) compared to On-Demand instance pricing. 

In addition, when Reserved Instances are assigned to a specific Availability Zone, they provide a capacity reservation, giving you additional confidence in your ability to launch instances when you need them.

For applications that have steady state or predictable usage, Reserved Instances can provide significant savings compared to using On-Demand instances. See How to Purchase Reserved Instances for more information.

Recommended for:

  - Applications with steady state usage
  - Applications that may require reserved capacity
  - Customers that can commit to using EC2 over a 1 or 3 year term to reduce their total computing costs

**4) Dedicated Hosts**

A Dedicated Host is a physical EC2 server dedicated for your use. 

Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses, including Windows Server, SQL Server, and SUSE Linux Enterprise Server (subject to your license terms), and can also help you meet compliance requirements. Learn more.

  - Can be purchased On-Demand (hourly).
  - Can be purchased as a Reservation for up to 70% off the On-Demand price.


## EC2 TYPES (2018)

F
I
G
H
T

D
R

M
C
P
X
























































































