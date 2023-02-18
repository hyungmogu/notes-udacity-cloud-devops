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

4. Deploy Sample App
- a. Download app

```
# Assuming you have already cloned the course repo as
git clone git clone https://github.com/udacity/DevOps_Microservices.git
# Move to the exercise folder if you want to write Dockerfile from scratch
cd DevOps_Microservices/Lesson-3-Containerization/python-helloworld
```
- b. Deploy docker image to docker hub

```
# Ensure Docker Desktop is running locally
docker --version
# Build an image using the Dockerfile in the current directory
docker build -t python-helloworld .
docker images
# Run a container
docker run -d -p 5000:5000 python-helloworld
# Check the output at http://localhost:5000/ or http://0.0.0.0:5000/ or http://127.0.0.1:5000/
docker ps
# Now, stop the container.
# Tag locally before pushing to the Dockerhub
# We have used a sample Dockerhub profile /sudkul
# Replace sudkul/ with your Dockerhub profile
docker tag python-helloworld sudkul/python-helloworld:v1.0.0
docker images
# Log into the Dockerhub from your local terminal
docker login
# Replace sudkul/ with your Dockerhub profile
docker push sudkul/python-helloworld:v1.0.0
# Check the image in your Dockerhub online at https://hub.docker.com/repository/docker/sudkul/python-helloworld
```

- c. Deploy kubernetes cluster once docker image is publically available

```
# Assuming the Kubernetes cluster is ready
kubectl get nodes
# Deploy an App from the Dockerhub to the Kubernetes Cluster
kubectl create deploy python-helloworld --image=sudkul/python-helloworld:v1.0.0
# See the status
kubectl get deploy,rs,svc,pods
# Port forward 
kubectl port-forward pod/python-helloworld-84857d9565-2598m --address 0.0.0.0 5000:5000
```

- d. Delete the cluster
    - You can delete either using
        1. cloudformation
        2. eksctl
    - Above is done to avoid leaving dangling resources
    - Using `eksctl`, it's

```
eksctl delete cluster --region=us-east-2 --name=eksctl-demo [--profile <profile-name>]
```