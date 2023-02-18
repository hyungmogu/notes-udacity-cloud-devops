# Creating EKS Cluster and Deploying App

- The difference between `eksctl` and `kubectl` is that:
    - `eksctl` is used to create/delete/edit a cluster
    - `kubectl` is used to interact with the cluster

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
- One way to view is via [Cloud Formation Console](https://us-east-2.console.aws.amazon.com/cloudformation/)
- Another way is via CLI
```
eksctl utils describe-stacks --region=us-east-2 --cluster=eksctl-demo [--profile <profile-name>]
```

3. View Details
- Details of newly created cluster can be observed by 
    - Only valid after cloudformation status if `CREATE_COMPLETE`
```
eksctl get cluster --name=eksctl-demo --region=us-east-2 [--profile <profile-name>]
```

- Health of cluster nodes can be obtained by using

```
kubectl get nodes  [--profile <profile-name>]
```
#