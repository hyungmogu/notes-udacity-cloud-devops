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
    - Name:
        - UdacityCapStoneKubernetes
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
        - URL:
            - https://console.aws.amazon.com/eks/home
            - EKS service → Amazon EKS → Clusters → Create Cluster
        - Name:
            - myEKSCluster
        - Version:
            - Default (check version online)
        - Cluster Service Role:
            - myEKSClusterRole (IAM role generated from `Prerequisites` step)
    2. **Specify Networking** 
        - Continues from step 1
        - Under `Networking Tab` (Or can be cloudformation)
            - Set `VPC`
            - Set Subnets
            - Set Security Group
            - Set cluster endpoint to public
    3. **Configure Logging**
        - Accept defaults
        - Wait until cluster shows active state (on EKS)

## Creating a Node Group
1. Click `myEKSCluster` on EKS Cluster page
2. Click Configuration → Compute → Add Node Group
3. Configure node group
    - Name:
        - myNodeGroup
    - Node IAM Role:
        - myEKSWorkerNodeRole
    - Node Group compute and scaling configuration 
        - AMI Type: Amazon Linux 2 (AL2_x86_64)
        - Capacity Type: On-Demand
        - Instance Type: t3.micro
        - Disk Size: 20 GiB
        - Scaling Configuration
            - Min size: 2
            - Max size: 2
            - Desired size: 2
    - Node Group network configuration
        - Choose subnets selected for `myEKSCluster`
    - SSH Keypair
        - Select `UdacityCapStoneKubernetes`