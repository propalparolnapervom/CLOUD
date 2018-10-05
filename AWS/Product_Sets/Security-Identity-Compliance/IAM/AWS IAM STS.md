# AWS IAM STS


## OVERALL

[Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)

You can use the **AWS Security Token Service (AWS STS)** to create and provide trusted users with temporary security credentials that can control access to your AWS resources. Temporary security credentials work almost identically to the long-term access key credentials that your IAM users can use, with the following differences:

Temporary security credentials are short-term, as the name implies. They can be configured to last for anywhere from a few minutes to several hours. After the credentials expire, AWS no longer recognizes them or allows any kind of access from API requests made with them.

Temporary security credentials are not stored with the user but are generated dynamically and provided to the user when requested. When (or even before) the temporary security credentials expire, the user can request new credentials, as long as the user requesting them still has permissions to do so.


## Creating a Role to Delegate Permissions to an IAM User

[Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)

You can use IAM roles to delegate access to your AWS resources. 

With IAM roles, you can establish trust relationships between your trusting account and other AWS trusted accounts. 

The trusting account owns the resource to be accessed and the trusted account contains the users who need access to the resource. 

However, it is possible for another account to own a resource in your account. 

> For example, the trusting account might allow the trusted account to create new resources, such as creating new objects in an Amazon S3 bucket. In that case, the account that creates the resource owns the resource and controls who can access that resource.


After you create the trust relationship, an IAM user or an application from the trusted account can use the AWS Security Token Service (AWS STS) **AssumeRole** API operation. This operation provides temporary security credentials that enable access to AWS resources in your account.

## AssumeRole

[Docs](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)





























