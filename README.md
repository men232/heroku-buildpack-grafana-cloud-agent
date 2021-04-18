# Heroku Buildpack for Grafana Cloud Agent

This is an unofficial Heroku buildpack for
[Grafana Cloud Agent](https://github.com/grafana/agent) deployments.

## Usage

### Binary
Create a bin folder in the root of your repository. Then download whatever
version of the [Grafana Cloud Agent](https://github.com/grafana/agent) you plan
on using. The linux binary should be named `agent`.

```
$ mkdir bin
$ curl -O -L "https://github.com/grafana/agent/releases/download/v0.4.0/agent-linux-amd64.zip"
$ unzip agent-linux-amd-64.zip
$ mv agent-linux-amd64 bin/agent
$ rm agent-linux-amd-64.zip
```

### Config
Your config file should be placed into the root as `graphana-agent-config.yml`.

The buildpack will substitute any environment variables. Example:

```yaml
---
server:
  http_listen_port: $PORT

prometheus:
  wal_directory: "./"
  global:
    scrape_interval: 5s
  configs:
    - name: agent
      host_filter: false
      scrape_configs:
        - job_name: taylor-swift-metrics
          metrics_path: /metrics
          scheme: https
          static_configs:
            - targets: ['target.taysway.xyz']
      remote_write:
        - url: https://prom.taysway.xyz/api/prom/push
          basic_auth:
            username: $USERNAME
            password: $PASSWORD
```

