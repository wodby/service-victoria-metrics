# VictoriaMetrics on Wodby

What Wodby sets up for single-node VictoriaMetrics on this service. It runs from the official `victoriametrics/victoria-metrics` image.

- It listens on port 8428, which is private. Other services in the environment reach it at `http://<VictoriaMetrics service name>:8428`.
- The same port serves the Prometheus-compatible query API (`/api/v1/query`, `/api/v1/query_range`), which Grafana uses with a Prometheus data source, Prometheus remote write (`/api/v1/write`) and the built-in interface (`/vmui`).
- No authentication is configured.
- The `data` volume is mounted at `/victoria-metrics-data`, the storage path given in the container arguments. Retention is the upstream default, since the manifest sets none.
- The manifest passes only the storage path and the listen address as flags and has no settings, links or config files. Environment variables are not read as flags, because `-envflag.enable` is not passed. No scrape configuration is set: metrics arrive only when something writes them.
- `/-/ready` and `/-/healthy` on port 8428 are the readiness and liveness endpoints.
