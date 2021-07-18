# Documentation

[Initial simplest EKS installation](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)


# Installation steps

```
export AWS_REGION="eu-central-1"
```

## 1. Create EKS

1. Create an Amazon VPC
```
aws cloudformation create-stack \
  --region ${AWS_REGION} \
  --stack-name my-eks-vpc-stack \
  --template-url https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml
```

2. Create IAM role

```
aws iam create-role \
  --role-name myAmazonEKSClusterRole \
  --assume-role-policy-document file://"cluster-role-trust-policy.json"

aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy \
  --role-name myAmazonEKSClusterRole
```

3-8. Create EKS cluster as described on this steps
[https://console.aws.amazon.com/eks/home#/clusters](https://console.aws.amazon.com/eks/home#/clusters)


## 2. Configure access to EKS from your local machine

```
export AWS_REGION="eu-central-1"
export AWS_ACCOUNT="491792459462"
export EKS_CLUSTER_NAME="my-cluster"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/kubernetes_experiments/KUBECONFIG_${EKS_CLUSTER_NAME}"
```

Show content of possible KUBECONFIG file (dry-run)
```
aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION} --dry-run > ${KUBECONFIG}
```

Test connection
```
kubectl get svc
```

## 3. Create an IAM OpenID Connect (OIDC) provider

OpenID Connect provider URL

[https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355](https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355)


## 4. Create nodes

> You can create a cluster with one of the following node types:
>  - Fargate
>  - Managed nodes

This is to create a **Amazon EC2 Linux managed node group**.

1. Create an IAM role for the Amazon VPC CNI plugin 

The Amazon EKS Amazon VPC CNI plugin is installed on a cluster, by default (starting from EKS versions 1.18). 

The plugin assigns an IP address from your VPC to each pod.

```
# create cni-role-trust-policy.json file
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Federated": "arn:aws:iam::491792459462:oidc-provider/oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355"
        },
        "Action": "sts:AssumeRoleWithWebIdentity",
        "Condition": {
          "StringEquals": {
            "oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355:sub": "system:serviceaccount:kube-system:aws-node"
          }
        }
      }
    ]
  }
  

# create Role from that cni-role-trust-policy.json file
aws iam create-role \
  --role-name myAmazonEKSCNIRole \
  --assume-role-policy-document file://"cni-role-trust-policy.json"

# Attach the required Amazon EKS managed IAM policy to the role.
aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy \
  --role-name myAmazonEKSCNIRole
```


2. Associate the Kubernetes service account used by the VPC CNI plugin to the IAM role.

> NOTE: Assumtions is that you are installing EKS version starting from 1.18, 
> where Amazon VPC CNI is already installed automatically by this time
> (during creation of control plain)
> 
```
aws eks update-addon \
  --cluster-name ${EKS_CLUSTER_NAME} \
  --addon-name vpc-cni \
  --service-account-role-arn arn:aws:iam::${AWS_ACCOUNT}:role/myAmazonEKSCNIRole 
```

