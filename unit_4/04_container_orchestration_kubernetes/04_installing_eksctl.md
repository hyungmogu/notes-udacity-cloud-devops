# Installing eksctl

- EKSCTL greatly simplifies EKS cluster generation
- Auto-generates necessary AWS resources
    1. VPC 
        - a virtual network defined by a range of IP addresses allocated to you.
    2. Subnets 
        - a subnet is a subset of the VPC (IP addresses) in your desired availability zone (data center)
    3. Nodegroups 
        - logical group of worker nodes. Note that each node is a Virtual Machine (VM)
    4. Deciding the AMI and Type of the worker nodes. AMI defines the underlying OS, the default is Linux with supporting packages. Type defines the hardware capacity.
    5. Kubernetes API endpoint

## Installation Instruction

### Linux

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

### Mac OS
```
# Check Homebrew 
brew --version
# If you do not have Homebrew installed - https://brew.sh/ 
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Install eksctl
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```
