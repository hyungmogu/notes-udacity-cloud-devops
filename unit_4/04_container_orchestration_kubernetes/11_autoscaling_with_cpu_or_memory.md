# Autoscaling with CPU or Memory

<img src="https://user-images.githubusercontent.com/6856382/219992837-b21731f7-dd06-482e-87b7-d55c667c9846.png">

## Horizontal Auto Scaler

- is a killer feature of Kubernetes
- has the ability to set-up auto-scaling
    - auto-scaling is based on CPU utilization, memory, or custom metrics defined in the Kubernetes metric server 
- has the ability to customize how it scales and whether it scales by CPU, Memory, or both

### Algorithm

```
newNumPods = ceil(currentNumPods * (currentMetric / desiredMetric))
```

### Syntax

```
kubectl scale {deployment name} --replicas={desired number of replicas}
```

#