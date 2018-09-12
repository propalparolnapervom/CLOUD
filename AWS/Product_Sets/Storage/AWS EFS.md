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
 
  
## EFS vs S3

    - it's block-based storage (not like S3 object-based storage) - we can put files in it and share them with other EC2 instances;
    - Read After Write Consistency (like S3);

## EFS vs EBS

  - EBS might be connected to only single EC2 instance;
  - EFS might be shared among several EC2 intances;
  
So EFS has ability to be a File Server - some kind of Central File Repository.

  

## MOUNT TARGET

An Amazon EFS file system is accessed by EC2 instances running inside one of your VPCs. 

Instances connect to a FS by using a network interface called a **mount target**. 

Each mount target has an IP address, which we assign automatically or you can specify.

We recommend creating a mount target in each of your VPC's AZ so that EC2 instances across your VPC can access the FS.

_______________________________________________

To access an Amazon EFS file system in a VPC, you need **mount targets**. For an Amazon EFS file system, the following is true:

  - You can create one mount target in each Availability Zone.

  - If the VPC has multiple subnets in an Availability Zone, you can create a mount target in only one of those subnets. All EC2 instances in the Availability Zone can share the single mount target.

> Note:
> We recommend that you create a mount target in each of the Availability Zones. There are cost considerations for mounting a file system on an EC2 instance in an Availability Zone through a mount target created in another Availability Zone. For more information, see Amazon EFS. In addition, by always using a mount target local to the instance's Availability Zone, you eliminate a partial failure scenario. If the mount target's zone goes down, you can't access your file system through that mount target.


## MAKE A MOUNTING

Mount already existing EFS to your `/var/www/html` dir.

> EC2 instances should be in the same Security Groups as EFS


EFS service -> Choose necessary FS -> "Amazon EC2 mount instructions" link -> Do steps described there.


Mount FS on all necessary EC2 instances:
```
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-f10bd8a8.efs.eu-central-1.amazonaws.com:/ /var/www/html
```

On one of your EC2 instance, for instance:
```
vim /var/www/html/index.html

  <html><h1> XBURSER: Test is successfull!!! </h1></html>
```

See just created file on all other EC2 instances
```
ls -la /var/www/html
```


































