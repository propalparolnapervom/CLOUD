# VPC: NAT

[Docs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat.html)

## OVERALL

> NOTE: NAT Gateways have to be used instead of NAT Instances whenever it's possible.

You can use a NAT device to enable instances in a private subnet to connect to the Internet (for example, for software updates) or other AWS services, but prevent the Internet from initiating connections with the instances. 

A NAT device forwards traffic from the instances in the private subnet to the Internet or other AWS services, and then sends the response back to the instances. 

When traffic goes to the Internet, the source IPv4 address is replaced with the NAT device’s address and similarly, when the response traffic goes to those instances, the NAT device translates the address back to those instances’ private IPv4 addresses.

NAT devices are not supported for IPv6 traffic—use an egress-only Internet gateway instead. For more information, see Egress-Only Internet Gateways.

__________________

AWS offers two kinds of NAT devices:
  - a NAT gateway;
  - NAT instance. 
  
We recommend NAT gateways, as they provide better availability and bandwidth over NAT instances. The NAT Gateway service is also a managed service that does not require your administration efforts. A NAT instance is launched from a NAT AMI. You can choose to use a NAT instance for special purposes.



## NAT INSTANCES

You can use a network address translation (NAT) instance in a public subnet in your VPC to enable instances in the private subnet to initiate outbound IPv4 traffic to the Internet or other AWS services, but prevent the instances from receiving inbound traffic initiated by someone on the Internet.

NAT gateways are not supported for IPv6 traffic—use an egress-only internet gateway instead. For more information, see [Egress-Only Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html).

  - When creating a NAT instance, disable Source/Dest. Check on the Instance;
  - NAT Instances must be in a public subnet;
  - There must be a route out of the private subnet to the NAT instance, in order for this to work;
  - The amount of traffic that NAT instances can support depends on the instance size. If your are bottlenecking, increase the instance size;
  - You can create a highavailability using:
    - ASG;
    - multiply subnets in different AZ;
    - a script to automate failover;
    - NAT instances are behind a Security Group;
    
    
## NAT GATEWAYS

[Docs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)

You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating a connection with those instances. 

You are charged for creating and using a NAT gateway in your account. NAT gateway hourly usage and data processing rates apply. Amazon EC2 charges for data transfer also apply. For more information, see [Amazon VPC Pricing](http://aws.amazon.com/vpc/pricing/).

NAT gateways are not supported for IPv6 traffic—use an egress-only internet gateway instead. For more information, see [Egress-Only Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html).

  - preffered by the enterprise;
  - Scale automatically up to 10Gbps
  - no need to patch
  - not associated with security groups;
  - automatically assigned a public ip address;
  - remember to update your route table;
  - for redanduncy you want to have multiply NAT Gateways in different subnets;
  - no need to disable Source/Dest Checks;
  - more secure than a NAT instance:
    - you don't have a SSH to your NAT Gateway;
    - you don't have to worry about OS patching;
    - you don't have to worry about antivirus;
  
  
  

## NAT INSTANCES vs NAT GATEWAYS

[Comparison](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)
































