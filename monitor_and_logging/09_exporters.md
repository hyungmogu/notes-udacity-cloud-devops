# Exporters

1. Prometheus is a `pull` system rather than a `push` system
    - `pull` system knows about the data sources and pulls in their time-series data
    - `push` system does not know about its data sources until they send something

2. Data is pulled from various sources using lightweight agents called `exporters`
    - data is scraped every x seconds
    - lots of prebuild exporters available

## Configuring Node Exporter

1. Create a new EC2 instance for testing.
2. SSH into and configure the EC2 instance with node_exporter using the instructions in [this](https://codewizardly.com/prometheus-on-aws-ec2-part2) tutorial.
3. Connect again via SSH to your Prometheus server in EC2.
4. Configure Prometheus to discover EC2 instances automatically following this tutorial.
5. View the test EC2 instance in Prometheus.