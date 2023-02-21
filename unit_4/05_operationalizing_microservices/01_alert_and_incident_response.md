# Alert and Incident Response

## Tools

1. Prometheus
2. Custom Alerts
3. Stackdriver
4. Weaveworks


## Operationalizing Microservice Overview

1. Application is stored in Git
2. Changes in Git trigger the CD server which then tests and deploys the code to a new environment
    - Environment is configured as Infrastructure as Code (IaC).
3. Microservice which is a containerized service running in Kubernetes has running metrics and instrumentation
4. Load test tool like Locust

<img src="https://user-images.githubusercontent.com/6856382/220151742-dfac8915-e2c6-47ad-b5e4-e80ad5465ba3.png">

#