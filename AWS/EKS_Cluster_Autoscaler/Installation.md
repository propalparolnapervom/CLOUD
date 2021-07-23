## Documentation

[How Cluster Autoscaler works on EKS](https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html)

This topic describes how you can deploy the Cluster Autoscaler to your Amazon EKS cluster and configure it to modify your Amazon EC2 Auto Scaling groups.


## Pre-installation

### 1. IAM OIDC provider

[Docs: Work with IAM OIDC](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html)

Determine whether you have an existing `IAM OIDC` provider for your EKS cluster.
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"

aws eks describe-cluster --name ${EKS_CLUSTER_NAME} --query "cluster.identity.oidc.issuer" --output text

  https://oidc.eks.eu-central-1.amazonaws.com/id/<OIDC_NUM>
```

Make sure that `IAM OIDC`, which is EKS cluster configured to use (from output above), exists
```
aws iam list-open-id-connect-providers | grep <OIDC_NUM>

  "Arn": "arn:aws:iam::<ACC_NUM>:oidc-provider/oidc.eks.eu-central-1.amazonaws.com/id/<OIDC_NUM>"
```
 If `IAM OIDC` arn was found, check is finished successfully. Go to the next step.
 
 If `IAM OIDC` was not found, create it.
 
 
### 2. Node groups with Auto Scaling groups tags 
 
Make sure Node groups of your EKS cluster have following tags.
 
Key	                                              Value
--
k8s.io/cluster-autoscaler/<cluster-name>	        owned
k8s.io/cluster-autoscaler/enabled	                TRUE





