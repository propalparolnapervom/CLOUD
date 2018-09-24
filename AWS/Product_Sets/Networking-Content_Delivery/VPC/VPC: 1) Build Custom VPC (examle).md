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

[customVPC_step1.JPG]





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

> Note: we've defined subnet for 256 adddresses, only 251 are available. That's because 5 addresses (first 4 and 1 last one) are always reserved by AWS. See Overall section.


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

[customVPC_step2.JPG]
























































