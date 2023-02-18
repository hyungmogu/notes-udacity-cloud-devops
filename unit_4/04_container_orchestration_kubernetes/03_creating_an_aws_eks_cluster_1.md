# Creating an AWS EKS Cluster - Web Console

## Prerequisites

1. Default VPC and subnets
    - Components:
        - VPC
        - Availability Zone
        - Subnets
        - Internet gateway
        - Route table 

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
    - Purpose:
        - To log into EC2.
    - How to generate SSH Keypair
        - https://console.aws.amazon.com/ec2/v2/home?
        - EC2 Service -> Network & Security -> Key Pairs
    - Note:
        - .ppk format is used by windows users
        - .pem format is used by Linux / Mac Users


## Create an EKS Cluster
- EKS cluster is comprised of:
    1. **Control Plane**
        - has nodes running the Kubernetes software
        - runs in AWS-owned accounts
    2. **Data Plane**
        - made up of worker nodes
        - runs in customer account

- Steps:
    1. **Cluster Configuration**
    2. **Specify Networking**
    3. **Configure Logging**