# Monitoring Logging and Debugging with Prometheus

1. Prometheus integrates very well with Kubernetes

## Alerting

1. Consider [normal distribution](https://en.wikipedia.org/wiki/Normal_distribution), [six sigma](https://en.wikipedia.org/wiki/Six_Sigma_), [68-95-99.7 rule](https://en.wikipedia.org/wiki/68%E2%80%9395%E2%80%9399.7_rule)

2. Design a process that alerts senior engineers when events are greater than three standard deviations from the mean and write up how the alerts should work
    - Who should get a page when an event is more significant than three standard deviants from the mean?
    - Should there be a backup person who gets alerted if the first person doesn’t respond within five minutes?
    - Should an alert wake up a team member at one standard deviation? What about two?

## Prometheus

1. To install prometheus, please refer to the following tutorial:
    - Also included in `/unit_3/monitor_and_logging`
    - https://codewizardly.com/prometheus-on-aws-ec2-part1/
    - https://codewizardly.com/prometheus-on-aws-ec2-part2
    - https://codewizardly.com/prometheus-on-aws-ec2-part4/

#