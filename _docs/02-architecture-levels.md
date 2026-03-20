---
title: "Architecture Levels"
permalink: /docs/architecture-levels/
excerpt: "Understanding Kyros's progressive architecture approach."
last_modified_at: 2025-03-20
toc: true
---

Kyros uses a progressive architecture approach with 5 distinct levels. Each level builds upon the previous one, adding complexity only when justified by data scale and requirements.

## Level Overview

| Level | Name | Stack | Data Size | Monthly Cost |
|:-----:|------|-------|-----------|--------------|
| **0** | Local | DuckDB + dbt | < 50 GB | $0 |
| **1** | Team | + PostgreSQL + Dagster + Superset | < 500 GB | $20-100 |
| **2** | Data Lake | + MinIO + JupyterLab + Grafana | < 1 TB | $50-150 |
| **3** | Distributed | + Spark + Trino | 1+ TB | $150-500 |
| **4** | Enterprise | + Kafka + Flink | Any | $500+ |

## Level 0: Local Development

**Best for:** Individual analysts, learning, prototyping

### Components
- **DuckDB** - In-process analytical database
- **dbt** - SQL transformations and data modeling

### When to Use
- Data fits in memory (< 50 GB)
- Single user / no collaboration needed
- Learning data engineering concepts
- Prototyping pipelines before deployment

### Upgrade When
- Need shared database access
- Need scheduled pipelines
- Need dashboards for stakeholders
- Data exceeds 50 GB

## Level 1: Team

**Best for:** Small teams, startups, departmental analytics

### Components
- Everything from Level 0, plus:
- **PostgreSQL** - Shared relational database
- **Dagster** - Pipeline orchestration
- **Superset** - BI dashboards
- **Portainer** - Container management
- **CloudBeaver** - Database UI

### When to Use
- Team needs shared database
- Need scheduled ETL pipelines
- Need self-service dashboards
- Data 50 GB - 500 GB

### Upgrade When
- Need object storage (S3-like)
- Need notebook environment
- Need monitoring/observability
- Data exceeds 500 GB

## Level 2: Data Lake

**Best for:** Growing teams, data science workflows

### Components
- Everything from Level 1, plus:
- **MinIO** - S3-compatible object storage
- **JupyterLab** - Notebooks for analysis
- **Grafana** - Monitoring dashboards
- **Loki** - Log aggregation
- **Promtail** - Log collection

### When to Use
- Need to store raw files (CSV, Parquet, JSON)
- Data scientists need notebooks
- Need operational monitoring
- Data 500 GB - 1 TB

### Upgrade When
- Single-node processing is too slow
- Need federated queries across sources
- Data exceeds 1 TB

## Level 3: Distributed

**Best for:** Large datasets, complex transformations

### Components
- Everything from Level 2, plus:
- **Spark** - Distributed processing
- **Trino** - Federated SQL queries
- **Code Server** - VS Code in browser

### When to Use
- Data exceeds 1 TB
- Need distributed compute
- Need to query multiple data sources
- Complex transformations that can't run on single node

### Upgrade When
- Need real-time streaming
- Need event-driven architecture
- Enterprise compliance requirements

## Level 4: Enterprise

**Best for:** Enterprise scale, real-time requirements

### Components
- Everything from Level 3, plus:
- **Kafka** - Event streaming
- **Flink** - Stream processing
- **Traefik** - Reverse proxy with HTTPS
- Full observability stack

### When to Use
- Real-time data requirements
- Event-driven architecture
- Enterprise-scale operations
- Any data scale

## Choosing the Right Level

### Decision Framework

```
┌─────────────────────────────────────────────────────────────────┐
│                    CHOOSING YOUR LEVEL                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. How much data do you have?                                 │
│     < 50 GB     → Start at Level 0                             │
│     50-500 GB   → Start at Level 1                             │
│     500 GB-1 TB → Start at Level 2                             │
│     1+ TB       → Start at Level 3                             │
│                                                                 │
│  2. Do you need real-time?                                     │
│     No          → Levels 0-3 are fine                          │
│     Yes         → You need Level 4                             │
│                                                                 │
│  3. How many users?                                            │
│     Just me     → Level 0 or 1                                 │
│     Small team  → Level 1 or 2                                 │
│     Enterprise  → Level 3 or 4                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Common Mistakes to Avoid

1. **Starting at Level 3 with 10 GB of data**
   - Spark adds operational complexity
   - PostgreSQL + dbt handles 10 GB easily

2. **Skipping Level 2 observability**
   - You need logs and monitoring before scaling
   - Debug issues before they become disasters

3. **Adding Kafka without real-time requirements**
   - Kafka adds significant complexity
   - Batch processing handles most use cases

## Upgrading Between Levels

Kyros makes upgrading easy:

```bash
# Currently at Level 1, want to add Level 2 components
cp presets/level-2.env .env
make generate
make up
```

Your existing data in PostgreSQL persists. New components are added alongside existing ones.

## Downgrading

Need to reduce costs or complexity?

```bash
# Scale back to Level 1
cp presets/level-1.env .env
make generate
make up
```

Unused services stop, but volumes persist if you need to scale back up later.
