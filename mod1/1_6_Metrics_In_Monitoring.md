# 1-6: Types of Metrics in a Monitoring System

## Learning Objectives

- Identify important metrics to track
- Describe factors that determine which metrics you should monitor

## Important Metrics to Track

- Host-based
- Application
- Network and connectivity
- Server pool

### Host-based Metrics

- CPU, memory, disk space, processes, etc
- Evaluates health and performance of individual pieces of hardware

### Application

- Units that depend on resources
- Indicates applications health, performance, or load
    - Error and success rates
    - Service failures and restarts
    - Performance and latency of responses
    - Resource usage

### Network and Connectivity

- Indicators of outward-facing availability
- Signs that services are accessible for any systems that span more than one machine
    - Connectivity
    - Error rates and packet loss
    - Latency
    - Bandwidth utilization

### Server Pools

- Higher-level extrapolations of host-based metrics, but based on homogenous servers rather than individual machine components
- Summarizes health of a collection of servers

## External Dependency Metrics

- Tracks system interactions with external system that your app is dependent on
    - i.e. third-party payment system
- Metrics include:
    - Service status and availability
    - Success and error rates
    - Run rate and operational costs
    - Resource exhaustion
