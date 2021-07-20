## Documentation

[Kubernetes upgrade order](https://kubernetes.io/releases/version-skew-policy/#supported-component-upgrade-order)

[EKS update steps](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html)


## Pre-installation

### Subnets have free IP
To update the cluster, Amazon EKS requires two to three free IP addresses from the subnets that were provided when you created the cluster. 
If these subnets don't have available IP addresses, then the update can fail. 

For example, go to the subnet in AWS Management console, select the subnet, see `Details` tab for free IPs.

### Subnets/SG are original ones
if any of the subnets or security groups that were provided during cluster creation have been deleted, the cluster update process can fail.


## Update EKS cluster

### 1. Preparation: Control plane version vs Nodes version

Compare the Kubernetes version of your cluster control plane to the Kubernetes version of your nodes.

> If your control plane is running version 1.20 and any of your nodes are running version 1.19, update your nodes to version 1.20 before updating your control plane's Kubernetes version to 1.21.

> See the original page to get node update links.


Get the Kubernetes version of your cluster control plane
```
kubectl version --short
```

Get the Kubernetes version of your nodes
```
kubectl get nodes
```

### 2. Preparation: Ensure that the proper pod security policies are in place

The `pod security policy admission controller` is enabled by default on Amazon EKS clusters. 

Before updating your cluster, ensure that the proper `pod security policies` are in place before you update to avoid any issues.
```
kubectl get psp eks.privileged
```

### 3. Preparation: Remove a discontinued term from your CoreDNS manifest (<= 1.17)

If you originally deployed your cluster on Kubernetes `1.17` or earlier, then you may need to remove a discontinued term from your CoreDNS manifest.

a. Check to see if your CoreDNS manifest has a line that only has the word upstream.
```
kubectl get configmap coredns -n kube-system -o jsonpath='{$.data.Corefile}' | grep upstream
```
If no output is returned, your manifest doesn't have the line and you can skip to the next step to update your cluster. 

If the word `upstream` is returned, then you need to remove the line (so do the next step).

b. Edit the configmap, removing the line near the top of the file that only has the word upstream. 

Don't change anything else in the file. 

After the line is removed, save the changes.
```
kubectl edit configmap coredns -n kube-system -o yaml

kubectl get configmap coredns -n kube-system -o jsonpath='{$.data.Corefile}' | grep upstream
```

### 4. Update: Cluster (Control Plane)

It can be done via eksctl/AWS Console/AWS CLI.

> NOTE: Because Amazon EKS runs a highly available control plane, you can update only one minor version at a time.
> 
> (if your current version is `1.19` and you want to update to `1.21`, 
> then you must first update your cluster to `1.20` and then update it from `1.20` to `1.21`)

> NOTE: Updating your cluster to a newer version may overwrite custom configurations.

> NOTE: Check other `Important` notes for this step on original doc page.

Version of the `eksctl` should be >= `0.58.0`
```
eksctl version
```

> NOTE: If you use local clients, your laptop has to be out of sleep for a few mins

Update Kubernetes version of your EKS control plane on 1 minor version later than it's current version
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
# Could be only next K8S minor version
export NEW_EKS_VERSION="1.18"

# Dry-run
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION}

# Actual steps
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION} --approve
```

### 5. Update: Worker Nodes

Update your nodes to the same Kubernetes minor version as your just updated cluster.


#### 5.1. Worker Nodes: AWS managed node group

[Docs: Updating managed node group](https://docs.aws.amazon.com/eks/latest/userguide/update-managed-node-group.html)
[Docs: Linux AMIs, optimized for EKS](https://docs.aws.amazon.com/eks/latest/userguide/eks-linux-ami-versions.html)

When you start updating Managed node group, EKS automatically completes [following](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-update-behavior.html) tasks for you.

To start version update process for specific Managed node group:
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export EKS_NODEGROUP_NAME="managed-ng-1"
export NEW_EKS_VERSION="1.18"

eksctl upgrade nodegroup --name=${EKS_NODEGROUP_NAME} --cluster=${EKS_CLUSTER_NAME} --kubernetes-version=${NEW_EKS_VERSION}
```

#### 5.2. Worker Nodes: Self-managed node group

[Docs: Updating Self-managed node group](https://docs.aws.amazon.com/eks/latest/userguide/update-workers.html)




### 6.(Optional) Update: Kubernetes Cluster Autoscaler

If you deployed the `Kubernetes Cluster Autoscaler` to your cluster before updating the cluster, 
update the `Cluster Autoscaler` to the latest version that matches the Kubernetes major and minor version that you updated to.

> NOTE: See original page.



### 7. (Optional) Update: NVIDIA device plugin

Clusters with GPU nodes only. So - usually skip it.


### 8. Update: Add-ons

[Docs: Amazon EKS add-on configuration](https://docs.aws.amazon.com/eks/latest/userguide/add-ons-configuration.html)

Steps depend on K8S version you are updating to, so see original page.

#### 8.1. Update: VPC CNI (when updated to `1.18`)

If you updated to `1.18`, you can **add** [Amazon VPC CNI Amazon EKS add-on](https://docs.aws.amazon.com/eks/latest/userguide/managing-vpc-cni.html#adding-vpc-cni-eks-add-on). 

> NOTE: **add** - as it was not present in EKS before this version.

To add the `vpc-cni` Amazon EKS add-on using `eksctl`:
```
export AWS_ACCOUNT_NUM="491792459462"
export EKS_CLUSTER_NAME="my-cluster-eksctl"


eksctl create addon \
    --name vpc-cni \
    --version latest \
    --cluster ${EKS_CLUSTER_NAME} \
    --service-account-role-arn arn:aws:iam::${AWS_ACCOUNT_NUM}:role/<eksctl-my-cluster-addon-iamserviceaccount-kube-sys-Role1-UK9MQSLXK0MW> \
    --force
```




#### 8.2. Update: CoreDNS



#### 8.3. Update: kube-proxy





## Post-installation

### Update kubectl
Update `kubectl` tool on:
- local machines of all users;
- CI/CD pipelines (if needed).












