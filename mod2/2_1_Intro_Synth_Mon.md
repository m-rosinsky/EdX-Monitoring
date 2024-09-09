# 2-1: Introduction to Synthetic Monitoring

## Learning Objectives

- Define Synthetic Monitoring
- Discuss the Importance of Synthetic Monitoring

## What is Synthetic Monitoring?

- A method used to keep track of how well an application or website is performing
- Scripted recordings simulating user interactions
- Actions recorded and replayed periodically
- Predictive behavior:
    - Understand UX
- Active approach

## Synthetic Monitors

- Use a network of checkpoints operating outside of the website's own servers
- Checkpoints are located in different parts of the network or world

## Synthetic Transaction Monitor

- Measures:
    - Availability (9's)
    - Responsiveness (latency over time)

## How does SM Work?

- Virtual client connects to application
- Virtual clients should be initiated by various OSs and locations around the globe

## SM Process

1. Monitoring system chooses a checkpoint and sends instructions to it
2. Checkpoint initiates contact, checks response, and proceeds
3. Checkpoint reports results and findings to monitoring system
4. If the check results in an error:
    - Monitoring system immediately requests a new test from another checkpoint
    - If the checkpoint reports same error, the system declares the error confirmed
    - System can send out an alert

## Importance of SM

- Monitor complex transactions and business process
    - Login
    - Search
    - Forms
    - Cart items
    - etc.

## RUM vs SM

| RUM | SM |
| --- | --- |
| Passive Monitoring | Active Monitoring |
| Relies on real users | Tracks simulated user transactions |
| Utilizes JS code snippet | Consistent testing by eliminating variables |
| Reports response times, load times, errors, browser, and locations | |
| Tracks and identifies long-term performance | Identifies and resolves immediate problems |

These two approaches work best in conjunction.
