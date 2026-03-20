---
title: "Components Reference"
permalink: /docs/components/
excerpt: "All available Kyros components and their configurations."
last_modified_at: 2025-03-20
toc: true
---

Kyros includes a curated set of open-source components for data engineering. Each component can be enabled or disabled independently.

## Storage & Data

### PostgreSQL
**Port:** 5432 | **Level:** 1+

Production-grade relational database for OLTP workloads, metadata storage, and application data.

```yaml
INCLUDE_POSTGRES=true
POSTGRES_USER=kyros
POSTGRES_PASSWORD=kyros_dev
```

**Use cases:**
- Application database
- Dagster metadata storage
- Superset metadata
- Hive metastore backend

### MinIO
**Port:** 9001 (Console), 9000 (API) | **Level:** 2+

S3-compatible object storage for data lake architecture.

```yaml
INCLUDE_MINIO=true
MINIO_ROOT_USER=kyros
MINIO_ROOT_PASSWORD=kyros_dev
```

**Use cases:**
- Raw data storage
- Parquet/Delta Lake files
- Spark checkpoints
- Artifact storage

### Kafka
**Port:** 9092 | **Level:** 4+

Distributed event streaming platform for real-time data pipelines.

```yaml
INCLUDE_KAFKA=true
```

**Use cases:**
- Real-time data ingestion
- Event-driven architecture
- Stream processing input

## Processing & Query

### Spark
**Ports:** 8080 (Master UI), 7077 (Master), 4040+ (Workers) | **Level:** 3+

Distributed data processing engine for large-scale batch and streaming workloads.

```yaml
WORKERS=2
SPARK_WORKER_MEMORY=2G
SPARK_EXECUTOR_MEMORY=2G
SPARK_WORKER_CORES=2
```

**Use cases:**
- Large-scale ETL
- Machine learning pipelines
- Data lake processing

### Trino
**Port:** 8082 | **Level:** 3+

Federated SQL query engine that can query multiple data sources with a single query.

```yaml
INCLUDE_TRINO=true
```

**Use cases:**
- Query across PostgreSQL, MinIO, and other sources
- Ad-hoc analytics
- Data virtualization

### Flink
**Port:** 8081 | **Level:** 4+

Stream processing framework for real-time analytics.

```yaml
INCLUDE_FLINK=true
```

**Use cases:**
- Real-time analytics
- Complex event processing
- Stream-to-batch integration

### dbt
**Level:** 0+

SQL transformation tool for data modeling and analytics engineering.

```yaml
INCLUDE_DBT=true
```

**Use cases:**
- Data transformations
- Data modeling
- Documentation generation

## Orchestration & BI

### Dagster
**Port:** 3000 | **Level:** 1+

Modern data orchestration platform with asset-based pipelines.

```yaml
INCLUDE_DAGSTER=true
DAGSTER_WORKSPACE=kyros
```

**Use cases:**
- Pipeline scheduling
- Data asset management
- Observability

### Superset
**Port:** 8088 | **Level:** 1+

Business intelligence platform for data exploration and visualization.

```yaml
INCLUDE_SUPERSET=true
SUPERSET_ADMIN=admin
SUPERSET_PASSWORD=admin
```

**Use cases:**
- Dashboards
- Ad-hoc queries
- Data exploration

## Development Tools

### JupyterLab
**Port:** 8888 | **Level:** 2+

Interactive notebook environment for data exploration and prototyping.

```yaml
INCLUDE_JUPYTERLAB=true
```

**Use cases:**
- Data exploration
- Prototyping
- Machine learning development

### Code Server
**Port:** 8083 | **Level:** 2+

VS Code in the browser for remote development.

```yaml
INCLUDE_CODE_SERVER=true
```

**Use cases:**
- Remote development
- Pipeline editing
- Configuration management

### CloudBeaver
**Port:** 8978 | **Level:** 1+

Web-based database management tool.

```yaml
INCLUDE_CLOUDBEAVER=true
```

**Use cases:**
- Database browsing
- SQL queries
- Schema exploration

### SQLPad
**Port:** 3001 | **Level:** 1+

Lightweight SQL editor with sharing capabilities.

```yaml
INCLUDE_SQLPAD=true
```

**Use cases:**
- Quick queries
- Query sharing
- Simple reporting

## Observability

### Grafana
**Port:** 3002 | **Level:** 2+

Monitoring and visualization platform with pre-configured dashboards.

```yaml
INCLUDE_GRAFANA=true
GRAFANA_PASSWORD=admin
```

**Use cases:**
- Log visualization
- Metrics dashboards
- Alerting

### Loki
**Port:** 3100 | **Level:** 2+

Log aggregation system designed to work with Grafana.

```yaml
INCLUDE_LOKI=true
```

**Use cases:**
- Centralized logging
- Log search
- Log-based metrics

### Promtail
**Level:** 2+

Log collector that ships logs to Loki.

```yaml
INCLUDE_PROMTAIL=true
```

**Use cases:**
- Container log collection
- Log shipping

## Infrastructure

### Kyros Dashboard
**Port:** 5000 | **Level:** 1+

Platform control panel for service overview and quick access.

```yaml
INCLUDE_KYROS=true
```

**Use cases:**
- Service status
- Quick navigation
- Health monitoring

### Portainer
**Port:** 9000 | **Level:** 1+

Container management UI for Docker.

```yaml
INCLUDE_PORTAINER=true
```

**Use cases:**
- Container management
- Log viewing
- Resource monitoring

### Traefik
**Ports:** 80, 443 | **Level:** 2+

Reverse proxy with automatic HTTPS via Let's Encrypt.

```yaml
INCLUDE_TRAEFIK=true
DOMAIN=yourdomain.com
```

**Use cases:**
- HTTPS termination
- Load balancing
- Service routing

## Component Dependencies

Some components depend on others:

| Component | Depends On |
|-----------|------------|
| Dagster | PostgreSQL |
| Superset | PostgreSQL, Dagster (optional) |
| Grafana | Loki |
| Promtail | Loki |
| Spark Workers | Spark Master |
| Hive Metastore | PostgreSQL |

Kyros automatically handles these dependencies through Docker Compose health checks.
