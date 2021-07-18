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

3.
[https://console.aws.amazon.com/eks/home#/clusters](https://console.aws.amazon.com/eks/home#/clusters)


## 2. Configure access to EKS

```
export AWS_REGION="eu-central-1"
export EKS_CLUSTER_NAME="my-cluster"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/kubernetes_experiments/KUBECONFIG_${EKS_CLUSTER_NAME}"
```

Show content of possible KUBECONFIG file (dry-run)
```
aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION} --dry-run > ${KUBECONFIG}
```

## 3. Create an IAM OpenID Connect (OIDC) provider

OpenID Connect provider URL

[https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355](https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355)


## 4. Create nodes


1. Create an IAM role for the Amazon VPC CNI plugin 

The Amazon EKS Amazon VPC CNI plugin is installed on a cluster, by default. The plugin assigns an IP address from your VPC to each pod.

```
# create cni-role-trust-policy.json
aws iam create-role \
  --role-name myAmazonEKSCNIRole \
  --assume-role-policy-document file://"cni-role-trust-policy.json"


aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy \
  --role-name myAmazonEKSCNIRole
```


2. Associate the Kubernetes service account used by the VPC CNI plugin to the IAM role.

```
aws eks update-addon \
  --cluster-name ${EKS_CLUSTER_NAME} \
  --addon-name vpc-cni \
  --service-account-role-arn arn:aws:iam::491792459462:role/myAmazonEKSCNIRole 
```

