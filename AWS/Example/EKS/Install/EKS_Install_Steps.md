# Documentation

[Initial simplest EKS installation](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)


# Installation steps

Mine variables to help run further steps
```
export AWS_REGION="eu-central-1"
export AWS_ACCOUNT="491792459462"
export EKS_CLUSTER_NAME="my-cluster"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/kubernetes_experiments/KUBECONFIG_${EKS_CLUSTER_NAME}"
```


# Step 1: Create your Amazon EKS cluster 

Basicly, Kubernetes `control plain` is created on this block of steps.

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

3-8. Create EKS cluster (its control plain) as described on this steps
[https://console.aws.amazon.com/eks/home#/clusters](https://console.aws.amazon.com/eks/home#/clusters)




# Step 2: Configure your computer to communicate with your cluster

> NOTE: Connection works on this step only if same IAM user was used during:
>  1) creating EKS cluster (control plain) via AWS managed console;
>  2) to setup credentials on your local machine, where you run `kubectl`

Show content of possible KUBECONFIG file (dry-run)
```
aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION} --dry-run > ${KUBECONFIG}
```

Test connection
```
kubectl get svc
```


# Step 3: Create an IAM OpenID Connect (OIDC) provider

Create an IAM OpenID Connect (OIDC) provider for your cluster so that Kubernetes service accounts used by workloads can access AWS resources. You only need to complete this step one time for a cluster.

1-8. Do the steps

[https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355](https://oidc.eks.eu-central-1.amazonaws.com/id/495D8CF9516BE87DC49ACFC20D44C355)




## Step 4: Create nodes

So Kubernetes `control plain` is created by now. Have to add `woker nodes` now.

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

> NOTE: Assumtion is that you are installing EKS version starting from 1.18, 
> where Amazon VPC CNI is already installed automatically by this time
> (during creation of control plain)
> 
```
aws eks update-addon \
  --cluster-name ${EKS_CLUSTER_NAME} \
  --addon-name vpc-cni \
  --service-account-role-arn arn:aws:iam::${AWS_ACCOUNT}:role/myAmazonEKSCNIRole 
```

3. Create a node IAM role and attach the required Amazon EKS IAM managed policy to it. The Amazon EKS node kubelet daemon makes calls to AWS APIs on your behalf. Nodes receive permissions for these API calls through an IAM instance profile and associated policies.
```
# Create node-role-trust-policy.json file
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

# Create the node IAM role from that node-role-trust-policy.json file
aws iam create-role \
  --role-name myAmazonEKSNodeRole \
  --assume-role-policy-document file://"node-role-trust-policy.json"


# Attach the required Amazon EKS managed IAM policies to the role.
aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy \
  --role-name myAmazonEKSNodeRole
  
aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly \
  --role-name myAmazonEKSNodeRole

```

4-12. Create nodes (EKS managed ones!)

[https://console.aws.amazon.com/eks/home#/clusters](https://console.aws.amazon.com/eks/home#/clusters)

To be able to SSH to the node once it's created, specify `SSH key` on this step. Or create as following:

```
aws ec2 create-key-pair --region ${AWS_REGION} --key-name xbsTestEKSCreationNode > xbsTestEKSCreationNode.pem
```

Specify this `SSH key` on the web-page then.

> NOTE: SG following SG will be created - with access only from your local machine, to be able to connect to the created Node, but restrict access for others - the EKS cluster can't work, as Nodes can't be accessed from EKS controlplain.





