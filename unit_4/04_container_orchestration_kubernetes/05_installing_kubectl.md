# Installing Kubectl

1. Check if `Kubectl is installed`

```
kubectl version --client
```

2. if not installed, use the following instruction

## Installing Kubectl - Mac OS (Apple Silicon)

1. Download the latest release
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl"
```

2. Validate Binary
    - Everything is good to go if result is `kubectl: OK`

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | shasum -a 256 --check
```