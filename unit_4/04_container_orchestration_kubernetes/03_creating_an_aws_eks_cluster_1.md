# Creating an AWS EKS Cluster - Web Console

## Prerequisites

1. Default VPC and subnets

2. IAM role for the cluster
    - Role Name:
        1. myEKSClusterRole
    - Trusted Entity:
        1. AWS Service
    - Use Case:
        1. EKS Cluster
    - Required Policy:
        1. AmazonEKSClusterPolicy

3. IAM role for the worker nodes
    - Purpose:
        - To give permission to the `kublet` running on the worker node to make calls to other APIs on your behalf
    - Role Name:
        1. myEKSWorkerNodeRole
    - Trusted Entity:
        1. AWS Service
    - Use Case:
        1. EC2
    - Required Policy:
        1. AmazonEKSWorkerNodePolicy
        2. AmazonEC2ContainerRegistryReadOnly
        3. AmazonEKS_CNI_Policy

4. SSH Keypair


## 