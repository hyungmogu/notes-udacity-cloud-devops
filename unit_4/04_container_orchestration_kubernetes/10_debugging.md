# Debugging

1. In a large scale system, we see
    1. Multiple containers in a pod
    2. Multiple pods in a Kubernetes Node
    3. Multiple Kubernetes nodes in a Kubernetes Cluster

    - Why?
        - 1. One pod could contain web application, and scale it specific to it's needs but it has specific memory and cpu requirements, causing the system to create another one.

        - 2. Another pod in the Kubernetes maybe is for monitoring (Prometheus)

        - 3. Another pod in the Kubernetes cluster maybe is for relational databases or specialized queing system

<img src="https://user-images.githubusercontent.com/6856382/219900552-16cc078b-8e81-40cc-a5af-d0c47523c38a.png"/>

## Debugging - Pod Issues

1. Use `kubectl` get pods to check the names of your running pods
    - Check for status like `Pending` instead of `Running`

2. Use use `kubectl describe pod {POD NAME}` to get more information about the pod

3. From the output of the above command, see `Events` header.
    - This should give `Reason` and `Message` related to failure
    - e.g. FailedScheduling

### Solution

1. `kubectl scale` if related to resource issue
    - Provides necessary resources for the `Pending` pod

## Debugging - Node Issues

1. 