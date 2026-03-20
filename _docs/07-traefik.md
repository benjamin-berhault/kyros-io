---
title: "Traefik & HTTPS"
permalink: /docs/traefik/
excerpt: "Setting up Traefik reverse proxy with automatic HTTPS."
last_modified_at: 2025-03-20
toc: true
---

Kyros includes Traefik for reverse proxy with automatic HTTPS via Let's Encrypt.

## Enabling Traefik

Add to your `.env`:

```bash
INCLUDE_TRAEFIK=true
DOMAIN=example.com
```

Then deploy:

```bash
make generate
make up
```

## Service URLs

With Traefik enabled, access services via subdomains:

| Service | URL |
|---------|-----|
| Traefik Dashboard | https://traefik.example.com |
| Superset | https://superset.example.com |
| Dagster | https://dagster.example.com |
| Grafana | https://grafana.example.com |
| JupyterLab | https://jupyter.example.com |
| MinIO | https://minio.example.com |
| Kyros | https://kyros.example.com |

## DNS Configuration

Create wildcard A record pointing to your server:

```
*.example.com  →  YOUR_SERVER_IP
```

Or individual records:

```
superset.example.com  →  YOUR_SERVER_IP
dagster.example.com   →  YOUR_SERVER_IP
grafana.example.com   →  YOUR_SERVER_IP
```

## Let's Encrypt (Production)

### Enable Automatic HTTPS

1. Edit `services/traefik.yml` and uncomment:
   ```yaml
   - "--certificatesresolvers.letsencrypt.acme.tlschallenge=true"
   - "--certificatesresolvers.letsencrypt.acme.email=${ACME_EMAIL}"
   - "--certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json"
   ```

2. Add to `.env`:
   ```bash
   ACME_EMAIL=admin@example.com
   ```

3. Redeploy:
   ```bash
   make generate
   make up
   ```

## Dashboard Access

The Traefik dashboard is protected by basic auth.

**Default credentials:** admin / admin

### Change Dashboard Password

Generate a new password hash:

```bash
htpasswd -nb admin your-secure-password
```

Add to `.env`:

```bash
TRAEFIK_DASHBOARD_AUTH='admin:$apr1$xyz...'
```

## Local Development

For local development without a domain:

### Option 1: Use localhost

```bash
DOMAIN=localhost
```

Access via: `http://superset.localhost:8088`

### Option 2: Edit /etc/hosts

```
127.0.0.1  superset.local dagster.local grafana.local
```

### Option 3: Use nip.io

```bash
DOMAIN=127.0.0.1.nip.io
```

Access via: `http://superset.127.0.0.1.nip.io`

## Architecture

```
                    ┌─────────────┐
                    │   Internet  │
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │   Traefik   │
                    │  :80 :443   │
                    └──────┬──────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
  ┌─────┴─────┐     ┌──────┴──────┐    ┌──────┴──────┐
  │  Superset │     │   Dagster   │    │   Grafana   │
  │   :8088   │     │    :3000    │    │    :3002    │
  └───────────┘     └─────────────┘    └─────────────┘
```

## Adding Services to Traefik

To expose a new service through Traefik, add labels:

```yaml
services:
  myservice:
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.myservice.rule=Host(`myservice.${DOMAIN}`)"
      - "traefik.http.routers.myservice.entrypoints=websecure"
      - "traefik.http.routers.myservice.tls.certresolver=letsencrypt"
      - "traefik.http.services.myservice.loadbalancer.server.port=8080"
```

## Troubleshooting

### Certificate not generating

1. Check DNS resolves correctly:
   ```bash
   dig superset.example.com
   ```

2. Verify port 443 is accessible from internet

3. Check Traefik logs:
   ```bash
   docker compose logs traefik
   ```

### 404 Not Found

- Verify the Host rule matches your domain
- Check service is on `my-network`
- Verify `traefik.enable=true` label

### Service not accessible

1. Verify labels are correct
2. Check service is running:
   ```bash
   docker compose ps
   ```
3. Check Traefik dashboard for registered services
