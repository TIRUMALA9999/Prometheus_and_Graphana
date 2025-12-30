# Prometheus + Grafana Monitoring Stack (Docker Compose)

![Docker](https://img.shields.io/badge/Docker-Compose-blue)
![Prometheus](https://img.shields.io/badge/Monitoring-Prometheus-red)
![Grafana](https://img.shields.io/badge/Dashboards-Grafana-orange)



This repository provides a minimal, reproducible **observability stack** using **Prometheus** for metrics scraping and **Grafana** for visualization—bootstrapped with **Docker Compose** and a working Prometheus scrape configuration. It also includes **Node Exporter** to expose host-level metrics.

---

## What this project demonstrates 

- Provisioned a containerized monitoring stack: **Prometheus + Grafana + Node Exporter**
- Configured Prometheus to scrape:
  - Prometheus server metrics (`prometheus:9090`)
  - Node Exporter metrics (`node-exporter:9100`)
- Delivered a repeatable local setup using **Docker Compose** (one command to run)
- Provides a baseline template that can be extended to monitor apps, Docker services, and cloud workloads

**Keywords:** Observability, Monitoring, Metrics, Prometheus, Grafana, Node Exporter, Docker Compose, DevOps, Dashboards, Alerting (extensible).

---

## Repository contents

```
Prometheus_and_Graphana-main/
├─ docker-compose.yml   # Runs Prometheus, Grafana, Node Exporter
└─ prometheus.yml       # Prometheus scrape configuration
```

---

## Architecture

1. **Node Exporter** exposes machine/system metrics at `:9100` (CPU, memory, disk, network).
2. **Prometheus** scrapes metrics every **5 seconds** and stores them in its time-series database.
3. **Grafana** connects to Prometheus as a data source and visualizes metrics via dashboards.

---

## Quickstart (run locally)

### Prerequisites
- Docker Desktop (or Docker Engine) installed

### Start the stack
From the repo root:

```bash
docker compose up -d
```

### Verify services
- Prometheus UI: http://localhost:9090  
- Grafana UI: http://localhost:3000  
- Node Exporter metrics: http://localhost:9100/metrics  

Stop:
```bash
docker compose down
```

---

## Prometheus configuration

Prometheus scrapes metrics every **5 seconds** and collects from:

- **Prometheus**: `prometheus:9090`
- **Node Exporter**: `node-exporter:9100`

You can modify these targets inside `prometheus.yml`.

---

## Grafana login & setup

Grafana runs at: http://localhost:3000

- Username: `admin`
- Password: `admin`  *(set via `GF_SECURITY_ADMIN_PASSWORD` in docker-compose)*

### Add Prometheus as a data source
1. Grafana → **Connections** → **Data sources** → **Add data source**
2. Choose **Prometheus**
3. URL: `http://prometheus:9090`
4. Save & test

### Import a Node Exporter dashboard
A common dashboard is **Node Exporter Full** (Grafana dashboard ID: **1860**).
1. Grafana → **Dashboards** → **New** → **Import**
2. Enter **1860** → Load
3. Select Prometheus data source → Import

---

## Example PromQL queries (for interviews)

- CPU usage (non-idle):
```promql
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

- Memory usage:
```promql
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100
```

- Disk usage:
```promql
100 - ((node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"} * 100) / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"})
```

---

## Points

- Deployed a containerized **Prometheus + Grafana observability stack** using **Docker Compose**, enabling repeatable local monitoring for applications and infrastructure.  
- Configured Prometheus scrape jobs and intervals to collect metrics from both **Prometheus server** and **Node Exporter**, supporting host-level performance visibility (CPU/memory/disk/network).  
- Enabled dashboard-driven monitoring in **Grafana** by wiring Prometheus as a data source and providing a scalable baseline for SRE/DevOps workflows.

---


## Author

**Tirumala Teja Yegineni**  
GitHub: https://github.com/TIRUMALA9999
