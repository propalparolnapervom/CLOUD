# AWS CLI COMMANDS


## DOCS

[Command Refference](https://docs.aws.amazon.com/cli/latest/index.html)



## OVERALL

**Version**
```
aws --version
```

**Help**
```
   #aws <service> help
   
aws s3 help
```


## EC2

[EC2 Command Refference](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html)

**Describe EC2 instances**
```
aws ec2 describe-instances
```

**Terminate EC2 instances**
```
aws ec2 terminate-instances --instance-ids i03-002rower235
```

## S3

[S3 Command Refference](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)

### BUCKETS

List buckets
```
aws s3 ls

      2018-08-19 10:29:22 xburser
      2018-08-19 10:28:33 xburserlondon
```

Copy bucket `xburser` to bucket `xburser london`
```
aws s3 cp --recursive s3://xburser s3://xburserlondon
```
























