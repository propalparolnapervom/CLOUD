# VPC: OVERALL


## OVERALL

**Virtual Private Cloud (VPC)** lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resource in a virtual network that you define.

Think of a **VPC** as a virtual Data Center in the cloud.

You have complete contol over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

You can easily customize the network configuration for your VPC. 

> For example, you can create a public-facing subnet for your webservers that has access to the Internet, and place your backend systems (DB/APP servers, etc) in a private-facing subnets with no Internet access. You can leverage multiply layers of security, including security groups and network access control lists, to help control access to EC2 instances in each subnet.

Additionally, you can create a Hardware **Virtual Private Network (VPN)** connection between your corporate datacenter and your VPC and leverage the AWS Cloud as an extention of your corporate datacenter.



![VPC](https://github.com/propalparolnapervom/OVERALL/blob/master/Pictures/VPC_DIAGRAM.JPG "The VPC diagram")


Every Region in the world has default VPC. You get that setup when you first setup your AWS account. Amazon provided it for you automatically.














































