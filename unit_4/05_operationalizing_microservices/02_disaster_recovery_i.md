# Disaster Recovery (Important)

- Design for failure is important but often overlooked

# Five Pillars of Well Architected Serverless System

1. Operational Excellence
    - Related to health of serverless application
    - How to understand application health?
        1. Metrics and alerts
            - e.g. Business metrics, customer experience metrics, other metrics
        2. Centralized logging
            - allows unique transaction ideas that narrows down critical failure

2. Reliability
    - Has to do with planning on the fact that failures occur
    - How to build reliable system?
        1. Build retry logic for key service
        2. Build system that can queue work when the service is unavailable
        3. Use highly avilable service that store multiple copies of the data like Amazon S3
        4. Archive key data to services that store immutable backup
        5. Test these backups and validate them on a recurring basis

3. Performance Efficiency
    - How to validate system's performance and efficiency?
        1. loader.io
        2. Apache Bench
        3. Locust

4. Cost Optimization
    - Serverless technologies like AWS Lambda fits into this category
        - is event driven
        - is charged based by usage

5. Security
    - How to protect system?
        1. Using POLP (principle of least privilege) - Giving out privileges they only need
        2. Protect data at rest + in transit

## Summary

IaC + GitOps (or devops) + load testing + logging --> more robust system

#