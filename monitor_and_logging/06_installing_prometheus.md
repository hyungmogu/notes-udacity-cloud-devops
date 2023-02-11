# Installing Prometheus

## Setting Up Prometheus

1. Add Security Group for Prometheus
    - Make sure the following ports are allowed:
        - a. 22 (for ssh)
        - b. 9093 (prometheus alert manager)
        - c. 9090 (prometheus server)
        - d. 9100 (prometheus node exporter)
1. Launch a new instance in EC2 with port 9090 open to the public.
    - Attach prometheus security group to this instance
2. Log into the instance via SSH.

```
ssh -i <.pem file attached to EC2 Instance> ubuntu@<PUBLIC_HOST_NAME_AWS_CONSOLE>
```

3. Download and extract the Prometheus server files.

```
wget https://github.com/prometheus/prometheus/releases/download/v2.19.0/prometheus-2.19.0.linux-amd64.tar.gz
tar xvfz prometheus-2.19.0.linux-amd64.tar.gz
```

4. Start Prometheus.

```
cd prometheus-2.19.0.linux-amd64;
./prometheus --config.files=./prometheus.yml
```

5. Open the instance's hostname or IP address in your browser with port 9090.
    - In chrome, safari, or firefox type the following:

```
<PUBLIC_HOST_NAME_AWS_CONSOLE>:9090/graph
```