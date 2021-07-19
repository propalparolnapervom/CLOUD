# EKSCTL
It's about how to create an EKS cluster with 1 command - so it doesn't give an extended understanding what's going on under the hood.

# DOCUMENTATION

[Getting started with EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

# INSTALL EKS

## Prerequisites

- `kubectl` – A command line tool for working with Kubernetes clusters. This guide requires that you use version 1.20 or later. For more information, see Installing kubectl.

- `eksctl` – A command line tool for working with EKS clusters that automates many individual tasks. This guide requires that you use version 0.56.0 or later. For more information, see The eksctl command line utility.

- `Required IAM permissions` – Create IAM user, that has right to create EKS and all this components. Configure access for that user from your local machine (via `~/.aws/credentials`, for example).

> NOTE: If EKS cluster is created under one IAM user and you'll try to connect under other user - it won't work, as by default only creation user is configured as the one who can connect to the EKS cluster (`aws-auth` configmap in the Kubernetes cluster).
