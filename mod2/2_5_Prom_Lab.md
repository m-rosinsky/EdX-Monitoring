# 2-5: Prometheus Lab

## Objectives

- Deploy and use prometheus

## Download Images

- Use the `node-exporter` image for a sample application
- Use prometheus to monitor sample app

Pulling the images:

```
docker pull bitnami/node-exporter:latest
```

```
docker pull bitnami/prometheus:latest
```

## Creating Sample Application

- Create three nodes of `node-exporter` on a sample network:

```
docker network create monitor
```

```
docker run -d --name node-exporter1 -p 9101:9100 --network monitor bitnami/node-exporter:latest
```

```
docker run -d --name node-exporter2 -p 9102:9100 --network monitor bitnami/node-exporter:latest
```

```
docker run -d --name node-exporter3 -p 9103:9100 --network monitor bitnami/node-exporter:latest
```

- View the running containers:

```
docker ps | grep node-exporter

dd5ae16fb303   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   10 seconds ago       Up 8 seconds        0.0.0.0:9103->9100/tcp, :::9103->9100/tcp   node-exporter3
8bf38f0cd6bd   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   15 seconds ago       Up 13 seconds       0.0.0.0:9102->9100/tcp, :::9102->9100/tcp   node-exporter2
cd93277d624e   bitnami/node-exporter:latest   "/opt/bitnami/node-e…"   About a minute ago   Up About a minute   0.0.0.0:9101->9100/tcp, :::9101->9100/tcp   node-exporter1
```

## Configure and Run Prometheus

- Create the Prometheus Config file

`prometheus.yml`:
```yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. The default is every 1 minute.

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter1:9100']
        labels:
          group: 'monitoring_node_ex1'
      - targets: ['node-exporter2:9100']
        labels:
          group: 'monitoring_node_ex2'
      - targets: ['node-exporter3:9100']
        labels:
          group: 'monitoring_node_ex3'
```

- Launch Prometheus monitor

```
docker run -d --name prometheus -p 9090:9090 --network monitor -v $(pwd)/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml bitnami/prometheus:latest
```

## Open the Prometheus UI

- Navigate to `localhost:9090`:

![Prometheus Dashboard](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0223EN-SkillsNetwork/images/prometheus_ui_light.png)

- View the targets in Status > Targets

![View Targets](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0223EN-SkillsNetwork/images/prometheus_status_light.png)

- Click Graph to return to home page to perform queries

## Execute First Query

- Perform the following query:

```promql
node_cpu_seconds_total
```

![First Query](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0223EN-SkillsNetwork/images/node_cpu_graph.png)

- Apply a filter to get details for only the `node-exporter2` instance:

```promql
node_cpu_seconds_total{instance="node-exporter2:9100"}
```

- Query for connections each node has:

```promql
node_ipvs_connections_total
```

## Stop and Observe

- Stop the `node-exporter` instance:

```
docker stop node-exporter1
```

- Observe in Targets page that host is down:

![Down Host](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0223EN-SkillsNetwork/images/node_down.png)

## Enable Application

- Create a python flask that implements Prometheus Metrics:

```python
from prometheus_flask_exporter import PrometheusMetrics
from flask import Flask

app = Flask(__name__)
metrics = PrometheusMetrics.for_app_factory()
metrics.init_app(app)

@app.route('/')
def root():
    return 'Hello from root!'

@app.route('/home')
def home():
    return 'Hello from home!'

@app.route('/contact')
def contact():
    return 'Contact us!'

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8080)
```

- Create a Dockerfile to containerize the application:

```Dockerfile
FROM python:3.9-slim
RUN pip install Flask prometheus-flask-exporter
WORKDIR /app
COPY pythonserver.py .
EXPOSE 8080
CMD ["python", "pythonserver.py"]
```

- Build

```
docker build -t pythonserver .
```

- Run

```
docker run -d --name pythonserver -p 8081:8080 --network monitor pythonserver
```

## Reconfigure Prometheus

In `prometheus.yml`:

- Create a new job to monitor `pythonserver` on port `8080`

```yaml
# my global config
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter1:9100']
        labels:
          group: 'monitoring_node_ex1'
      - targets: ['node-exporter2:9100']
        labels:
          group: 'monitoring_node_ex2'
      - targets: ['node-exporter3:9100']
        labels:
          group: 'monitoring_node_ex3'

  - job_name: 'monitor-pythonserver'
    static_configs:
      - targets: ['pythonserver:8080']
        labels:
          group: 'monitoring_python'
```

- Restart Prometheus:

```
docker restart prometheus
```

- Check new Target page:

![Pythonserver target](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0223EN-SkillsNetwork/images/prom_python_server.png)

## Monitor Application

- Make requests to sample service using `curl`:

```
curl localhost:8081
curl localhost:8081/home
curl localhost:8081/contact
```

- Use Prometheus UI to query for following metrics:

- `flask_http_request_duration_seconds_bucket`
- `flask_http_request_total`
- `process_virtual_memory_bytes`
