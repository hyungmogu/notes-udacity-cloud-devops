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

3. Configure Alertmanager as a service

/etc/systemd/system/alertmanager.service
```
[Unit]
Description=Alert Manager
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/prometheus/alertmanager.yml \
  --storage.path=/var/lib/alertmanager

Restart=always

[Install]
WantedBy=multi-user.target
```

4. Configure Systemd

```
sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager
```

5. Generate App password from an email platform of choice
    - a. Go to your account: https://myaccount.google.com
    - b. From the left menu select `Security`
    - c. Select the Signing in to Google panel select `App Passwords`
    - d. Make sure the following settings are enabled / disabled
        - i. 2fa Verification is set up for your account
        - ii. 2fa Verification is not set up for security keys only
        - iii. Your account is not through work, school, or other organization.
        - iv. You’ve not turned on Advanced Protection for your account.

    - e. Create a new App password
    - f. Choose a custom name for the App password.
    - g. Save the App password in a safe place.

6. Create a rule
    - a rule includes sending an alert if the instance is down for more than 3 minutes

/etc/prometheus/rules.yml
```
groups:
- name: Down
  rules:
  - alert: InstanceDown
    expr: up == 0
    for: 3m
    labels:
      severity: 'critical'
    annotations:
      summary: "Instance  is down"
      description: " of job  has been down for more than 3 minutes."
```

7. Configure Prometheus
    - Change the permission of the directories, files and binaries added to the system

```
sudo chown -R prometheus:prometheus /etc/prometheus
```




#