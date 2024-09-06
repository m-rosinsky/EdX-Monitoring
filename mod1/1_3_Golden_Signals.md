# 1-3: Golden Signals of Monitoring

## Learning Objectives

- Desrcibe the four Golden Signals
- Explain the importance of Golden Signals

## What are Golden Signals?

- The gold standard for monitoring a web app's metrics

## What are the Four Golden Signals?

- Latency
- Traffic
- Errors
- Saturation

## Latency

- Time b/w when request is sent and completed
- Both successful and failed requests incur latency, and both are important to be tracked

## Traffic

- How in-demand the service is
- Storage system maybe be queries/sec, websites may be requests/sec

## Errors

- Explicit
    - Request fails (HTTP 500s, connection errors)
- Implicit
    - Request succeeds, but wrong information is delivered
- Policy
    - Request succeeds, but outside the policy window (too long latency)

## Saturation

- Percentage of use of a system
- Compute resource memory
- DB disk usage
