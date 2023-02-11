# Exporters

1. Prometheus is a `pull` system rather than a `push` system
    - `pull` system knows about the data sources and pulls in their time-series data
    - `push` system does not know about its data sources until they send something

2. Data is pulled from various sources using lightweight agents called `exporters`
    - data is scraped every x seconds
    - lots of prebuild exporters available

## Configuring Node Exporter

1. Create security group for Prometheus if it doesn't exist
    - Make sure the following ports are allowed:
        - a. 22 (for ssh)
        - b. 9093 (prometheus alert manager)
        - c. 9090 (prometheus server)
        - d. 9100 (prometheus node exporter)
2. Create IAM ssh key if it doesnt exist
    - Should have Programmatic access
    - Should have `AmazonEC2ReadOnlyAccess` from `Attach Existing Policies` tab 
2. Create a new EC2 instance for testing.
    - This is second ec2 after setting up prometheus
    - Assign security group to this ec2 instance
3. SSH into and configure the EC2 instance with node_exporter using the instructions in [this](https://codewizardly.com/prometheus-on-aws-ec2-part2) tutorial. 
    - a. should have two ec2 instances, one security group, one ssh key:
        - i. prometheus-server, port 9090
        - ii. prometheus-node-exporter, port 9100

    - b. start a session in the new virtual machine

    ```
    ssh -i <.pem file attached to ec2> ubuntu@<PUBLIC_HOST_FOR_PROMETHEUS_NODE_EXPORTER_INSTANCE>
    ```

    - c. create a user for Prometheus Node Exporter

    ```
    sudo useradd --no-create-home node_exporter
    ```

    - d. Install Node Exporter binaries

    ```
    wget https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
    tar xzf node_exporter-1.0.1.linux-amd64.tar.gz
    sudo cp node_exporter-1.0.1.linux-amd64/node_exporter /usr/local/bin/node_exporter
    rm -rf node_exporter-1.0.1.linux-amd64.tar.gz node_exporter-1.0.1.linux-amd64
    ```

    - e. Create `/etc/systemd/system/node-exporter.service` if it doesnt exist

    ```
    [Unit]
    Description=Prometheus Node Exporter Service
    After=network.target

    [Service]
    User=node_exporter
    Group=node_exporter
    Type=simple
    ExecStart=/usr/local/bin/node_exporter

    [Install]
    WantedBy=multi-user.target
    ```

    - f. Configure systemd

    ```
    sudo systemctl daemon-reload
    sudo systemctl enable node-exporter
    sudo systemctl start node-exporter
    sudo systemctl status node-exporter
    ```

    - g. To configure prometheus server, start a session in prometheus-server ec2 instance

    ```
    ssh -i <.pem for prometheus> ubuntu@<PUBLIC_URL_FOR_PROMETHEUS_SERVER_EC2_INSTANCE>
    ```

    - h. Edit `/etc/prometheus/prometheus.yml` file

    ```
    global:
    scrape_interval: 15s
    external_labels:
        monitor: 'prometheus'

    scrape_configs:

    - job_name: 'node_exporter'

        static_configs:

        - targets: ['ec2-13-58-127-241.us-east-2.compute.amazonaws.com:9100']
    ```

    - i. Restart prometheus instance

    ```
    sudo systemctl restart prometheus
    ```

4. Check. if the prometheus server is running correctly

```
<PUBLIC_URL_PROMETHEUS_SERVER>:9090/targets
```