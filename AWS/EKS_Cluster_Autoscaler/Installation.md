## Documentation

[How Cluster Autoscaler works on EKS](https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html)

This topic describes how you can deploy the Cluster Autoscaler to your Amazon EKS cluster and configure it to modify your Amazon EC2 Auto Scaling groups.


## Pre-installation

### 1. IAM OIDC provider

[Docs: Work with IAM OIDC](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html)

Determine whether you have an existing `IAM OIDC` provider for your EKS cluster.
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"

aws eks describe-cluster --name ${EKS_CLUSTER_NAME} --query "cluster.identity.oidc.issuer" --output text

  https://oidc.eks.eu-central-1.amazonaws.com/id/<OIDC_NUM>
```

Make sure that `IAM OIDC`, which is EKS cluster configured to use (from output above), exists
```
aws iam list-open-id-connect-providers | grep <OIDC_NUM>

  "Arn": "arn:aws:iam::<ACC_NUM>:oidc-provider/oidc.eks.eu-central-1.amazonaws.com/id/<OIDC_NUM>"
```
 If `IAM OIDC` arn was found, check is finished successfully. Go to the next step.
 
 If `IAM OIDC` was not found, create it.
 
 
### 2. Auto Scaling Groups tags 
 
Make sure that `Auso Scaling Grougps` for `Node groups` of your EKS cluster have following tags.
```
Key	                                                Value
k8s.io/cluster-autoscaler/<cluster-name>	        owned
k8s.io/cluster-autoscaler/enabled	                TRUE
```


## IAM policy and role

Create an IAM policy that grants the permissions that the Cluster Autoscaler requires to use an IAM role.

1. Create an IAM policy.

Create `cluster-autoscaler-policy.json` file:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

Create the policy from the file above:
```
export AUTOSCALER_IAMPOLICY_NAME="AmazonEKSClusterAutoscalerPolicy"

aws iam create-policy \
    --policy-name ${AUTOSCALER_IAMPOLICY_NAME} \
    --policy-document file://cluster-autoscaler-policy.json
```


2. Create an IAM role and attach an IAM policy to it

Create serviceaccount "kube-system/cluster-autoscaler" and IAM role for it, with attached policy from above.
```
export EKS_CLUSTER_NAME="my-cluster-eksctl"
export AWS_ACC_NUM="491792459462"
export AUTOSCALER_IAMPOLICY_NAME="AmazonEKSClusterAutoscalerPolicy"

eksctl create iamserviceaccount \
  --cluster=${EKS_CLUSTER_NAME} \
  --namespace=kube-system \
  --name=cluster-autoscaler \
  --attach-policy-arn=arn:aws:iam::${AWS_ACC_NUM}:policy/${AUTOSCALER_IAMPOLICY_NAME} \
  --override-existing-serviceaccounts \
  --approve
```




## Deploy the Cluster Autoscaler

1. Deploy the Cluster Autoscaler.

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
```

See result:
```
kubectl describe serviceaccount -n kube-system cluster-autoscaler
```

2. Annotate the cluster-autoscaler service account with the ARN of the IAM role that you created previously.

> NOTE: Seems, like this is already done by `eksctl create iamserviceaccount` run
```
export AWS_ACC_NUM="491792459462"
export AUTOSCALER_IAMROLE_NAME="eksctl-my-cluster-eksctl-addon-iamserviceacc-Role1-CCMRIINGHHIY"

kubectl annotate serviceaccount cluster-autoscaler \
  -n kube-system \
  eks.amazonaws.com/role-arn=arn:aws:iam::${AWS_ACC_NUM}:role/${AUTOSCALER_IAMROLE_NAME}
```

3. Patch the deployment to add the `cluster-autoscaler.kubernetes.io/safe-to-evict` annotation to the Cluster Autoscaler pods with the following command.
```
kubectl patch deployment cluster-autoscaler \
  -n kube-system \
  -p '{"spec":{"template":{"metadata":{"annotations":{"cluster-autoscaler.kubernetes.io/safe-to-evict": "false"}}}}}'

kubectl get deployment -n kube-system cluster-autoscaler -o yaml | grep "safe-to-evict"
```























