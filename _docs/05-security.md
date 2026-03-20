---
title: "Security"
permalink: /docs/security/
excerpt: "Securing your Kyros deployment with secrets management."
last_modified_at: 2025-03-20
toc: true
---

Kyros supports Docker secrets for secure credential management in production environments.

## Default Credentials

**Development defaults (change for production!):**

| Service | Username | Password |
|---------|----------|----------|
| PostgreSQL | kyros | kyros_dev |
| Superset | admin | admin |
| MinIO | kyros | kyros_dev |
| Grafana | admin | admin |

## Generating Secure Secrets

```bash
make secrets
```

This creates secure random passwords in the `secrets/` directory:
- `postgres_password.txt`
- `superset_password.txt`
- `superset_secret_key.txt`
- `minio_password.txt`
- `grafana_password.txt`

## Enabling Docker Secrets

```bash
# Generate secrets
make secrets

# Enable in .env
make secrets-enable

# Or manually add to .env:
USE_DOCKER_SECRETS=true

# Regenerate and restart
make generate
make up
```

## How Docker Secrets Work

1. Secret files are mounted as Docker secrets
2. Services read credentials from `/run/secrets/<secret_name>`
3. Passwords are never exposed in environment variables or logs

## Manual Secret Creation

```bash
mkdir -p secrets

# PostgreSQL
echo "your-secure-password" > secrets/postgres_password.txt

# Superset
echo "your-superset-password" > secrets/superset_password.txt
openssl rand -hex 32 > secrets/superset_secret_key.txt

# MinIO
echo "your-minio-password" > secrets/minio_password.txt

# Grafana
echo "your-grafana-password" > secrets/grafana_password.txt

# Set permissions
chmod 600 secrets/*.txt
```

## Secret Rotation

1. Stop services: `make down`
2. Regenerate secrets: `make secrets`
3. Restart services: `make up`

**Note:** Some services may require data migration when changing passwords.

## Security Checklist

Before going to production:

- [ ] Generate unique secrets (`make secrets`)
- [ ] Enable Docker secrets (`USE_DOCKER_SECRETS=true`)
- [ ] Enable HTTPS with Traefik
- [ ] Restrict file permissions on secrets
- [ ] Never commit secrets to git
- [ ] Configure firewall rules
- [ ] Enable Grafana alerting
- [ ] Set up regular backups

## Network Security

By default, all services are on an internal Docker network (`my-network`). Only exposed ports are accessible from the host.

### Restricting Access

To restrict services to localhost only, modify port mappings in service files:

```yaml
ports:
  - "127.0.0.1:8088:8088"  # Only accessible from localhost
```

### Using Traefik

For production, use Traefik to expose services with HTTPS:

```bash
INCLUDE_TRAEFIK=true
DOMAIN=yourdomain.com
```

See [Traefik & HTTPS](/docs/traefik/) for details.

## Secrets Best Practices

1. **Never commit secrets to git** - Use `.gitignore`
2. **Use environment-specific secrets** - Different passwords for dev/staging/prod
3. **Rotate regularly** - At least quarterly
4. **Audit access** - Monitor who has access to secrets
5. **Use external secret stores** - Consider HashiCorp Vault for enterprise
