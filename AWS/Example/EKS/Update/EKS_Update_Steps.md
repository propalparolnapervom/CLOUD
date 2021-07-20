## Documentation

[Kubernetes upgrade order](https://kubernetes.io/releases/version-skew-policy/#supported-component-upgrade-order)

[EKS update steps](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html)

[eksctl docs](https://eksctl.io/)


## Preparation

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

The pod security policy admission controller is enabled by default on Amazon EKS clusters. Before updating your cluster, ensure that the proper pod security policies are in place before you update to avoid any issues.
```
kubectl get psp eks.privileged
```

### 3. Remove a discontinued term from your CoreDNS manifest (<= 1.17)

If you originally deployed your cluster on Kubernetes 1.17 or earlier, then you may need to remove a discontinued term from your CoreDNS manifest.

a. Check to see if your CoreDNS manifest has a line that only has the word upstream.
```
kubectl get configmap coredns -n kube-system -o jsonpath='{$.data.Corefile}' | grep upstream
```
If no output is returned, your manifest doesn't have the line and you can skip to the next step to update your cluster. If the word upstream is returned, then you need to remove the line.

b. Edit the configmap, removing the line near the top of the file that only has the word upstream. Don't change anything else in the file. After the line is removed, save the changes.
```
kubectl edit configmap coredns -n kube-system -o yaml
```

### Update the cluster (eksctl/AWS Console/AWS CLI)


















