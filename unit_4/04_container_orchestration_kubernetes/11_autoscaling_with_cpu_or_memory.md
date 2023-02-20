# Autoscaling with CPU or Memory

## Horizontal Auto Scaler

- is a killer feature of Kubernetes
- is the ability to set-up auto-scaling
    - auto-scaling is based on CPU utilization, memory, or custom metrics defined in the Kubernetes metric server 

### Algorithm

```
newNumPods = ceil(currentNumPods * (currentMetric / desiredMetric))
```

### Syntax

```
kubectl scale {deployment name} --replicas={desired number of replicas}
```

#