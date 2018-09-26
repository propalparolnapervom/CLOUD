# VPC: VPC END POINTS

## OVERALL

A **VPC endpoint** allows you to securely connect your VPC to another service.

An interface endpoint is powered by [PrivateLink](https://docs.aws.amazon.com/console/vpc/endpoints/privatelink), and uses an elastic network interface (ENI) as an entry point for traffic destined to the service.

A gateway endpoint serves as a target for a route in your route table for traffic destined for the service.

_____________

If you have an EC2 instance, which is in some Private Subnet, it is behind NAT Gateway to access Internet. So when you want to reach S3, for example, it has to go out of Private Subnet, via NAT Gateway, to the Internet.

VPC Endpoint allows you to stay into your Private Subnet and reach internal resouces such S3. So it is some kind of internal gateway.



## STEPS

### IAM ROLE TO ACCESS S3

Create a IAM role which allows EC2 to get a full access to S3.

Attach the IAM role to your EC2 instance.


### VPC ENDPOINT

Go to **VPC** service.

Click **Endpoints** link.

Hit **Create Endpoint** button.

```
Service Category: AWS Services
Service Name: 
  ...
  com.amazonaws.eu-central-1.ec2 Interface    <== Just interface that is associated with EC2 instance and serves as entry point           
  com.amazonaws.eu-central-1.s3 Gateway   <== Choose this. Gateways are like NAT Gateway, so its highly available
  ...
VPC: ...xbsVPC...
Configure route tables: check the main one (with no inet access)
Policy: Full access
```

Hit **Create Endpoint** button.

Hit **Close** button.


Your Main Route Table (the one without Inet access) now has additional line.

S3 is now accessible from EC2 instance in your Private Subnet, without Internet.




























