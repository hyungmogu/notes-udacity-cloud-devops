# Install Kubernetes

1. The simplest way is to use `Docker for Windows` or `Docker for Mac` as it already has `kubectl` command

2. The next recommended way is to use cloud shell tool like `AWS cloud 9` or `Google Cloud Shell`

3. The last way is manual install. The procedure is as follows:

Mac OS
```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"
```

Linux
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```

4. Verify if kubernetes is installed correctly by using `kubcectl` command
