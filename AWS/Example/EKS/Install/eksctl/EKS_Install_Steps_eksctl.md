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

## Install (command line)
Set up necessary vars
```
export EKS_CLUSTER_NAME="my-cluster-via-eksctl"
export EKS_VER="1.17"
export AWS_REGION="eu-central-1"
export SSH_KEY_4_WORK_NODES="/home/sburtovyi/overall/to_del_dir/k8s_experiments/xbsTestEKSCreationNode.pem"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/k8s_experiments/${EKS_CLUSTER_NAME}.kubeconfig"

```

Run installation command with necessary keys (if some key is not specified, default one is in use):
```
# Dry-run
eksctl create cluster \
--name ${EKS_CLUSTER_NAME} \
--version ${EKS_VER} \
--region ${AWS_REGION} \
--with-oidc \
--managed \
--dry-run

# Actual steps
eksctl create cluster \
--name ${EKS_CLUSTER_NAME} \
--version ${EKS_VER} \
--region ${AWS_REGION} \
--with-oidc \
--kubeconfig ${KUBECONFIG} \
--managed
```

## Intall (config file)

[Docs](https://eksctl.io)

Create a config file `eks_cluster.yaml`
```
###########################
## Origin of this example:
## https://eksctl.io/
###########################
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: my-cluster-eksctl
  region: eu-central-1
  version: "1.17"

managedNodeGroups:
  - name: managed-ng-1
    instanceType: t2.micro
    minSize: 2
    maxSize: 2
    desiredCapacity: 2
    volumeSize: 10
    ssh: # use existing EC2 key
      # It means that AWS already contains EC2 SSH key,
      # and it's name should be defined here
      # (it's not the file location on your PC)
      publicKeyName: xbsTestEKSCreationNode

iam:
  withOIDC: true
```

Install EKS cluster from the file
```
eksctl create cluster -f eks_cluster.yaml
```

# CONNECT

```
export AWS_REGION="eu-central-1"
export AWS_ACCOUNT="491792459462"
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/k8s_experiments/KUBECONFIG_${EKS_CLUSTER_NAME}"

aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION} --dry-run > ${KUBECONFIG}

kubectl get pods --all-namespaces
```

# DELETE EKS

```
eksctl delete cluster --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION}
```

