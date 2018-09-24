# VPC: BUILD CUSTOM VPC (EXAMPLE)

**What**

Build your own Custom VPC
  - Manually (so no Wizards);
  - With 2 Subnets:
    - 1 public;
    - 1 private;




## STEPS


### CREATING VPC

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






### CREATING SUBNETS

Create 2 subnets in different AZ.

> Remember: 1 subnet = 1 AZ

#### SUBNET 1

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


#### SUBNET 2

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



### CREATING INTERNET GATEWAY

#### CREATE IGW

Create an Internet Gateway for VPC, so 1 of 2 already created Subnets became eventually public (got Internet connectivity).

Click **Internet Gateways** link.

Hit **Create internet gateway** button.
```
Name tag: xbsIGW
```

Hit **Create** button.

Hit **Close** button.


#### ATTACH IGW

Just created Internet Gateway is Detached by default. So it's need to be attached.

Check just created IGW.

**Attach to VPC** from **Actions** drop-down list.
```
VPC: ...xbsVPC...   <== choose your VPC
```

Hit **Attach** button.

> Note: You can't have multiply IGW withing one VPC. Just try to attach another one IGW to your VPC ...



### ROUTE TABLE

**Route tables** are created by default when you create your VPC. 

After such default creation Route Table became **main** table (Main:yes attribute on Summary tab).

All subnets inplicitly associated with Main Route Table, if no other was specified.

So: 
  - you don't wan't, for example, your Main Route Table has an Intenet access (all new subnets in this case will have Internet access by default);
  - if subnet is not associated with some Route Table explicitly, it will be associated with Main Route Table by default.


#### CREATE ADDITIONAL ROUTE TABLE TO ACCESS INET

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


#### ASSOCIATE A SUBNET WITH INET ROUTE TABLE

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



### GIVE YOUR PUBLIC SUBNET PUBLIC IP

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



### VERIFICATION: PUBLIC/PRIVATE SUBNETS

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


























