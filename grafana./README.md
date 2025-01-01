# Prometheus and Grafana Image Renderer Setup with Docker

This guide helps you set up **Prometheus** for monitoring and the **Grafana Image Renderer** for rendering graphs using Docker.

---

## Table of Contents

- [Prometheus Setup](#prometheus-setup)
- [Grafana Image Renderer Setup](#grafana-image-renderer-setup)
- [Configuration File](#configuration-file)

---

## Prometheus Setup

Run the following command to start a Prometheus container:

```bash
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```

### Notes:
- Replace `/path/to/prometheus.yml` with the actual path to your Prometheus configuration file.
- Prometheus will be accessible at `http://localhost:9090`.

---

## Grafana Image Renderer Setup

Run the following command to start the Grafana Image Renderer container:

```bash
docker run -d \
  --name grafana-image-renderer \
  -p 3001:3001 \
  --link grafana \
  bitnami/grafana-image-renderer
```

### Notes:
- This container is designed to work alongside a Grafana instance.
- Replace `grafana` with the name of your Grafana container if it's named differently.

---

## Configuration File

### Prometheus Configuration (`prometheus.yml`)

Here’s a basic example of the Prometheus configuration file:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
```

Make sure to adjust the `targets` field to include the endpoints of the services you want to monitor.

---

## Access and Usage

1. **Prometheus:** Open your web browser and go to `http://localhost:9090` to access the Prometheus web interface.
2. **Grafana Image Renderer:** The image renderer will be available at `http://localhost:3001` and integrates with Grafana to generate graphs.

---
