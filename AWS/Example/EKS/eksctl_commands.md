# WHAT

[Docs](https://eksctl.io/)

eksctl is a simple CLI tool for creating and managing clusters on EKS - Amazon's managed Kubernetes service for EC2. 
It is written in Go, uses CloudFormation, was created by Weaveworks



# Addons

[Docs](https://eksctl.io/usage/addons/)

Starting from `1.18`, EKS contains following 3 default `add-ons`:
- kube-proxy
- aws-node
- coredns


## List

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
