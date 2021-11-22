# Heroku Buildpack for Grafana Cloud Agent

This is an unofficial Heroku buildpack for
[Grafana Cloud Agent](https://github.com/grafana/agent) deployments.

## Usage

### Binary
Automaticly downloads most newest binary that will placed in `bin/grafana-agent`

### Config
Your config file should be placed into the root as `config/grafana-agent.yml`.

The buildpack will substitute any environment variables. Example:

```yaml
---
integrations:
  prometheus_remote_write:
    - basic_auth:
        password: ${GRAFANA_CLOUD_API_KEY}
        username: ${GRAFANA_CLOUD_USERNAME}
      url: https://prometheus-prod-10-prod-us-central-0.grafana.net/api/prom/push

prometheus:
  configs:
    - name: integrations
      remote_write:
        - basic_auth:
            password: ${GRAFANA_CLOUD_API_KEY}
            username: ${GRAFANA_CLOUD_USERNAME}
          url: https://prometheus-prod-10-prod-us-central-0.grafana.net/api/prom/push
      scrape_configs:
        - job_name: integrations/nodejs
          honor_labels: true
          metrics_path: /api/v1/services/metrics
          static_configs:
            - targets:
                - ${APP_DOMAIN}
  global:
    scrape_interval: 60s
  wal_directory: /tmp/grafana-agent-wal

server:
  http_listen_port: 12345
```

