# AWS SWITCH ROLE



## Creating a Role to Delegate Permissions to an IAM User

[Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)

You can use IAM roles to delegate access to your AWS resources. 

With IAM roles, you can establish trust relationships between your trusting account and other AWS trusted accounts. 

The **trusting account** owns the resource to be accessed and the **trusted account** contains the users who need access to the resource. 

However, it is possible for another account to own a resource in your account. 

> For example, the *trusting account* might allow the *trusted account* to create new resources, such as creating new objects in an Amazon S3 bucket. In that case, the account that creates the resource owns the resource and controls who can access that resource.

After you create the trust relationship, an IAM user or an application from the *trusted account* can use the **AWS Security Token Service (AWS STS)** [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API operation. This operation provides temporary security credentials that enable access to AWS resources in your account.



### AssumeRole

[Docs](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)

**AssumeRole** returns a set of temporary security credentials that you can use to access AWS resources that you might not normally have access to. 

These temporary credentials consist of 
  - an access key ID
  - a secret access key
  - a security token. 

Typically, you use AssumeRole for cross-account access or federation.

The following [table](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_request.html#stsapi_comparison) compares features of the API operations in AWS STS that return temporary security credentials. 

> Note: You cannot use *AWS account root user* credentials to call AssumeRole. You must use credentials for an IAM user or an IAM role to call AssumeRole.

________________

For cross-account access, imagine that you own multiple accounts and need to access resources in each account. 

You could create long-term credentials in each account to access those resources. 

However, managing all those credentials and remembering which one can access which account can be time consuming. 

Instead, you can create one set of long-term credentials in one account and then use temporary security credentials to access all the other accounts by assuming roles in those accounts.

_____

By default, the temporary security credentials created by AssumeRole last for one hour. However, you can use the optional DurationSeconds parameter to specify the duration of your session. You can provide a value from 900 seconds (15 minutes) up to the maximum session duration setting for the role. This setting can have a value from 1 hour to 12 hours. To learn how to view the maximum value for your role, see [View the Maximum Session Duration Setting for a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html#id_roles_use_view-role-max-session) in the IAM User Guide. The maximum session duration limit applies when you use the AssumeRole* API operations or the assume-role* CLI commands. However the limit does not apply when you use those operations to create a console URL. 

______

To assume a role, your AWS account must be trusted by the role. The **trust relationship** is defined in the role's trust policy when the role is created. That trust policy states which accounts are allowed to delegate access to this account's role.

The user who wants to access the role must also have permissions delegated from the role's administrator. If the user and the role are in a different account, then the user's administrator must attach a policy. That attached policy must allow the user to call AssumeRole for the ARN of the role in the other account. If the user is in the same account as the role, then you can do either of the following:
  - Attach a policy to the user (identical to the previous user in a different account)
  - Add the user as a principal directly in the role's trust policy.

In this case, the trust policy acts as the only resource-based policy in IAM. Users in the same account as the role do not need explicit permission to assume the role.





## Granting a User Permissions to Switch Roles

[Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html?icmpid=docs_iam_console)

When you create a role for cross-account access, you establish trust from the account that owns the role and the resources (trusting account) to the account that contains the users (trusted account). To do this, you specify the trusted account number as the Principal in the role's trust policy. That allows potentially any user in the trusted account to assume the role. To complete the configuration, the administrator of the trusted account must give specific groups or users in that account permission to switch to the role.

To grant a user permission to switch to a role, you create a new policy for the user or edit an existing policy to add the required elements. You can then send the users a link that takes the user to the Switch Role page with all the details already filled in. Alternatively, you can provide the user with the account ID number or account alias that contains the role and the role name. The user then goes to the Switch Role page and adds the details manually. For details on how a user switches roles, see Switching to a Role (Console).

Note that you can switch roles only when you sign in as an IAM user. You cannot switch roles when you sign in as the AWS account root user.

























