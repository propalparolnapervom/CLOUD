## WHAT

[Docs](https://eksctl.io/)

`eksctl` is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes service for EC2. 
It is written in Go, uses CloudFormation, was created by Weaveworks

## Cluster

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

## Nodegroup

### Update

To start version update process for specific Managed node group:
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export EKS_NODEGROUP_NAME="managed-ng-1"
export NEW_EKS_VERSION="1.18"

eksctl upgrade nodegroup --name=${EKS_NODEGROUP_NAME} --cluster=${EKS_CLUSTER_NAME} --kubernetes-version=${NEW_EKS_VERSION}

```


## Addons

[Docs](https://eksctl.io/usage/addons/)

Starting from `1.18`, EKS contains following 3 default `add-ons`:
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
