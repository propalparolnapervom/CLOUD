## WHAT

[Docs](https://eksctl.io/)

`eksctl` is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes service for EC2. 
It is written in Go, uses CloudFormation, was created by Weaveworks

## Cluster

### List
Show list of clusters, available for `eksctl`
```
eksctl get clusters

eksctl get clusters --region eu-west-2

export AWS_REGION=eu-west-2
eksctl get clusters

# Make sure the cluster is available for eksctl tool
eksctl get cluster --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION}
```

### Install

> NOTE: If some key is not specified, default one is in use


Install via config file
```
# 1. Create a config file eks_cluster.yaml


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
  

# 2. Install EKS cluster from the file
eksctl create cluster -f eks_cluster.yaml

```

Install via command-line 

> NOTE: Currently this example doesn't include definition of:
> - how to use your private ssh key to connect to the nodes (so you can't);
> - how to define which EC2 instance type to use for a worker nodes (so a big one created automatically)
```
export EKS_CLUSTER_NAME="my-cluster-via-eksctl"
export EKS_VER="1.17"
export AWS_REGION="eu-central-1"
export SSH_KEY_4_WORK_NODES="/home/sburtovyi/overall/to_del_dir/k8s_experiments/xbsTestEKSCreationNode.pem"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/k8s_experiments/${EKS_CLUSTER_NAME}.kubeconfig"

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


### Update

Update `K8S` version of your `EKS control plane` on 1 minor version later than it's current version
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
# Could be only next K8S minor version
export NEW_EKS_VERSION="1.18"

# Dry-run
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION}

# Actual steps
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION} --approve
```
### Connect

Create a KUBECONFIG file for a specific EKS cluster
```
export AWS_REGION="eu-central-1"
export AWS_ACCOUNT="491792459462"
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export KUBECONFIG="/home/sburtovyi/overall/to_del_dir/k8s_experiments/KUBECONFIG_${EKS_CLUSTER_NAME}"

aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION} --dry-run > ${KUBECONFIG}

kubectl get pods --all-namespaces
```


### Delete

Delete specific EKS cluster
```
eksctl delete cluster --name ${EKS_CLUSTER_NAME} --region ${AWS_REGION}
```


## Nodegroup

### List

List Nodegroups in the EKS cluster
```
export AWS_REGION="us-east-1"
export EKS_CLUSTER_NAME="data-dev"
export EKS_NODEGROUP_NAME="managed-ng-1"

eksctl get nodegroup --cluster=${EKS_CLUSTER_NAME} --region=${AWS_REGION}

eksctl get nodegroup --cluster=<clusterName> --name=${EKS_NODEGROUP_NAME} --region=${AWS_REGION}

eksctl get nodegroup --cluster=${EKS_CLUSTER_NAME} --output=yaml --region=${AWS_REGION}
```

### Update

To start version update process for specific Managed node group:
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export EKS_NODEGROUP_NAME="managed-ng-1"
export NEW_EKS_VERSION="1.18"

eksctl upgrade nodegroup --name=${EKS_NODEGROUP_NAME} --cluster=${EKS_CLUSTER_NAME} --kubernetes-version=${NEW_EKS_VERSION}

```

### Check health issues
```
# Check health issues for the specific Nodegroup
eksctl utils nodegroup-health --name=${EKS_NODEGROUP_NAME} --cluster=${EKS_CLUSTER_NAME} --region ${AWS_REGION}
```


## Addons

[Docs](https://eksctl.io/usage/addons/)

Starting from `1.18`, EKS can manage following K8S `add-ons` (or they can still be manually managed, if upgraded from previous version and decided not to install EKS management on them):
- kube-proxy
- aws-node
- coredns



### List

Which `default EKS addons` are enabled for your Kubernetes cluster in AWS
```
eksctl get addons --cluster <cluster-name>
```

Which `default EKS addons` are available for installation for your Kubernetes cluster in AWS
```
eksctl utils describe-addon-versions --cluster <cluster-name>
```

Which `default EKS addons` are available in general for specific Kubernetes version in AWS
```
eksctl utils describe-addon-versions --kubernetes-version 1.18
```

### Get

Get current version of the installed EKS add-on.
```
export EKS_ADDON_NAME="vpc-cni"  # could be: vpc-cni, kube-proxy, coredns
export EKS_CLUSTER_NAME="my-cluster-eksctl"

eksctl get addon --name ${EKS_ADDON_NAME} --cluster ${EKS_CLUSTER_NAME}
```
