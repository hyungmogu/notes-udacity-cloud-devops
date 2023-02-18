# Creating EKS Cluster and Deploying App

## Create an EKS Cluster using the EKSCTL utility

1. Create Command

```
eksctl create cluster --name eksctl-demo --region=us-east-2 [--profile <profile-name>]
```     

- Creates one node group containing two m5.large nodes
- Creates two subnets in separate availability zones
    - 2 public
    - 2 private
- Creates two separate CloudFormation stacks 
    - 1. eksctl-eksctl-demo-cluster for the cluster
    - 2. eksctl-eksctl-demo-nodegroup-ng-7f9854c3 for the initial nodegroup

- More is found here: https://eksctl.io/usage/creating-and-managing-clusters/

2. View progress

