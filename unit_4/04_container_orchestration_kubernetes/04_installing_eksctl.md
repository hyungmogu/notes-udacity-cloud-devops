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

- Are all found [here](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

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

- Add additional permission, when facing permission error

```
sudo chown -R $(whoami) /usr/local/<directory_name>
```

### Windows (Powershell)

```
# Install Chocolatey. Refer to the https://chocolatey.org/install  for detailed steps
Set-ExecutionPolicy AllSigned 
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
# Exit and re-run Powershell as an Admin
chocolatey install eksctl
# Verify
choco -?
```

### How it Works

#### 1. Create a basic cluster

```
eksctl create cluster
```

- Creates with default parameters
    1. An auto-generated name
    2. Two m5.large worker nodes. Recall that the worker nodes are the virtual machines, and the m5.large type defines that each VM will have 2 vCPUs, 8 GiB memory, and up to 10 Gbps network bandwidth.
    3. Use the [Linux AMIs](https://docs.aws.amazon.com/eks/latest/userguide/eks-optimized-ami.html) as the underlying machine image
    4. Your default region
    5. A dedicated VPC

```
eksctl create cluster --name myCluster --nodes=4
```
- Creates default Kubernetes cluster with some (here, `name` and `nodes`) customized

#### 2. Create an advanced cluster

```
eksctl create cluster --config-file=<path>
```
- Example is given [here](https://eksctl.io/)

#### 3. List the details

```
eksctl get cluster [--name=<name>][--region=<region>]

```
- Lists details about existing cluster

#### 4. Delete a cluster

```
eksctl delete cluster --name=<name> [--region=<region>]
```
- Deletes the cluster as well as all the associated resources