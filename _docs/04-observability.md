---
title: "Observability"
permalink: /docs/observability/
excerpt: "Monitoring your Kyros platform with Grafana and Loki."
last_modified_at: 2025-03-20
toc: true
---

Kyros includes a complete observability stack with Grafana, Loki, and Promtail for monitoring and troubleshooting your data platform.

## Components

| Component | Purpose | Port |
|-----------|---------|------|
| **Grafana** | Dashboards and alerting | 3002 |
| **Loki** | Log aggregation | 3100 |
| **Promtail** | Log collection | - |

## Enabling Observability

Observability components are included in Level 2+:

```bash
# Enable in .env
INCLUDE_GRAFANA=true
INCLUDE_LOKI=true
INCLUDE_PROMTAIL=true
```

Or deploy Level 2:

```bash
make level-2
```

## Accessing Grafana

Open http://localhost:3002

**Default credentials:** admin / admin

## Pre-configured Dashboards

Kyros includes two pre-configured dashboards:

### Container Logs

View and search logs from all containers:
- Filter by service name
- Full-text search
- Time range selection

### Kyros Overview

Platform health at a glance:
- Service status
- Log volume by service
- Error rate trends

## Viewing Logs

### Via Grafana (Recommended)

1. Open Grafana at http://localhost:3002
2. Go to **Explore** (compass icon)
3. Select **Loki** datasource
4. Enter a query:

```logql
{container_name="dagster"} |= "error"
```

### Common LogQL Queries

**All logs from a service:**
```logql
{container_name="superset"}
```

**Errors only:**
```logql
{container_name=~".+"} |~ "error|Error|ERROR"
```

**Specific time range with keyword:**
```logql
{container_name="postgres"} |= "connection"
```

### Via CLI

```bash
# All services
make logs

# Specific service
docker compose logs -f dagster

# Last 100 lines
docker compose logs --tail 100 superset
```

## Alerting

Kyros includes pre-configured alert rules:

| Alert | Condition | Severity |
|-------|-----------|----------|
| High Error Rate | >50 errors in 5 min | Warning |
| PostgreSQL Errors | >5 errors in 5 min | Critical |
| Dagster Pipeline Failure | Any failure | Warning |
| Spark Job Errors | >10 errors in 10 min | Warning |
| Container Restarting | >20 restarts in 5 min | Warning |
| MinIO Storage Errors | >5 errors in 5 min | Critical |

### Configuring Notifications

Edit contact points in Grafana or update the provisioning files.

**Slack notifications:**
```yaml
# In docker/grafana/provisioning/alerting/contactpoints.yml
- uid: kyros-slack
  type: slack
  settings:
    url: "${SLACK_WEBHOOK_URL}"
    recipient: "#alerts"
```

**Email notifications:**
```yaml
- uid: kyros-email
  type: email
  settings:
    addresses: "team@example.com"
```

## Adding Custom Dashboards

### Via Grafana UI

1. Create dashboard in Grafana
2. Save and export as JSON
3. Place in `docker/grafana/provisioning/dashboards/json/`

### Via Provisioning

Add JSON files to `docker/grafana/provisioning/dashboards/json/` and restart Grafana.

## Log Retention

Default retention is 744 hours (31 days). Configure in `docker/loki/loki-config.yml`:

```yaml
limits_config:
  retention_period: 744h
```

## Troubleshooting

### Grafana can't connect to Loki

Check Loki is healthy:
```bash
curl http://localhost:3100/ready
```

### Missing logs

Verify Promtail is running:
```bash
docker compose logs promtail
```

Check Promtail can access Docker socket:
```bash
docker exec promtail ls /var/run/docker.sock
```

### High memory usage

Reduce Loki retention or adjust resource limits in `services/loki.yml`.
