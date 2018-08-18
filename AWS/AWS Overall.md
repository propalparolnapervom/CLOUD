# AWS OVERALL

## USEFULL LINKS

[AWS Udemy Course](https://www.udemy.com/aws-certified-solutions-architect-associate/)


## CLOUD COMPUTING

In 2006, **Amazon Web Services (AWS)** started to offer IT services to the market in the form of web services, which is nowadays known as **cloud computing**.

**Cloud computing** is an internet-based computing service in which large groups of remote servers are networked to allow centralized data storage, and online access to computer services or resources. Using cloud computing, organizations can use shared computing and storage resources rather than building, operating, and improving infrastructure on their own.

There are three types of clouds:

  - **Public Cloud**: The third-party service providers make resources and services available to their customers via Internet. Customer’s data and related security is with the service providers’ owned infrastructure.

  - **Private Cloud**: Also provides almost similar features as public cloud, but the data and services are managed by the organization or by the third party only for the customer’s organization. In this type of cloud, major control is over the infrastructure so security related issues are minimized.

  - **Hybrid Cloud**: A hybrid cloud is the combination of both private and public cloud. The decision to run on private or public cloud usually depends on various parameters like sensitivity of data and applications, industry certifications and required standards, regulations, etc.


There are three types of service models in cloud:

  - **IaaS**: Infrastructure as a Service. It provides users with the capability to provision processing, storage, and network connectivity on demand. Using this service model, the customers can develop their own applications on these resources.

  - **PaaS**: Platform as a Service. Here, the service provider provides various services like databases, queues, workflow engines, e-mails, etc. to their customers. The customer can then use these components for building their own applications. The services, availability of resources and data backup are handled by the service provider that helps the customers to focus more on their application's functionality.

  - **SaaS**: Software as a Service. As the name suggests, here the third-party providers provide end-user applications to their customers with some administrative capability at the application level, such as the ability to create and manage their users. Also some level of customizability is possible such as the customers can use their own corporate logos, colors, etc.


### Advantages of Cloud Computing

**Cost-Efficient** − Building our own servers and tools is time-consuming as well as expensive as we need to order, pay for, install, and configure expensive hardware, long before we need it. However, using cloud computing, we only pay for the amount we use and when we use the computing resources. In this manner, cloud computing is cost efficient.

**Reliability** − A cloud computing platform provides much more managed, reliable and consistent service than an in-house IT infrastructure. It guarantees 24x7 and 365 days of service. If any of the server fails, then hosted applications and services can easily be transited to any of the available servers.

**Unlimited Storage** − Cloud computing provides almost unlimited storage capacity, i.e., we need not worry about running out of storage space or increasing our current storage space availability. We can access as much or as little as we need.

**Backup & Recovery** − Storing data in the cloud, backing it up and restoring the same is relatively easier than storing it on a physical device. The cloud service providers also have enough technology to recover our data, so there is the convenience of recovering our data anytime.

**Easy Access to Information** − Once you register yourself in cloud, you can access your account from anywhere in the world provided there is internet connection at that point. There are various storage and security facilities that vary with the account type chosen.



### Disadvantages of Cloud Computing


**Security issues**

Security is the major issue in cloud computing. The cloud service providers implement the best security standards and industry certifications, however, storing data and important files on external service providers always bears a risk.

AWS cloud infrastructure is designed to be the most flexible and secured cloud network. It provides scalable and highly reliable platform that enables customers to deploy applications and data quickly and securely.

**Technical issues**

As cloud service providers offer services to number of clients each day, sometimes the system can have some serious issues leading to business processes temporarily being suspended. Additionally, if the internet connection is offline then we will not be able to access any of the applications, server, or data from the cloud.

**Not easy to switch service providers**

Cloud service providers promises vendors that the cloud will be flexible to use and integrate, however switching cloud services is not easy. Most organizations may find it difficult to host and integrate current cloud applications on another platform. Interoperability and support issues may arise such as applications developed on Linux platform may not work properly on Microsoft Development Framework (.Net).



## DOMAINS IN WHICH AWS OFFER SERVICES

### Compute

It is used to process data on the cloud by making use of powerful processors which serve multiple instances at a time.

**EC2**

It is a web service which provides re-sizable compute capacity in the cloud. It is designed to make the web scale computing easier for developers. 

**Elastic Beanstalk**

Lets you quickly deploy and manage applications in AWS without worrying about the underlying infrastructure. 

**Elastic Load Balancing**

ELB automatically manages the workload on your instances and distributes them to other instances in case of an instance failure.

**Lambda**

Is used to execute backend code without worrying about the underlying architecture, you just upload the code and it runs, it’s that simple! 





### Storage and Content Delivery

The storage as the name suggests, is used to store data in the cloud, this data can be stored anywhere but content delivery on the other hand is used to cache data nearer to the user so as to provide low latency.

**S3**

S3 stands for simple storage service, it is used for storing data in the form of objects in the AWS Cloud.

**CloudFront**

Is a Content Delivery Network (CDN) which is used to cache data to an edge location which reduces latency.

**Glacier**

Glacier is an archiving service offered by Amazon, which offers low cost data archiving.

**Snowball**

It offers physical transfer of data between user’s location and AWS data centers, the device which is used to transfer the data is called Snowball.


_______________________________________


### Database

The database domain is used to provide reliable relational and non relational database instances managed by AWS.

**RDS**

Amazon RDS is a RDBMS which does routine database tasks in 6 familiar databases like:
  - Amazon Aurora
  - MySQL
  - MariaDB
  - Oracle
  - MS SQL
  - PostgreSQL

**DynamoDB**

It is a fully managed No-SQL database service. It is known for extremely low latencies and scalability.


_______________________________________

### Networking

It includes services which provide a variety of networking features such as security, faster access etc.

**VPC**

Amazon VPC lets you launch AWS resources in a virtual network that you define. It closely resembles a traditional network that you’d operate in your data center.


**Route 53**

Route 53 is a highly scalable and highly available Domain Name System by Amazon AWS. The name is in reference to the TCP and UDP’s port 53 where DNS requests are addressed.


**Direct Connect**

It helps you establish a private connection between your premises and AWS, therefore giving better network performance and throughput than an Internet based connection.

_______________________________________

### Management Tools

It includes services which can be used to manage and monitor your AWS instances.

**CloudWatch**

It is a monitoring tool by AWS which is used to keep a track on the AWS resources and the applications you run on Amazon AWS.


**CloudFormation**

It is a service which helps you setup and model your Amazon AWS resources so that you can spend less time managing these resources and more time focusing on the development.


**CloudTrail**

AWS CloudTrail is a logging service which records the API calls to your Amazon AWS account and delivers them to you.


**Command Line Tool**

It is an all in one tool to manage all your AWS services, by downloading and configuring only one tool you can manage all the AWS services through the command line.

**OpsWorks**

It is a configuration management tool that helps configure and operate applications of all size and shapes using Chef.

**Trusted Advisor**

Trusted Advisor is a customized cloud monitoring tool, that analyzes your AWS environment and gives insights on the expense, performance improvement, security gaps and reliability.


_______________________________________


### Security and Identity

It includes services for user authentication or limiting access to a certain set of audience on your AWS resources.


**Identity and Access Management(IAM)**

It is an AWS service that helps you control access to your AWS resources for your users.


**Key Management Service**

It is a managed service that helps you create and control encryption keys which is used to encrypt your data, and uses Hardware Security Modules to protect the security of your keys.


_______________________________________


### Application Services

It includes simple services like notifications, emailing and queuing.



**SES**

It is a cost effective emailing service which is built on the scalable and reliable infrastructure of Amazon.com


**SNS**

It is a web service offered by AWS that manages the delivery of messages to subscribed endpoints or clients.


**SQS**

It is a fast, reliable and scalable message queuing service, it can be used to transmit any volume of data at any level of throughput, without losing any messages or without the use of any other service.



























