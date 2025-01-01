----------------------------
docker run -d \
  --name prometheus \
  -p 9090:9090 \
  -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus


-------------------
docker run -d \
  --name grafana-image-renderer \
  -p 3001:3001 \
  --link grafana \
  bitnami/grafana-image-renderer

-----------------------
