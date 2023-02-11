# Installing Prometheus

## Setting Up Prometheus

1. Add Security Group for Prometheus
    - Make sure the following ports are allowed:
        - a. 22 (for ssh)
        - b. 9093 (prometheus alert manager)
        - c. 9090 (prometheus server)
        - d. 9100 (prometheus node exporter)
2. Create a ssh key if it doesn't exist
    - 
3. Launch a new instance in EC2 with port 9090 open to the public.
    - Attach prometheus security group to this instance
4. Log into the instance via SSH.

```
ssh -i <.pem file attached to EC2 Instance> ubuntu@<PUBLIC_HOST_NAME_AWS_CONSOLE>
```

5. create a different user for prometheus
    - for added protection

```
sudo useradd --no-create-home prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

6. install Prometheus

```
wget https://github.com/prometheus/prometheus/releases/download/v2.19.0/prometheus-2.19.0.linux-amd64.tar.gz
tar xvfz prometheus-2.19.0.linux-amd64.tar.gz

sudo cp prometheus-2.19.0.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-2.19.0.linux-amd64/promtool /usr/local/bin/
sudo cp -r prometheus-2.19.0.linux-amd64/consoles /etc/prometheus
sudo cp -r prometheus-2.19.0.linux-amd64/console_libraries /etc/prometheus

sudo cp prometheus-2.19.0.linux-amd64/promtool /usr/local/bin/
rm -rf prometheus-2.19.0.linux-amd64.tar.gz prometheus-2.19.0.linux-amd64
```

7. initialize `/etc/prometheus/prometheus.yml`

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

8. Configure `/etc/systemd/system/prometheus.service`

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

9. change permission of the directories

```
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /var/lib/prometheus
```

10. configure systemd

```
sudo systemctl daemon-reload
sudo systemctl enable prometheus
```

11. Create Node exporter (see 09_exporters.md)

#