# VPC: OVERALL


## OVERALL

**Virtual Private Cloud (VPC)** lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resource in a virtual network that you define.

Think of a **VPC** as a virtual Data Center in the cloud.

You have complete contol over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

You can easily customize the network configuration for your VPC. 

> For example, you can create a public-facing subnet for your webservers that has access to the Internet, and place your backend systems (DB/APP servers, etc) in a private-facing subnets with no Internet access. You can leverage multiply layers of security, including security groups and network access control lists, to help control access to EC2 instances in each subnet.

Additionally, you can create a Hardware **Virtual Private Network (VPN)** connection between your corporate datacenter and your VPC and leverage the AWS Cloud as an extention of your corporate datacenter.



![VPC](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/VPC_DIAGRAM.JPG "The VPC diagram")



## VPC vs REGION

Every Region in the world has default VPC. You get that setup when you first setup your AWS account. Amazon provided it for you automatically.

You can have multiply VPC in the Region. By default that limit is 5, but you can make it bigger by e-mailing to AWS.


## IP vs SUBNETS

[Docs](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)

Each **Subnet** - different AZ.

AWS allows you to use following IP ranges for subnets:

  - 10.0.0.0 - 10.255.255.255 (10/8 prefix)
  - 172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
  - 192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
  
Example of the WebSite to count CIDR: [http://cidr.xyz/](http://cidr.xyz/)

You always lose 5 IP addresses from your range (first 4 and last 1). [For example](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#VPC_Sizing), in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:
  - 10.0.0.0: Network address.
  - 10.0.0.1: Reserved by AWS for the VPC router.
  - 10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see [Amazon DNS Server](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#AmazonDNS).
  - 10.0.0.3: Reserved by AWS for future use.
  - 10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.



`/28` is mask for smalest range of IP adresses you can have in AWS.

**Rout table** between subnets define which subnet can talk with which subnet.

> You can have only 1 Internet Gateway for 1 VPC.


## DEFAULT VPC vs CUSTOM VPC

  - Default VPC is user friendly, allowing you to immediately deploy instances;
  - All Subnets in default VPC have a route out to the Internet;
  - Each EC2 instance in default VPC has both a public and private IP address;



## VPC PEERING

If you have several private VPC, they can connect with each other - via peering.

  - allows you to connect one VPC with another via a direct network route using private IP addresses;
  - instances behave as if they were on the same private network;
  - you can peer VPC's with other AWS accounts as well as with other VPCs in the same account;
  - peering is in a star configuration: i.e. 1 central VPC peers with 4 others. NO TRANSITIVE PEERING!!!
    - You have VPC A peered to VPC B and to VPC C. So VPC A can connect with VPC B and with VPC C. In this case VPC B cannot connect with VPC C. Additional peering between them has to be established to be able to communicate.



## CONCLUSION

   - Think of VPC as a logical datacenter in AWS;
   - Consists of:
    - Internet Gateways (or Virtual Private Gateways);
    - Route Tables;
    - Network Access Control Lists;
    - Security Groups;
   - 1 Subnet = 1 AZ;
   - Security Groups are **stateful**; Network Access Control Lists are **stateless** (so in first case opened port 80 for inbound automatically opens port for outpound, in second - not automatically);
   - NO TRANSITIVE PEERING

> Note: only 1 Internet GateWay may be attached to VPC

































