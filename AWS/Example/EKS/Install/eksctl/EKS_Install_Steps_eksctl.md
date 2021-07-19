# EKSCTL
It's about how to create an EKS cluster with 1 command - so it doesn't give an extended understanding what's going on under the hood.

# DOCUMENTATION

[Getting started with EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

[eksctl utility](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

# INSTALL EKS

## Prerequisites

- **kubectl** – A command line tool for working with Kubernetes clusters. This guide requires that you use version 1.20 or later. For more information, see Installing kubectl.

- **eksctl** – A command line tool for working with EKS clusters that automates many individual tasks. This guide requires that you use version 0.56.0 or later. For more information, see The eksctl command line utility.

- **Required IAM permissions** – Create IAM user, that has right to create EKS and all this components. Configure access for that user from your local machine (via `~/.aws/credentials`, for example).

> NOTE: If EKS cluster is created under one IAM user and you'll try to connect under other user - it won't work, as by default only creation user is configured as the one who can connect to the EKS cluster (`aws-auth` configmap in the Kubernetes cluster).

- **ssh key pair** (optional) - if you gonna try to connect worker nodes via SSH.

> NOTE: If you don't have SSH key created: `aws ec2 create-key-pair --region us-west-2 --key-name myKeyPair`

## Install
Set up necessary vars
```
export EKS_CLUSTER_NAME="my-cluster-via-eksctl"
export AWS_REGION="eu-central-1"
export SSH_KEY_4_WORK_NODES="/home/sburtovyi/overall/to_del_dir/k8s_experiments/xbsTestEKSCreationNode.pem"

```

Run installation command with necessary keys (if some key is not specified, default one is in use):
```
eksctl create cluster \
--name ${EKS_CLUSTER_NAME} \
--region ${AWS_REGION} \
--with-oidc \
--ssh-access \
--ssh-public-key ${SSH_KEY_4_WORK_NODES} \
--managed
```

# DELETE EKS

```
eksctl delete cluster --name my-cluster --region us-west-2
```

