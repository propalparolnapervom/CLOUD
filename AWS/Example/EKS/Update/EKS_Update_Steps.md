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
export AWS_REGION="us-east-1"
export EKS_CLUSTER_NAME="my-cluster-eksctl"
# Could be only next K8S minor version
export NEW_EKS_VERSION="1.18"

# Dry-run
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION} --region ${AWS_REGION}

# Actual steps
eksctl upgrade cluster --name ${EKS_CLUSTER_NAME} --version=${NEW_EKS_VERSION} --region ${AWS_REGION} --approve
```

### 5. Update: Worker Nodes

Update your nodes to the same Kubernetes minor version as your just updated cluster.


#### 5.1. Worker Nodes: AWS managed node group

[Docs: Updating managed node group](https://docs.aws.amazon.com/eks/latest/userguide/update-managed-node-group.html)
[Docs: Linux AMIs, optimized for EKS](https://docs.aws.amazon.com/eks/latest/userguide/eks-linux-ami-versions.html)

When you start updating Managed node group, EKS automatically completes [following](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-update-behavior.html) tasks for you.

To start version update process for specific Managed node group:
```
export AWS_REGION="us-east-1"
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export EKS_NODEGROUP_NAME="managed-ng-1"
export NEW_EKS_VERSION="1.18"

eksctl get nodegroup --cluster=${EKS_CLUSTER_NAME} --region ${AWS_REGION} --output=yaml

eksctl upgrade nodegroup --name=${EKS_NODEGROUP_NAME} --cluster=${EKS_CLUSTER_NAME} --kubernetes-version=${NEW_EKS_VERSION} --region ${AWS_REGION}
```

#### 5.2. Worker Nodes: Self-managed node group

[Docs: Updating Self-managed node group](https://docs.aws.amazon.com/eks/latest/userguide/update-workers.html)




### 6.(Optional) Update: Kubernetes Cluster Autoscaler

If you deployed the `Kubernetes Cluster Autoscaler` to your cluster before updating the cluster, 
update the `Cluster Autoscaler` to the latest version that matches the Kubernetes major and minor version that you updated to.

Open the Cluster Autoscaler [releases page](https://github.com/kubernetes/autoscaler/releases) from GitHub in a web browser and find the latest Cluster Autoscaler version that matches the Kubernetes major and minor version of your cluster. 

> Note: For example, if the Kubernetes version of your cluster is `1.21`, find the latest `Cluster Autoscaler` release that begins with `1.21`. Record the semantic version number (`1.21.n`) for that release to use in the next step.

Set the Cluster Autoscaler image tag to the version that you recorded in the previous step with the following command.
```
export CLUSTER_AUTOSCALER_VER="1.18.3"  # Use latest for curr K8S release

k describe deployment -n kube-system cluster-autoscaler | grep -i image

kubectl set image deployment cluster-autoscaler \
  -n kube-system \
  cluster-autoscaler=k8s.gcr.io/autoscaling/cluster-autoscaler:v${CLUSTER_AUTOSCALER_VER}

kubectl describe deployment -n kube-system cluster-autoscaler | grep -i image
```

Make sure pod is up and running
```
kubectl get pods -n kube-system | grep -i autoscaler

kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler

```



### 7. (Optional) Update: NVIDIA device plugin

Clusters with GPU nodes only. So - usually skip it.



### 8. Update: Add-ons

[Docs: Amazon EKS add-on configuration](https://docs.aws.amazon.com/eks/latest/userguide/add-ons-configuration.html)

Steps might depend on:
- K8S version you are updating to, 
- Wherher you added you `K8S add-on` as `EKS add-on` or not (starting from `1.18` EKS can manage it for you, if you add it to EKS).

So see original page.

Also, starting from `1.18` version, `add-ons` can be managed wether by EKS, or (still) manually. Thus, affects its upgrade steps.


### 8.1. (Optional) When add-on is managed manually (not by EKS)

#### 8.1.1. Update (manual): VPC CNI

[Docs: Manual update of VPC CNI add-on](https://docs.aws.amazon.com/eks/latest/userguide/managing-vpc-cni.html#updating-vpc-cni-add-on)

Check current version of the K8S `vpc-cni` add-on.
```
kubectl describe daemonset aws-node --namespace kube-system | grep Image | cut -d "/" -f 2

amazon-k8s-cni-init:v1.7.5-eksbuild.1
amazon-k8s-cni:v1.7.5-eksbuild.1
```
> NOTE: Any changes you've made to the plugin's default settings on your cluster can be overwritten 
> with default settings when applying the new version of the manifest. 
> To prevent loss of your custom settings, download the manifest, change the default settings as necessary, 
> and then apply the modified manifest to your cluster. 

> NOTE: You should only update one minor version at a time. 
> For example, if your current version is `1.6` and you want to update to `1.8`, you should update to `1.7` first, then update to `1.8`.

[Latest VPC-CNI release](https://github.com/aws/amazon-vpc-cni-k8s/releases)

**(Optional) If you DO NOT care about custom parameters of VPC CI**
Download general manifest file.
```
curl -o aws-k8s-cni.yaml https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/release-1.8/config/v1.8/aws-k8s-cni.yaml
```
If necessary, replace `<region-code>` in the following command with the Region that your cluster is in and then run the modified command to replace the Region code in the file (currently `us-west-2`).
```
sed -i.bak -e 's/us-west-2/eu-central-1/g' aws-k8s-cni.yaml
```    

**(Optional: Start) Only if you DO care about possible custom parameters of VPC CNI**
Remember latest suggested version of the plugin:
```
cat aws-k8s-cni.yaml | grep amazon-k8s-cni

        "image": "602401143452.dkr.ecr.eu-central-1.amazonaws.com/amazon-k8s-cni:v1.8.0"
        "image": "602401143452.dkr.ecr.eu-central-1.amazonaws.com/amazon-k8s-cni-init:v1.8.0"
```


Download current manifest file.
```
kubectl get daemonset -n kube-system aws-node -o yaml
```

Update that part of `aws-k*s-cni.yaml` file with output above, where this `DaemonSet` is defined.

Update lines with plugin images to latest version (those, defined at the very start of this block).

Check what is suggested now (should be the same as above):
```
cat aws-k8s-cni.yaml | grep amazon-k8s-cni
```

**(Optional: Finish) Only if you DO care about possible custom parameters of VPC CNI**

Apply necessary configuration:
```
kubectl apply -f aws-k8s-cni.yaml

    clusterrolebinding.rbac.authorization.k8s.io/aws-node unchanged
    clusterrole.rbac.authorization.k8s.io/aws-node unchanged
    customresourcedefinition.apiextensions.k8s.io/eniconfigs.crd.k8s.amazonaws.com unchanged
    daemonset.apps/aws-node configured
    serviceaccount/aws-node unchanged
```

Wait until this up and running:
```
kubectl get pods -n kube-system | grep aws-node
```
Make sure it's `daemonset` has necessary plugin version:
```
kubectl describe daemonset aws-node --namespace kube-system | grep Image | cut -d "/" -f 2
```

#### 8.1.2. Update (manual): CoreDNS

[Docs: Manual update of CoreDNS add-on](https://docs.aws.amazon.com/eks/latest/userguide/managing-coredns.html#updating-coredns-add-on)

1. Check the current version of your CoreDNS deployment.
```
kubectl describe deployment coredns \
    --namespace kube-system \
    | grep Image \
    | cut -d "/" -f 3
    
    coredns:v1.6.6-eksbuild.1
```

2. If your current coredns version is `1.5.0` or later, but earlier than the version listed in the [CoreDNS versions table](https://docs.aws.amazon.com/eks/latest/userguide/managing-coredns.html#coredns-versions), then skip this step. 

If your current version is earlier than `1.5.0`, then you need to modify the ConfigMap for CoreDNS to use the forward plug-in, rather than the proxy plug-in.

Open the configmap with the following command.
```
kubectl edit configmap coredns -n kube-system
```
Replace proxy in the following line with `forward`. Save the file and exit the editor.
```
proxy . /etc/resolv.conf
```

3. If you originally deployed your cluster on Kubernetes 1.17 or earlier, then you may need to remove a discontinued term from your CoreDNS manifest.

> NOTE: You must complete this before upgrading to CoreDNS version `1.7.0`, but it's recommended that you complete this step even if you're upgrading to an earlier version.

Check to see if your CoreDNS manifest has the line.
```
kubectl get configmap coredns -n kube-system -o jsonpath='{$.data.Corefile}' | grep upstream
```

Edit the configmap with the following command, removing the line in the file that has the word upstream in it. Do not change anything else in the file. Once the line is removed, save the changes.
```
kubectl edit configmap coredns -n kube-system -o yaml
```

4. Retrieve your current coredns image:
```
kubectl get deployment coredns --namespace kube-system -o=jsonpath='{$.spec.template.spec.containers[:1].image}'

    602401143452.dkr.ecr.eu-central-1.amazonaws.com/eks/coredns:v1.6.6-eksbuild.1
```

5. If you're updating to CoreDNS `1.8.3`, you need to add the endpointslices permission to the `system:coredns` Kubernetes clusterrole.
```
kubectl edit clusterrole system:coredns -n kube-system

```
Add the following line under the existing permissions lines in the file.
```
...
- apiGroups:
  - discovery.k8s.io
  resources:
  - endpointslices
  verbs:
  - list
  - watch
...
```

6. Update CoreDNS deployment:
```
export AWS_ACC_4_AWS_IMAGES="602401143452" # usually it stays the same
export AWS_REGION="eu-central-1"
export NEW_COREDNS_VERSION="1.7.0" # recommended one for current K8S version

kubectl set image --namespace kube-system deployment.apps/coredns \
    coredns=${AWS_ACC_4_AWS_IMAGES}.dkr.ecr.${AWS_REGION}.amazonaws.com/eks/coredns:v${NEW_COREDNS_VERSION}-eksbuild.1
```

Check that new pods are up and running:
```
kubectl get pods -n kube-system | grep -i coredns
```

Check the deployment has necessary image:
```
kubectl describe deployment coredns \
    --namespace kube-system \
    | grep Image \
    | cut -d "/" -f 3
```



#### 8.1.3. Update (manual): kube-proxy

[Docs: Manual update of kube-proxy add-on](https://docs.aws.amazon.com/eks/latest/userguide/managing-kube-proxy.html#updating-kube-proxy-add-on)

> NOTE: Update your cluster and nodes to a new Kubernetes minor version before updating kube-proxy to the same minor version as your updated cluster's minor version

1. Check the current version of your kube-proxy deployment.
```
kubectl get daemonset kube-proxy --namespace kube-system -o=jsonpath='{$.spec.template.spec.containers[:1].image}'

602401143452.dkr.ecr.eu-central-1.amazonaws.com/eks/kube-proxy:v1.17.9-eksbuild.1
```

2. Check necessary `kube-proxy` version [here](https://docs.aws.amazon.com/eks/latest/userguide/managing-kube-proxy.html#kube-proxy-versions).

Deploy it:
```
export AWS_ACC_4_AWS_IMAGES="602401143452" # usually it stays the same
export AWS_REGION="eu-central-1"
export NEW_KUBEPROXY_VERSION="1.18.8-eksbuild.1" # recommended one for current K8S version

kubectl set image daemonset.apps/kube-proxy \
     -n kube-system \
     kube-proxy=${AWS_ACC_4_AWS_IMAGES}.dkr.ecr.${AWS_REGION}.amazonaws.com/eks/kube-proxy:v${NEW_KUBEPROXY_VERSION}
```

Verify the pods are up and running.
```
kubectl get pods -n kube-system | grep kube-proxy
```

Verify that daemonset uses necessary image:
```
kubectl get daemonset kube-proxy --namespace kube-system -o=jsonpath='{$.spec.template.spec.containers[:1].image}'

```



3. Check few optional steps here.



### 8.2. (Optional) When add-on is managed by EKS (or needs to be managed by EKS)


#### 8.2.1. Update: Add VPC CNI

[Docs: Add already existing VPC CNI add-on to be managed by EKS](https://docs.aws.amazon.com/eks/latest/userguide/managing-vpc-cni.html#adding-vpc-cni-eks-add-on)

> NOTE: **add** - as it was not present in EKS before this version.
> If you have **not added** the Amazon VPC CNI Amazon EKS add-on, the Amazon VPC CNI add-on is still running on your cluster. 
> You can still update it manually.

To add the `vpc-cni` Amazon EKS add-on using `eksctl`:
```
export AWS_REGION="us-east-1"
export AWS_ACCOUNT_NUM="491792459462"
export EKS_CLUSTER_NAME="my-cluster-eksctl"


eksctl create addon \
    --name vpc-cni \
    --version latest \
    --cluster ${EKS_CLUSTER_NAME} \
    --region ${AWS_REGION} \
    --service-account-role-arn arn:aws:iam::${AWS_ACCOUNT_NUM}:role/<eksctl-my-cluster-addon-iamserviceaccount-kube-sys-Role1-UK9MQSLXK0MW> \
    --force
```








## Post-installation

### Update kubectl
Update `kubectl` tool on:
- local machines of all users;
- CI/CD pipelines (if needed).












