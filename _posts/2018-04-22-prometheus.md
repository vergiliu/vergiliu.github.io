---
layout: post
title: "Prometheus"
date: 2018-04-22 22:22:22
tags: [tutorials, Prometheus, 2018]
---
#### Prometheus
##### Configuration

- relabel instances (aka remove ports) - in main Prometheus YAML file
```YAML
scrape_configs:
    - job_name: 'prometheus'
      static_configs:
          - targets: ['host:port']
      relabel_configs:
          - source_labels: [__address__]
            target_label:  instance
            regex: ^(localhost):9100*
            replacement: mac-${1}
          - source_labels: [__address__]
            target_label:  instance
            regex: ^.*:9091
            replacement: pushgw
```

#### Alerting (for version 2.x)
- Alert rules configuration - in main Prometheus YAML file
```YAML
rule_files:
    - "rules/test.rules"
```
- the `test.rules` file
```YAML
groups:
- name: host
  rules:

  - alert: test_new_cpu
    expr:  (100 * (1 - avg by(job)(irate(node_cpu{mode='idle'}[1m])))) > 20
    annotations:
      summary: "Instance {{ $labels.instance }} CPU usage is dangerously high"
      description: "This device's CPU usage has exceeded the threshold with a value of {{ $value }}."
    labels:
      severity: warning
```
- Alert configuration in Alertmanager
```YAML
route:
    receiver: default
    routes:
        - match:
            alertname: ALL
          repeat_interval: 1m
          receiver: all
receivers:
- name: default
- name: ALL
```
