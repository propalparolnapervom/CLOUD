# VPC: BUILD CUSTOM VPC (EXAMPLE)

**What**

Build your own Custom VPC
  - Manually (so no Wizards);
  - With 2 Subnets:
    - 1 public;
    - 1 private;
  - Private subnet still has to have outbound Internet access;



## 1) CREATING VPC

Go to **VPC** service.

Choose necessary Region.

Click **Your VPCs** link.

Hit **Create VPC** button.

```
Name tag: xbsVPC
IPv4 CIDR block:  10.0.0.0/16  <== /16 is the bigest range you can have in AWS
IPv6 CIDR block:  10.0.0.0/16 radiobutton is selected
Tenancy: Default  <== if you choose Dedicate (so no other AWS account will share the hardware with you),
                      it will be more expensive
```

Hit **Yes, Create** button.

Following (default) things created automatically after that:
  - Route Tables;
  - Network ACLs;
  - Security Groups.

Following things you have to create manually:
  - Subnets;
  - Internet Gateway.
  

All you've created so far:

![This is Inline style](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/customVPC_step1.JPG "You have for now")






## 2) CREATING SUBNETS

Create 2 subnets in different AZ.

> Remember: 1 subnet = 1 AZ

### 2.1) SUBNET 1

Click **Subnets** link.

Make sure you're in the necessary Region.

Hit **Create subnet** button.
```
Name tag: 10.0.1.0-eu-central-1a    <== any random name
VPC:  ...xbsVPC...                  <== for previously created VPC
Availability Zone:  eu-central-1a   <== for example, I'd like to create a subnet for this AZ
IPv4 CIDR block: 10.0.1.0/24        <== for example, I want my subnet to have 256 addresses
```

Hit **Create** button.

Hit **Close** button.

> Note: we've defined subnet for 256 addresses, but only 251 are available. That's because 5 addresses (first 4 and 1 last one) are always reserved by AWS. See Overall section.


### 2.2) SUBNET 2

Click **Subnets** link.

Make sure you're in the necessary Region.

Hit **Create subnet** button.
```
Name tag: 10.0.2.0-eu-central-1b    <== any random name
VPC:  ...xbsVPC...                  <== for previously created VPC
Availability Zone:  eu-central-1b   <== for example, I'd like to create a subnet for this (different from previous) AZ
IPv4 CIDR block: 10.0.2.0/24        <== for example, I want my subnet to have 256 addresses
```

Hit **Create** button.

Hit **Close** button.


So following is created by now:

![This is Inline style](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/customVPC_step2.JPG "You have for now")


So we've got 2 subnets now:
  - there's nothing inside them;
  - they are both private now.



## 3) CREATING INTERNET GATEWAY

### 3.1) CREATE IGW

Create an Internet Gateway for VPC, so 1 of 2 already created Subnets became eventually public (got Internet connectivity).

Click **Internet Gateways** link.

Hit **Create internet gateway** button.
```
Name tag: xbsIGW
```

Hit **Create** button.

Hit **Close** button.


### 3.2) ATTACH IGW

Just created Internet Gateway is Detached by default. So it's need to be attached.

Check just created IGW.

**Attach to VPC** from **Actions** drop-down list.
```
VPC: ...xbsVPC...   <== choose your VPC
```

Hit **Attach** button.

> Note: You can't have multiply IGW withing one VPC. Just try to attach another one IGW to your VPC ...



## 4) ROUTE TABLE

**Route tables** are created by default when you create your VPC. 

After such default creation Route Table became **main** table (Main:yes attribute on Summary tab).

All subnets inplicitly associated with Main Route Table, if no other was specified.

So: 
  - you don't wan't, for example, your Main Route Table has an Intenet access (all new subnets in this case will have Internet access by default);
  - if subnet is not associated with some Route Table explicitly, it will be associated with Main Route Table by default.


### 4.1) CREATE ADDITIONAL ROUTE TABLE TO ACCESS INET

Because of reasons above, to access Internet let's create a new (separate one) Route Table.


**Create additional Route Table**

Click **Route Tables** link.

Hit **Create Route Table** button.
```
Name tag: xbsInternetRouteOut
VPC: ...xbsVPC...
```

Hit **Yes, Create** button.

> Notice: this is not 'Main' Route Table


**Enable Internet Access for the Route Table**

Check just created `xbsInternetRouteOut` Route Table.

Click **Routes** tab.

Click **Edit** button.

Click **Add another route** button.
```
Destination: 0.0.0.0/0    <== "any IP address"
Target: ...xbsIGW...      <== your Internet Gateway
```

Click **Save** button.


### 4.2) ASSOCIATE A SUBNET WITH INET ROUTE TABLE

So we have:
  - 2 private subnets;
  - 2 Route Tables:
    - default, with no Internet access;
    - just created one, with Internet access.
    
**Make 1 Subnet public**

Check just created `xbsInternetRouteOut` Route Table (the one with Internet access).

Click **Subnet Associations** tab.

Click **Edit** button.

Check necessary subnet (`10.0.1.0-eu-central-1a`, for example).

Click **Save** button.

Make sure the subnet now in the list of explicitly defined subnets, associated with current Route Table `xbsInternetRouteOut`.



## 5) GIVE YOUR PUBLIC SUBNET PUBLIC IP

By default when you create a subnet, it doesn't have ability to get Public IP automatically. You need to specify it excplicitly.

You want to have 1 subnet publically available, so it has to get Public IP automatically.

**Give your public subnet ability to get Public IP**

Click **Subnets** link.

Check your eventually public subnet (`10.0.1.0-eu-central-1a`).

Click **Actions** drop-down list, click **Modify auto-assign IP settings** option.
```
Auto-assign IPv4: Enable auto-assign public IPv4 address is checked
```

Click **Save** button.



## 6) VERIFICATION: PUBLIC/PRIVATE SUBNETS

Create EC2 instances.

**In the public subnet**
```
...
Network: ...xbsVPC...
Subnet: ...10.0.1.0-eu-central-1a...
Auto-assign Public IP: Use subnet setting (Enable)
...
Name: PublicServer
...
Create a new security group   <== security groups are available only within VPC, so no security groups in the new VPC
  HTTP 80
  HTTPS 443
  SSH 22
```

**In the private subnet**
```
...
Network: ...xbsVPC...
Subnet: ...10.0.2.0-eu-central-1b...
Auto-assign Public IP: Use subnet setting (Disable)
...
Name: PrivateServer
...
Choose a deault security group   <== as you don't wan't it to be opened to the world
```

Check that 1 server is public, another one is not.

__________________

So eventually we have following picture:

![This is Inline style](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/customVPC_step3.JPG "You have for now")

For now this 2 EC2 instances are not allowed to communicate with each other as they are in the different Security Groups.


## 7) MAKE INTERNET AVAILABLE FROM PRIVATE SUBNET 

### 7.1) VERIFY PRIVATE SERVER HAS NO INET

**Create a new Security Group from private subnet**

Let's say, DB will be placed on private subnet. 

So create a new security group `xbsRDS` for private subnet to open necessary ports for `10.0.1.0/24` source (our public subnet):
```
HTTP 80
HTTPS 443
SSH 22
MYSQL/Aurora 3306
All ICMP IPv4 0-65535
```

Place `PrivateServer` into just create `xbsRDS` security group.


**Connect to PrivateServer from PublicServer**

SSH to `PublicServer`.

Ping `PrivateServer` (its private IP).

> NOTE: !!! NEVER PLACE KEYS ON PROD!!! JUST LEARNING AIM !!!

> !!! BASTIONS SHOULD BE USED INSTEAD !!!

Place private key for `PrivateServer` on `PublicServer` to connect to `PrivateServer`.
  - Find private key on your local PC;
  - Copy it;
  - On the `PublicServer` as root user:
```
mkdir ~/dont_do_like_this
cd ~ec2-user/dont_do_like_this
vim kp_frankfurt.pem
chmod 400 kp_frankfurt.pem
```

Connect to `PrivateServer` via its Private IP.
```
cd ~ec2-user/dont_do_like_this
ssh ec2-user@10.0.2.126 -i kp_frankfurt.pem
```

Try to make some updates on `PrivateServer`. You can't. Because you don't have an Internet access.



## 7.2) NAT INSTANCES

> They are old, NAT Gateway might be used instead. Make following stes just for general understanding.


**Launch NAT instance**

Go to **EC2** service.

Hit **Launch instance** button.

Click **Community AMIs** link.

Search and choose the one with "NAT" in the name (`amzn-ami-vpc-nat-hvm-2018.03.0.20180508-x86_64-ebs - ami-0097b5eb`, for example).

Launch instance from it:
  - in your VPC;
  - in public subnet;
  - name it `NAT-INSTANCE`;
  - in security group that was used for public subnet.



**Disable Source/Dest Check for NAT instance**

[Docs](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#EIP_Disable_SrcDestCheck)

> Each EC2 instance performs source/destination checks by default. This means that the instance must be the source or destination of any traffic it sends or receives. However, a NAT instance must be able to send and receive traffic when the source or destination is not itself. 

Therefore, you must disable source/destination checks on the NAT instance.

Go to **EC2** service.

Check just created `NAT-INSTANCE` instance.

Click **Actions** drop-down list, **Networking**, then **Change Source/Dest. Check**.

Hit **Yes, Disable** button.



**Update Route Table to see NAT Instance**

Go to **VPC** service.

Click **Route Tables** link.

Check Main Route Table for your VPC (the one without Internet access).

Click **Routes** tab.

Hit **Edit** button, hit **Add another route** button.
```
Destination: 0.0.0.0/0
Target: ...NAT-INSTANCE...   <== check your NAT instance
```

Hit **Save button**.

Now outbound Internet access is available for `PrivatServer`.

___________________________________

> All traffic for Private Subnet goes through 1 NAT instance, so it might be a bottleneck (t2.micro, for example).

> It relies on a single OS on NAT instance, so if it crashes, we're going to lose all internet for Private Subnet.

> Yes, it can be put in Autoscaling Groups, in multiply AZ, but it becomes more complicated.


That's why new NAT Gateway is much better: automation of all that steps, so eventually we're relying on a single instance in the single AZ.

_______________________

Terminate `NAT-INSTANCE`.



### 7.3) NAT GATEWAY

> NOTE: NAT Gateway operates only for IPv4. For IPv6 you have to use Engress Only Internet Gateways


**Create NAT Gateway**

Go to **VPC** service.

Click **NAT Gateways** link.

Hit **Create NAT Gateway** button.

```
Subnet:                       <== chose your public subnet
Elastic IP Allocation ID:     <== Hit "Create New EIP" for now
```

Hit **Create a NAT Gateway** button.

Hit **Close** button.

Wait some time until your NAT Gateway will be created (its status changed to **available**).


**Update Route Table to see NAT Gateway**

Go to **VPC** service.

Click **Route Tables** link.

Check Main Route Table for your VPC (the one without Internet access).

Click **Routes** tab.

Hit **Edit** button, hit **Add another route** button.
```
Destination: 0.0.0.0/0
Target: ...nat...         <== chose your NAT Gateway
```

Hit **Save button**.

Now outbound Internet access is available for `PrivatServer`.

How it looks now:


![This is Inline style](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/customVPC_step4.JPG "You have for now")






























































