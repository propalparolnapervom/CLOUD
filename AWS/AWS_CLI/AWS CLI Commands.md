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

#### LIST

List buckets
```
aws s3 ls

      2018-08-19 10:29:22 xburser
      2018-08-19 10:28:33 xburserlondon
```

#### CREATE

Create a `xbs-web-bash` S3 bucket (in the same Region `eu-central-1` where EC2 instance will be created).
```
aws s3 mb s3://xbs-web-bash --region eu-central-1
```

> No "_" is allowed in the name of the S3 bucket.


#### DELETE

**Delete empty bucket**
```
aws s3 rb s3://xbswebbash
```

**Delete object in the bucket**
```
aws s3 rm s3://mybucket/test2.txt
```

#### COPY

**Bucket to Bucket**

Copy bucket `xburser` to bucket `xburser london`
```
aws s3 cp --recursive s3://xburser s3://xburserlondon
```

**Local PC to Bucket**

Place HTML-page to `xbs-web-bash` S3 bucket.
```
aws s3 cp D:\Khlam\html\index_xburser.html s3://xbs-web-bash
```

**Bucket to Local PC**

Place HTML-page to `xbs-web-bash` S3 bucket.
```
aws s3 cp s3://xbs-web-bash/index_xburser.html D:\Khlam\html
```




















