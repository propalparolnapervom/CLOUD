# AWS EC2: USER DATA (BASH SCRIPTING)

[User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)


## OVERALL

You can pass two types of user data to Amazon EC2: 
  - shell scripts
  - cloud-init directives.
  
You can also pass this data into the launch wizard as:
  - a plain text;
  - a file (this is useful for launching instances using the command line tools);
  - as base64-encoded text (for API calls).

If you are interested in more complex automation scenarios, consider using **AWS CloudFormation** and **AWS OpsWorks**. 

**Shell sript** - list of AWS command lines that will be run during EC2 instance startup.

Scripts entered as user data are executed as the **root** user.



## SHELL SCRIPT: EXAMPLE OF THE STEPS TO AUTOMATE USER DATA

Make some steps to automate a process of WebServer creation.

**1. Create necessary HTML-pages**

On your local PC:
```
vi index.html
  
  <html><h1> XBURSER: This is available AUTOMATICALLY!!! </h1></html>
```

**2. Place HTML-pages to S3**

For example, from your local PC via AWS CLI.

2.1. Create a `xbs-web-bash` S3 bucket (in the same Region `eu-central-1` where EC2 instance will be created).
```
aws s3 mb s3://xbs-web-bash --region eu-central-1
```

2.2. Make sure necessary S3 bucket has been created.
```
aws s3 ls
```

2.3. Place just created HTML-pages to this `xbs-web-bash` S3 bucket. No need to do it publically available.
```
aws s3 cp D:\Khlam\html\index.html s3://xbs-web-bash
```

**3. Create a Role**

Create an IAM role `S3-Admin-Access` for EC2, that will allow the full admin access to S3.


**4. Create EC2 instance**

Create an EC2 instance with:
  4.1. Just created Role.
  4.2. Advanced Details fullfilled with necessary `User Data` area (`As text` radiobutton is selected):
  
```
#!/bin/bash

yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on 

aws s3 cp s3://xbs-web-bash/index.html /var/www/html
```
  
  
  
**5. Verify availability of just automatically copied HTML-page**

Make a verification by entering public IP addres in a browser just after EC2 instance is up and running.


**6. Debug**

Check the output for errors of steps above.
```
cat /var/log/cloud-init-output.log
```
































