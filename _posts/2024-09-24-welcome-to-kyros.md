---
title: "Introducing Kyros: Data Platform at the Right Scale"
date: 2024-09-24
categories:
  - announcement
tags:
  - kyros
  - data-platform
  - open-source
---

We are excited to introduce **Kyros**, an open-source data platform that grows with your needs. Start simple, scale when ready.

## The Problem

Building a data platform usually means choosing between:

- **Simple tools** that work today but won't scale
- **Enterprise platforms** with complexity you don't need yet

Either way, you end up rewriting infrastructure as your needs change.

## The Kyros Approach

Kyros solves this with **5 architecture levels**:

| Level | What You Get |
|-------|--------------|
| **Level 0** | Dagster + PostgreSQL for learning and prototyping |
| **Level 1** | Add Superset for BI dashboards |
| **Level 2** | Add Grafana + Loki for production monitoring |
| **Level 3** | Add Spark + MinIO for distributed processing |
| **Level 4** | Add JupyterLab for full analytics workbench |

Each level builds on the previous. Your pipelines, dashboards, and configurations carry forward—no rewrites needed.

## Key Features

- **Progressive scaling** - Start with Level 1, grow to Level 4
- **Cloud-agnostic** - Runs anywhere Docker runs
- **Production-ready** - HTTPS, secrets management, backup/restore
- **Observable** - Centralized logging with Grafana and Loki
- **Open source** - Apache 2.0 licensed

## Get Started

```bash
git clone https://github.com/benjamin-berhault/kyros.git
cd kyros
make level-1
```

You'll have PostgreSQL, Dagster, and Superset running in minutes.

## Learn More

- [Documentation](https://benjamin-berhault.github.io/kyros-io/docs/quick-start-guide/)
- [GitHub Repository](https://github.com/benjamin-berhault/kyros)
- [Architecture Levels](https://benjamin-berhault.github.io/kyros-io/docs/architecture-levels/)

---

Kyros is designed to make data engineering accessible—whether you're a solo developer learning pipelines or a team running production workloads.
