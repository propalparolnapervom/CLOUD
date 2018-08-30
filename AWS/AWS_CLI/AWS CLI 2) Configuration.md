# AWS CLI CONFIGURATION

[Config Docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)


## QUICK CONFIGURATION

For general use, the `aws configure` command is the fastest way to set up your AWS CLI installation.

[Region Names & Codes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions)

```
sudo su
aws configure

      AWS Access Key ID [None]: 
      AWS Secret Access Key [None]: 
      Default region name [None]: eu-central-1
      Default output format [None]: json
```


## CREDENTIALS STORAGE

Provided during AWS CLI configuration credentials are stored LOCALLY in the home of the config user (that's why ROLES are in use):
```
ls -la ~/.aws

      -rw------- 1 root root   46 Aug 30 11:59 config
      -rw------- 1 root root  116 Aug 30 11:59 credentials
 
 
cat ~/.aws/config

      [default]
      output = json
      region = eu-central-1
      
      
cat ~/.aws/credentials     

      [default]
      aws_access_key_id = LA...
      aws_secret_access_key = CHgc...
```






























