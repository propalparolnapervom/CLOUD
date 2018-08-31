# AWS EC2: ROLES

[IAM Roles for EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html?icmpid=docs_ec2_console)

## OVERALL

Storage of AWS credentials locally on the EC2 instance can be copromised. They have to be updated then. What if you have 1000 EC2 instances - do you want to updated local credentials on each of those servers? That's where Roles can help.

**IAM Role** - secure way to grant permissions to entities that you trust. 

Examples of entities include the following:

  - IAM user in another account
  - Application code running on an EC2 instance that needs to perform actions on AWS resources
  - An AWS service that needs to act on resources in your account to provide its features
  - Users from a corporate directory who use identity federation with SAML
  

IAM roles issue keys that are valid for short durations, making them a more secure way to grant access.

Role is **global** (Region is not specified).

It is possible to attach IAM Role to running EC2 instance;


## CREATE A ROLE FOR EC2

**1. Role creation**

AWS Console -> IAM service ->  Roles link -> Create Role button -> AWS Service -> EC2 -> Next: Permissions button -> AmazonS3FullAccess is checked -> Next: Review button -> Role Name: S3-Admin-Role -> Create Role button


**2. Create EC2 with new Role**

**3. Verify EC2 sees S3**

Log to EC2 and see S3 buckets.
```
aws s3 ls
```

It works, but no credentials are stored locally:
```
ls -la ~/.aws

      ls: cannot access /root/.aws: No such file or directory
```



































