---
title: "Quick-Start Guide"
permalink: /docs/quick-start-guide/
excerpt: "Get Kyros running in under 5 minutes."
last_modified_at: 2025-03-20
redirect_from:
  - /setup/
toc: true
---

Kyros makes it easy to deploy a complete data platform. Choose your architecture level and deploy with a single command.

## Prerequisites

- **Docker** and **Docker Compose** (v2+)
- **Python 3.8+** (for the CLI)
- **8GB+ RAM** (16GB+ for Level 3+)
- **Git**

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/benjamin-berhault/kyros.git
cd kyros
```

### 2. Choose Your Architecture Level

| Level | Name | What You Get | Data Size | Cost/Month |
|:-----:|------|--------------|-----------|------------|
| **0** | Local | DuckDB + dbt | < 50 GB | $0 |
| **1** | Team | + PostgreSQL + Dagster + Superset | < 500 GB | $20-100 |
| **2** | Data Lake | + MinIO + JupyterLab + Grafana | < 1 TB | $50-150 |
| **3** | Distributed | + Spark + Trino | 1+ TB | $150-500 |
| **4** | Enterprise | + Kafka + Flink | Any | $500+ |

### 3. Deploy

**Option A: Use Makefile (Recommended)**

```bash
# Deploy Level 1 (Team stack)
make level-1

# Or Level 2 (Data Lake)
make level-2

# Or Level 3 (Distributed)
make level-3
```

**Option B: Interactive CLI**

```bash
./kyros-cli.py
```

The CLI guides you through:
- Selecting a level or custom configuration
- Choosing specific components
- Setting Spark worker count
- Deploying with real-time build logs

**Option C: Manual Configuration**

```bash
# Copy a preset
cp presets/level-1.env .env

# Generate docker-compose.yml
make generate

# Start services
make up
```

## Accessing Services

After deployment, access services at:

| Service | URL | Default Credentials |
|---------|-----|---------------------|
| Kyros Dashboard | http://localhost:5000 | - |
| Superset | http://localhost:8088 | admin / admin |
| Dagster | http://localhost:3000 | - |
| Grafana | http://localhost:3002 | admin / admin |
| JupyterLab | http://localhost:8888 | - |
| MinIO Console | http://localhost:9001 | kyros / kyros_dev |
| Portainer | http://localhost:9000 | (set on first login) |

## Common Commands

```bash
# View all available commands
make help

# Check service status
make status

# View logs
make logs

# Stop all services
make down

# Restart services
make restart
```

## Custom Configuration

Edit `.env` to enable/disable specific components:

```bash
# Components
INCLUDE_POSTGRES=true
INCLUDE_DAGSTER=true
INCLUDE_SUPERSET=true
INCLUDE_MINIO=true
INCLUDE_JUPYTERLAB=true
INCLUDE_GRAFANA=true
INCLUDE_LOKI=true
INCLUDE_TRINO=false
INCLUDE_KAFKA=false
INCLUDE_FLINK=false

# Spark workers (Level 3+)
WORKERS=2

# Resources
SPARK_WORKER_MEMORY=2G
SPARK_WORKER_CORES=2
```

After editing, regenerate and restart:

```bash
make generate
make up
```

## Do You Need Spark?

Before deploying Level 3+, ask yourself:

| Data Size | Recommendation |
|-----------|----------------|
| < 10 GB | PostgreSQL + dbt or DuckDB. Spark is overkill. |
| 10-100 GB | DuckDB or PostgreSQL + dbt. Spark optional. |
| 100 GB - 1 TB | PostgreSQL/warehouse + dbt. Spark starts making sense. |
| 1+ TB | Spark is justified. Consider Trino for federated queries. |

## Production Deployment

For production environments:

1. **Generate secure secrets:**
   ```bash
   make secrets
   ```

2. **Enable Docker secrets:**
   ```bash
   make secrets-enable
   ```

3. **Enable HTTPS with Traefik:**
   ```bash
   # Add to .env
   INCLUDE_TRAEFIK=true
   DOMAIN=yourdomain.com
   ```

4. **Set up backups:**
   ```bash
   make backup
   ```

See [Security Guide](/docs/security/) and [Production Guide](/docs/production/) for more details.

## Troubleshooting

### Services won't start

Check Docker is running and has enough resources:
```bash
docker info
docker compose ps
```

### Out of memory

Reduce the number of services or increase Docker memory limit. Start with Level 1 and add components gradually.

### Port conflicts

Check if ports are already in use:
```bash
lsof -i :3000  # Check Dagster port
lsof -i :8088  # Check Superset port
```

### View service logs

```bash
docker compose logs dagster
docker compose logs superset
```

## Next Steps

- [Architecture Levels](/docs/architecture-levels/) - Deep dive into each level
- [Components Reference](/docs/components/) - All available components
- [Observability](/docs/observability/) - Monitoring with Grafana + Loki
- [Security](/docs/security/) - Secrets management and HTTPS
