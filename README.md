Author: Aishwarya Ganesh

Overview

This project sets up a basic observability stack for monitoring system metrics using Node Exporter, Prometheus, and Grafana on WSL2/Docker.
It allows tracking CPU, memory, and disk usage in real time.

Features

Node Exporter: Exposes system metrics (CPU, memory, disk)

Prometheus: Scrapes metrics from Node Exporter

Grafana: Visualizes metrics in dashboards

Setup

Start Node Exporter:

docker run -d --name node-exporter -p 9100:9100 --restart unless-stopped prom/node-exporter

Start Prometheus: Configure prometheus.yml to scrape Node Exporter:

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['host.docker.internal:9100']

Then run:

docker run -d --name prometheus -p 9090:9090 -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

Start Grafana:

docker run -d --name grafana -p 3000:3000 grafana/grafana

Create dashboards in Grafana

Add panels for CPU, memory, and disk using Prometheus queries:

CPU: node_cpu_seconds_total{mode="idle"}

Memory: 100 * (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes

Disk: 100 * (node_filesystem_size_bytes - node_filesystem_avail_bytes) / node_filesystem_size_bytes

Access dashboards:

Grafana: http://localhost:3000

Notes

This stack is suitable for learning and small-scale monitoring.

All components run via Docker on WSL2, making it portable and easy to reset.


