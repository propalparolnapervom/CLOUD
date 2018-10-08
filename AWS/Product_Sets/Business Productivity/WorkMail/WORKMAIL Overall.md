# WORKMAIL OVERALL

[Docs](https://docs.aws.amazon.com/workmail/latest/adminguide/what_is.html)



## OVERALL

**WorkMail** is a secure, managed business email and calendaring service with support for existing desktop and mobile email clients. 

You can access your email, contacts, and calendars using Microsoft Outlook, your browser, or their native iOS and Android email applications. 

You can integrate Amazon WorkMail with your existing corporate directory and control both the keys that encrypt your data and the location in which your data is stored.

_________

You can access Amazon WorkMail using the web application: [https://**alias**.awsapps.com/mail](https://*alias*.awsapps.com/mail).



## CONCEPTS

[Concepts](https://docs.aws.amazon.com/workmail/latest/adminguide/what_is.html#workmail_concepts)

**Organization**

A tenant setup for Amazon WorkMail.



**Alias**

A globally unique name to identify your organization. The alias is used to access the Amazon WorkMail web application (https://**alias**.awsapps.com/mail).



**Domain**

The web address that comes after the @ symbol in an email address. You can add a domain that receives mail and delivers it to mailboxes in your organization.



**Test mail domain**

A domain is automatically configured during setup that can be used for testing Amazon WorkMail. 

The test mail domain is **alias**.awsapps.com and is used as the default domain if you do not configure your own domain. 

The test mail domain is subject to different limits. 

For more information, see [Amazon WorkMail Limits](https://docs.aws.amazon.com/workmail/latest/adminguide/workmail_limits.html).



**Directory**

An AWS Simple AD, AWS Managed AD, or AD Connector created in AWS Directory Service. 

If you create an organization using the Amazon WorkMail Quick setup, we create a WorkMail directory for you. You cannot view a WorkMail directory in AWS Directory Service.



**User**

A user created in the AWS Directory Service. 

When a user is enabled for Amazon WorkMail, they receive their own mailbox to access.

When a user is disabled, they cannot access Amazon WorkMail.



**Group**

A group used in AWS Directory Service. 

A group can be used as a distribution list or a security group in Amazon WorkMail. 

Groups do not have their own mailboxes.



**Resource**

A resource represents a meeting room or equipment resource that can be booked by Amazon WorkMail users.



**Mobile device policy**

Various IT policy rules that control the security features and behavior of a mobile device.



## RELATED AWS SERVICES

[Related Services](https://docs.aws.amazon.com/workmail/latest/adminguide/what_is.html#related_services)




## SUPPORTED AWS REGIONS AND ENDPOINT

[List](https://docs.aws.amazon.com/general/latest/gr/rande.html#wm_region) of supported AWS Regions and endpoints for AWS WorkMail.

__________

P.S.

To reduce data latency in your APP, most Amazon Web Services offer a regional endpoint to make your requests. 

An **endpoint** is a URL that is the entry point for a web service. 

> For example, https://dynamodb.us-west-2.amazonaws.com is an entry point for the Amazon DynamoDB service.

Some services, such as IAM, do not support regions; therefore, their endpoints do not include a region. Some services, such as Amazon EC2, let you specify an endpoint that does not include a specific region, for example, https://ec2.amazonaws.com. In that case, AWS routes the endpoint to us-east-1.

If a service supports regions, the resources in each region are independent. 

> For example, if you create an Amazon EC2 instance or an Amazon SQS queue in one region, the instance or queue is independent from instances or queues in another region.





























