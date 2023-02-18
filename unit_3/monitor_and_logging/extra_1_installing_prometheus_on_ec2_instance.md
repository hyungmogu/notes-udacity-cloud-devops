# Installing prometheus on aws ec2 instance

## Instruction

1. ssh into ec2 server

2. run the following commands to
    - a. create a different user than root to run specific services
    - b. create a directory to host Prometheus configuration and another one to host its data

```
sudo useradd --no-create-home prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

3. install Prometheus

```
wget https://github.com/prometheus/prometheus/releases/download/v2.19.0/prometheus-2.19.0.linux-amd64.tar.gz
tar xvfz prometheus-2.19.0.linux-amd64.tar.gz

sudo cp prometheus-2.19.0.linux-amd64/prometheus /usr/local/bin
sudo cp prometheus-2.19.0.linux-amd64/promtool /usr/local/bin/
sudo cp -r prometheus-2.19.0.linux-amd64/consoles /etc/prometheus
sudo cp -r prometheus-2.19.0.linux-amd64/console_libraries /etc/prometheus
rm -rf prometheus-2.19.0.linux-amd64.tar.gz prometheus-2.19.0.linux-amd64
```

4. make prometheus to monitor itself
    - this is for a proof of concept

/etc/prometheus/prometheus.yml
```
global:
  scrape_interval: 15s
  external_labels:
    monitor: 'prometheus'

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

5. Make prometheus available as a service
  - every time system is rebooted, prometheus starts with the OS

/etc/systemd/system/prometheus.service
```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

6. Change permission of directories

```
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /var/lib/prometheus

```

7. configure systemd

```
sudo systemctl daemon-reload
sudo systemctl enable prometheus
```

## Configuring Prometheus for AWS Service

1. Create IAM role with 
  - Programmatic access
  - `AmazonEC2ReadOnlyAccess` from `Attach Existing Policies` tab

2. log into prometheus server

```
ssh -i prometheus.pem ubuntu@ec2-3-17-28.53.us-east-2.compute.amazonaws.com
```

3. edit `/etc/prometheus/prometheus.yml` and add created access key calues


```
global:
  scrape_interval: 1s
  evaluation_interval: 1s

scrape_configs:
  - job_name: 'node'
    ec2_sd_configs:
      - region: us-east-1
        access_key: PUT_THE_ACCESS_KEY_HERE
        secret_key: PUT_THE_SECRET_KEY_HERE
        port: 9100
```

4. Restart Prometheus service

```
sudo systemctl restart prometheus
```

5. Check it out by accessing 


## References

1. https://codewizardly.com/prometheus-on-aws-ec2-part1/