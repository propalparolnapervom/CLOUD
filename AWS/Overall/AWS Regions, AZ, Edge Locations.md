# AWS REGIONS, AZ, EDGE LOCATIONS

[Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

[Official Docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)

## OVERALL

Amazon EC2 is hosted in multiple locations world-wide. 

These locations are composed of regions and Availability Zones. 

Each **region** is a separate geographic area. 

Each region has multiple, isolated locations known as **Availability Zones**. 

Amazon EC2 provides you the ability to place resources, such as instances, and data in multiple locations. 

Resources aren't replicated across regions unless you do so specifically.


____________________________________________


So,

An **Amazon AWS region** is a physical location spread across globe to host your data to reduce latency. In each region there will be at least 2 AZ for fault tolerance.

An **Availability Zone (AZ)** is a combination of one or more data centers in a given region. These datacenters need not to be separated by multiple kilometers physically but by meters with in a physical compound which are completely isolated from each other failure such as power, network in a given AZ.  It is a logical grouping of data centers in a given region for service high availability. These AZs in a region are connected with direct Fiber optic links which have capacity of around 25Tbps bandwidth and a latency of 2ms to 1ms. 

A **datacenter** is a location where actual physical data resides. A data center typically have 50000 to 80000 physical servers. A single or couple of data centers are clubbed in to one AZ. Its really hard to get how many datacenters Amazon operates. 

An **edge location** is where end users access services located at AWS. They are located in most of the major cities around the world and are specifically used by *CloudFront (CDN)* to distribute content to end user to reduce latency. It is like frontend for the service we access which are located in AWS cloud. Below are some of the Edge locations as of this writing.

____________________________________________

To deliver content to end users with lower latency, *Amazon CloudFront* uses a global network of 134 Points of Presence (123 **Edge Locations** and 11 **Regional Edge Caches**) in 61 cities across 28 countries.

____________________________________________

Each region is completely independent. 

Each Availability Zone is isolated, but the Availability Zones in a region are connected through low-latency links. 

____________________________________________
2018:
  - 18 geographic regions;
  - 55 AZ in them (12 more to add);
  - 1 local region;

____________________________________________

The following table lists the regions provided by an AWS account. You can't describe or access additional regions from an AWS account, such as AWS GovCloud (US) or China (Beijing).

Code	      Name

us-east-1     US East (N. Virginia)

us-east-2     US East (Ohio)

us-west-1     US West (N. California)

us-west-2     US West (Oregon)

ca-central-1      Canada (Central)

eu-central-1      EU (Frankfurt)

eu-west-1     EU (Ireland)

eu-west-2     EU (London)

eu-west-3     EU (Paris)

ap-northeast-1      Asia Pacific (Tokyo)

ap-northeast-2      Asia Pacific (Seoul)

ap-northeast-3      Asia Pacific (Osaka-Local)

ap-southeast-1      Asia Pacific (Singapore)

ap-southeast-2      Asia Pacific (Sydney)

ap-south-1       Asia Pacific (Mumbai)

sa-east-1  South America (São Paulo)





































