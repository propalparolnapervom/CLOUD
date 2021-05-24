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

List buckets, files time modification NEWER then some date
```
BUCKET_NAME="90poe-ui-drydock"
aws s3api list-objects --bucket ${BUCKET_NAME} --query 'Contents[?LastModified>`2020-01-16`][].{Key: Key}'
```

List buckets, files time modification OLDER then some date
```
BUCKET_NAME="90poe-ui-drydock"
aws s3api list-objects --bucket ${BUCKET_NAME} --query 'Contents[?LastModified<`2020-01-16`][].{Key: Key}'
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
aws s3 cp D:\Khlam\html\index.html s3://xbs-web-bash
```

Place Dir to S3 bucket
```
aws s3 cp SOURCE_DIR s3://DEST_BUCKET/ --recursive
```

**Bucket to Local PC**

Place HTML-page to `xbs-web-bash` S3 bucket.
```
aws s3 cp s3://xbs-web-bash/index.html D:\Khlam\html
```

Place ONLY CONTENT (so no folder structure) of the `db_recreation` dir from S3 bucket to local PC
```
aws s3 sync s3://bucket_name/path_to_folder/db_recreation .

   OR
   
aws s3 cp s3://bucket_name/path_to_folder/db_recreation . --recursive
```





## EKS

### Authentication

Which IAM account you have used to log in to AWS 
```
aws sts get-caller-identity 

{
    "UserId": "ASDFASFD:<you_account_name>",
    "Account": "<you_account_number>",
    "Arn": "arn:aws:sts::<you_account_number>:assumed-role/<iam_role_name>/<you_account_number>"
}
```

Get a cluster authentication token
```
aws eks get-token --cluster-name <EKS_CLUSTER_NAME>

   # OR
   
aws eks get-token --cluster-name <EKS_CLUSTER_NAME> -r <AWS_IAM_ROLE_YOU_AUTH_WITH_ON_EKS>
```

Verify a token
```
aws-iam-authenticator verify -i jenkins-new -t "k8s-aws-v1..."
```

Create a KUBECONFIG file automatically (dry-run)
```
aws eks update-kubeconfig --name <EKS_CLUSTER_NEW> --region <EKS_AWS_REGION> --role-arn <AWS_IAM_ROLE_TO_AUTH_WITH> --dry-run
```

### Cluster

Describe EKS cluster
```
aws eks describe-cluster --name <EKS_CLUSTER_NAME>

{
    "cluster": {
        "name": "<EKS_CLUSTER_NAME>",
        "arn": "arn:aws:eks:us-east-1:<aws_acc_number>:cluster/<EKS_CLUSTER_NAME>",
        "createdAt": <SOME_TIME>,
        "version": "1.17",
        "endpoint": "https://<some_code>.gr7.<aws_region>.eks.amazonaws.com",
        "roleArn": "arn:aws:iam::<aws_acc_number>:role/<iam_role_name>",
        "resourcesVpcConfig": {
            "subnetIds": [
               ...
            ],
            "securityGroupIds": [
               ...
            ],
            "clusterSecurityGroupId": <some_SG_name>,
            "vpcId": <SOME_VPC_NAME>,
            "endpointPublicAccess": true,
            "endpointPrivateAccess": false,
            "publicAccessCidrs": [
               ...
            ]
        },
        "kubernetesNetworkConfig": {
               ...
        },
        "logging": {
            "clusterLogging": [
                {
                    "types": [
                        ...
                    ],
                    "enabled": false
                }
            ]
        },
        "identity": {
            "oidc": {
               ...
            }
        },
        "status": "ACTIVE",
        "certificateAuthority": {
            "data": "<CERTIFIACTE_FOR_YOU_CLUSTER>"
        },
        "platformVersion": "eks.4",
        "tags": {}
    }
}


```















