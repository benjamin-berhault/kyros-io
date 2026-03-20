---
permalink: /about/
title: "About"
excerpt: "Kyros is an open-source, cloud-agnostic data platform that scales from laptop to enterprise."
last_modified_at: 2025-03-20
toc: true
---

**Kyros** is an open-source data platform that grows with your needs. Start with a simple PostgreSQL + Dagster setup, and progressively add components as your requirements evolve—without rewriting your infrastructure.

Our mission is to provide a **right-sized** alternative to one-size-fits-all platforms. Why pay for enterprise complexity when you need a data warehouse? Why struggle with basic tools when you need distributed processing?

[Get Started]({{ "/docs/quick-start-guide/" | relative_url }}){: .btn .btn--success .btn--large}
[View on GitHub](https://github.com/benjamin-berhault/kyros){: .btn .btn--primary .btn--large}

## Philosophy

### Right Tool, Right Time, Right Cost

Traditional data platforms force a choice: simple but limited, or powerful but complex. Kyros offers a third way with **5 architecture levels**:

| Level | Focus | When to Use |
|-------|-------|-------------|
| **0** | Learning | Tutorials, prototyping |
| **1** | Foundation | Small teams, simple pipelines |
| **2** | Production | Production workloads with monitoring |
| **3** | Scale | Large datasets, distributed processing |
| **4** | Enterprise | Full analytics platform |

Each level builds on the previous, so you never throw away work when scaling up.

### No Vendor Lock-in

Kyros runs anywhere Docker runs:
- Your laptop for development
- A single VPS for small workloads
- A multi-node cluster for enterprise scale
- Any cloud provider (AWS, GCP, Azure, DigitalOcean)

## Core Components

| Component | Purpose |
|-----------|---------|
| **PostgreSQL** | Data warehouse and metadata storage |
| **Dagster** | Pipeline orchestration and scheduling |
| **Apache Superset** | Business intelligence and dashboards |
| **MinIO** | S3-compatible object storage |
| **Apache Spark** | Distributed data processing |
| **Grafana + Loki** | Observability and logging |
| **Traefik** | HTTPS and reverse proxy |

## Documentation

| Guide | Description |
|-------|-------------|
| [Quick Start]({{ "/docs/quick-start-guide/" | relative_url }}) | Get running in 5 minutes |
| [Architecture Levels]({{ "/docs/architecture-levels/" | relative_url }}) | Understand the scaling model |
| [Components]({{ "/docs/components/" | relative_url }}) | Full component reference |
| [Security]({{ "/docs/security/" | relative_url }}) | Docker secrets and best practices |
| [Production]({{ "/docs/production/" | relative_url }}) | Production deployment checklist |

## Credits

Built with these excellent open-source projects:

- [Apache Spark](https://spark.apache.org) - Distributed processing
- [Apache Superset](https://superset.apache.org) - Business intelligence
- [Dagster](https://dagster.io) - Data orchestration
- [PostgreSQL](https://www.postgresql.org) - Database
- [MinIO](https://min.io) - Object storage
- [Grafana](https://grafana.com) - Observability
- [Traefik](https://traefik.io) - Reverse proxy

---

Kyros is developed and maintained by [Benjamin Berhault](https://github.com/benjamin-berhault).
