---
title: ""
layout: splash
permalink: /
date: 2024-09-24T11:48:41-04:00
header:
  overlay_color: "#1a1a1a"
  overlay_filter: "0.7"
  overlay_image: /assets/images/kyros-splash-bg.webp
  actions:
    - label: "Get Started"
      url: "/docs/quick-start-guide/"
    - label: "GitHub"
      url: "https://github.com/benjamin-berhault/kyros"
excerpt: "Deploy the right data architecture at the right time. Start simple, scale when justified. Open-source, cloud-agnostic, modular."
intro:
  - excerpt: "**The data engineering industry has an over-engineering problem.** Companies with 10GB deploy Spark clusters. Startups pay $15k/month for Databricks when DuckDB would suffice. Every vendor says 'use us.' Nobody says 'you don't need us yet.' **Kyros is different.**"
feature_row:
  - image_path: /assets/images/kyros-feature-1.webp
    alt: "Progressive Architecture"
    title: "Progressive Architecture"
    excerpt: "5 architecture levels from local development to enterprise scale. Start with Level 0 (DuckDB + dbt), grow to Level 4 (Kafka + Flink) only when justified."
  - image_path: /assets/images/kyros-feature-2.webp
    alt: "Cost-Effective"
    title: "Right-Sized Costs"
    excerpt: "Level 0 costs $0/month. Level 1 starts at $20. Only pay for what you need. No vendor lock-in, no surprise bills."
  - image_path: /assets/images/kyros-feature-3.webp
    title: "Open-Source Stack"
    excerpt: "Built on proven technologies: PostgreSQL, Dagster, Superset, Spark, Trino, Kafka, Flink. All open-source, all production-ready."
feature_row2:
  - image_path: /assets/images/kyros-feature-4.webp
    alt: "One-Command Deploy"
    title: "One-Command Deploy"
    excerpt: "Choose your level: `make level-1` deploys PostgreSQL + Dagster + Superset. `make level-3` adds Spark + Trino. Interactive CLI guides you through custom configurations."
  - image_path: /assets/images/kyros-feature-5.webp
    alt: "Production Ready"
    title: "Production Ready"
    excerpt: "Includes Grafana + Loki for observability, Traefik for HTTPS, backup/restore scripts, Docker secrets for security, and health monitoring out of the box."
  - image_path: /assets/images/kyros-feature-6.webp
    alt: "Decision Helper"
    title: "Teaches Trade-offs"
    excerpt: "Kyros doesn't just provide tools—it explains when to use them. Built-in guidance helps you understand when it's time to scale up (or stay simple)."
levels_row:
  - title: "Level 0: Local"
    excerpt: "DuckDB + dbt<br>**$0/month** • < 50 GB"
  - title: "Level 1: Team"
    excerpt: "+ PostgreSQL + Dagster + Superset<br>**$20-100/month** • < 500 GB"
  - title: "Level 2: Data Lake"
    excerpt: "+ MinIO + JupyterLab + Grafana<br>**$50-150/month** • < 1 TB"
  - title: "Level 3: Distributed"
    excerpt: "+ Spark + Trino + Code Server<br>**$150-500/month** • 1+ TB"
  - title: "Level 4: Enterprise"
    excerpt: "+ Kafka + Flink + Full Observability<br>**$500+/month** • Any scale"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

## Architecture Levels

Choose your complexity level based on actual data needs:

{% include feature_row id="levels_row" %}

{% include feature_row id="feature_row2" %}

## Who Is This For?

- **Students** learning data engineering without cloud bills
- **Bootstrapped startups** who need results, not impressive architecture
- **Career switchers** building portfolio projects
- **Small teams** who want to grow into complexity, not start with it
- **Anyone** tired of over-engineering

## Philosophy

1. **Complexity is a cost** — Every service added is operational burden. Justify it.
2. **Ready to scale, not already scaled** — Have the path, don't walk it prematurely.
3. **Teach the trade-offs** — Don't just provide tools, explain when to use them.
4. **Accessible by default** — A student and a startup should both find value here.
5. **Honest about limitations** — This is a learning and launching platform, not enterprise software.

[Get Started with Kyros](/docs/quick-start-guide/){: .btn .btn--success .btn--large}
[View on GitHub](https://github.com/benjamin-berhault/kyros){: .btn .btn--primary .btn--large}
