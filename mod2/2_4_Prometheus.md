# 2-4: Introduction to Prometheus

## Learning Objectives

- Describe what Prometheus is used for
- Explain the Prometheus architecture and how it works
- Summarize the benefits and features of Prometheus

## What is Prometheus?

- Open-source monitoring and alerting solution by SoundCloud
- Monitors servers, VMs, apps, and DBs
- Provides detailed, actionable metrics

## How does Prometheus Work?

- Automatic discovery of services
- Manually define targers
- Prometheus sends HTTP requests from HTTP/S endpoints
- Scrapes realtime metrics
- Organizes and stores collected data in TSDB
- Allows queries using PromQL
- Alerting

## Application Instrumentation

- Developers add instrumentation that produces metrics
- Requires client libraries
- Devs can also manually define metrics in source code (<b>direct instrumentation</b>)

## Client Libraries

- Prometheus includes official libraries for:
    - Python
    - Go
    - Ruby
    - etc.
- 3rd-party libraries support more languages

## Exporter

- Deployed beside the target app
- Intakes requests from Prometheus and gathers data
- Transforms data to correct format
- Returns data to Prometheus

## Platform Support

- Native support for some services, meaning no monitoring agents are required
- Examples include:
    - Kubernetes
    - etcd
    - SkyDNS
    - etc.

## Alertmanager

- A flexible metrics collection and alerting tool that can be combined with Prometheus
- When an alert condition is detected, Prom sends to Alertmanager
