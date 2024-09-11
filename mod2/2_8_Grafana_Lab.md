# 2-8: Monitoring Node Exporter with Prometheus and Grafana

## Objective

- Use `node-exporter` image to mimic sample application
- Monitor sample application with Prometheus and Grafana

## Pull Images

```
docker pull bitnami/node-exporter:latest
```

## Launch Applications

- Create a network

```
docker network create monitor
```

- Stand up first `node-exporter` server

```
docker run -d -p 9101:9100 --name node-exporter1 --network monitor bitnami/node-exporter:latest
```

URL: `https://rosinskymike-9101.theiadockernext-0-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai/`

- Launch two more `node-exporter` servers

```
docker run -d -p 9102:9100 --name node-exporter2 --network monitor bitnami/node-exporter:latest

docker run -d -p 9103:9100 --name node-exporter3 --network monitor bitnami/node-exporter:latest
```

- Verify

```
docker ps | grep node-exporter

d99d1b1bc7c6   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   11 seconds ago       Up 10 seconds       0.0.0.0:9103->9100/tcp, [::]:9103->9100/tcp   node-exporter3
f6dd25237921   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   15 seconds ago       Up 14 seconds       0.0.0.0:9102->9100/tcp, [::]:9102->9100/tcp   node-exporter2
db99ca8ce802   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   About a minute ago   Up About a minute   0.0.0.0:9101->9100/tcp, [::]:9101->9100/tcp   node-exporter1
```

## Download, Configure, and Run Prometheus

- Pull image

```
docker pull bitnami/prometheus:latest
```

- Create Prometheus config file:

`prometheus.yml`:
```yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.

scrape_configs:
  - job_name: 'node'

    static_configs:
      - targets: ['rosinskymike-9101.theiadockernext-0-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai']
        labels:
          group: 'monitoring_node_ex1'
      - targets: ['rosinskymike-9102.theiadockernext-0-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai']
        labels:
          group: 'monitoring_node_ex2'
      - targets: ['rosinskymike-9103.theiadockernext-0-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai']
        labels:
          group: 'monitoring_node_ex3'
```

- Launch Prometheus container

```
docker run --rm --name prometheus -p 9090:9090 --network monitor -v $(pwd)/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml bitnami/prometheus:latest
```

## Open the Prometheus UI

- Check Targets

## Execute First Query

- Perform following query:

```
node_cpu_seconds_total
```

## Start Grafana

- Run grafana instance

```
docker run --name=grafana -dp 3000:3000 --network monitor grafana/grafana
```

- Login

- Click on `Data Sources` to add first data source
- Choose `Prometheus` from list of available options