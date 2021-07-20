## Documentation

[Kubernetes upgrade order](https://kubernetes.io/releases/version-skew-policy/#supported-component-upgrade-order)

[EKS update steps](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html)

[eksctl docs](https://eksctl.io/)


## Pre-installation

### Subnets have free IP
To update the cluster, Amazon EKS requires two to three free IP addresses from the subnets that were provided when you created the cluster. 
If these subnets don't have available IP addresses, then the update can fail. 

For example, go to the subnet in AWS Management console, select the subnet, see `Details` tab for free IPs.

### Subnets/SG are original ones
if any of the subnets or security groups that were provided during cluster creation have been deleted, the cluster update process can fail.


## Update EKS cluster (control plane)

### 1. Compare the Kubernetes version of your cluster control plane to the Kubernetes version of your nodes.

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

### 2. Ensure that the proper pod security policies are in place

The `pod security policy admission controller` is enabled by default on Amazon EKS clusters. 

Before updating your cluster, ensure that the proper `pod security policies` are in place before you update to avoid any issues.
```
kubectl get psp eks.privileged
```

### 3. Remove a discontinued term from your CoreDNS manifest (<= 1.17)

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

### Update the cluster (eksctl/AWS Console/AWS CLI)

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



## Post-installation

### Update kubectl
Update `kubectl` tool on:
- local machines of all users;
- CI/CD pipelines (if needed).












