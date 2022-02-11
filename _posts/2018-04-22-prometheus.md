---
layout: post
title: "Prometheus"
date: 2018-04-22 22:22:22
tags: [tutorials, Prometheus, 2018]
---

#### Prometheus
- for live reload either enable `--web.enable-lifecycle` when starting up and then `curl -X POST localhost:9090/-/reload` _or_
- `kill -1 $prometheus_PID` send SIGHUP

##### Configuration
- relabel instances (aka remove ports) - in main Prometheus YAML file

```yaml
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

```yaml
rule_files:
    - "rules/test.rules"
```

- the `test.rules` file

```yaml
groups:
- name: host
  rules:

  - alert: idle_below_30pct
    expr:  (100 * (1 - avg by(job)(irate(node_cpu{mode='idle'}[1m])))) < 30
    annotations:
      summary: "Instance {{ $labels.instance }} CPU usage is dangerously high"
      description: "{{ $labels.instance }} is using a LOT of CPU. CPU usage is {{ humanize $value}}%."
    labels:
      severity: warning

  - alert: low_disk_space_root
    expr: node_filesystem_avail{mountpoint="/"} < 50_000_000_000
    annotations:
        summary: "Instance {{ $labels.instance }} disk {{ $device }} is low on space"
        description: "Description disk {{ $labels.instance }} is at {{ humanize $value }}B"
    labels:
        group: storage
        severity: critical
```

- Alert configuration in Alertmanager

```yaml
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
