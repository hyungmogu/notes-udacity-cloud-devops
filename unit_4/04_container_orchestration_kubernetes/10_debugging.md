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

