# Sending Alert Messages

- `Prometheus` has alot of channels of communication to use for alerts

- Helpful alerting include
    - a. providing a brief description of the problem (easier to understand, the better)
    - b. stack trace and source code if code-related issues occur
    - c. URL if it can be used by engineer to troubleshoot the problem

- NOTE: MAKE SURE TO KEEP ALERTS SIMPLE (ALERT FATIGUE)

## Adding Alert Manager

1. On "Prometheus Server" (not the one that contains node exporter), install alert manager

```
wget https://github.com/prometheus/alertmanager/releases/download/v0.21.0/alertmanager-0.21.0.linux-amd64.tar.gz
tar xvfz alertmanager-0.21.0.linux-amd64.tar.gz

sudo cp alertmanager-0.21.0.linux-amd64/alertmanager /usr/local/bin
sudo cp alertmanager-0.21.0.linux-amd64/amtool /usr/local/bin/
sudo mkdir /var/lib/alertmanager

rm -rf alertmanager*
```

2. Add Alertmanager’s configuration `/etc/prometheus/alertmanager.yml`

/etc/prometheus/alertmanager.yml
```
route:
  group_by: [Alertname]
  receiver: email-me

receivers:
- name: email-me
  email_configs:
  - to: EMAIL_YO_WANT_TO_SEND_EMAILS_TO
    from: YOUR_EMAIL_ADDRESS
    smarthost: smtp.gmail.com:587
    auth_username: YOUR_EMAIL_ADDRESS
    auth_identity: YOUR_EMAIL_ADDRESS
    auth_password: YOUR_EMAIL_PASSWORD
```

