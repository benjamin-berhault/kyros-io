---
title: "Production Checklist"
permalink: /docs/production/
excerpt: "Preparing Kyros for production deployment."
last_modified_at: 2025-03-20
toc: true
---

Before deploying Kyros to production, complete this checklist to ensure security, reliability, and performance.

## Security

- [ ] **Generate unique secrets**
  ```bash
  make secrets
  make secrets-enable
  ```

- [ ] **Enable HTTPS with Traefik**
  ```bash
  INCLUDE_TRAEFIK=true
  DOMAIN=yourdomain.com
  ACME_EMAIL=admin@yourdomain.com
  ```

- [ ] **Change all default passwords**
  - PostgreSQL
  - Superset
  - MinIO
  - Grafana
  - Traefik dashboard

- [ ] **Restrict network access**
  - Configure firewall (only expose 80/443)
  - Use VPN for admin access

- [ ] **Review file permissions**
  ```bash
  chmod 600 secrets/*.txt
  chmod 700 secrets/
  ```

## Reliability

- [ ] **Set up monitoring**
  ```bash
  INCLUDE_GRAFANA=true
  INCLUDE_LOKI=true
  INCLUDE_PROMTAIL=true
  ```

- [ ] **Configure alerting**
  - Set up Slack/email notifications in Grafana
  - Review pre-configured alert rules

- [ ] **Set up backups**
  ```bash
  # Test backup
  make backup

  # Schedule daily backups via cron
  0 2 * * * cd /path/to/kyros && ./scripts/backup.sh all
  ```

- [ ] **Test restore procedure**
  ```bash
  make restore
  ```

- [ ] **Configure health checks**
  - All services have health checks configured
  - Verify with `docker compose ps`

## Performance

- [ ] **Size resources appropriately**

  | Level | Recommended RAM | CPU |
  |-------|-----------------|-----|
  | 1 | 8 GB | 4 cores |
  | 2 | 12 GB | 4 cores |
  | 3 | 16 GB | 8 cores |
  | 4 | 32 GB | 16 cores |

- [ ] **Configure resource limits**
  - Review `deploy.resources` in service files
  - Adjust based on workload

- [ ] **Set appropriate worker counts**
  ```bash
  WORKERS=2  # Adjust based on data volume
  SPARK_WORKER_MEMORY=4G
  SPARK_WORKER_CORES=4
  ```

- [ ] **Use SSD storage**
  - PostgreSQL data directory
  - MinIO storage
  - Docker volumes

## Operations

- [ ] **Document your deployment**
  - Architecture level chosen
  - Enabled components
  - Custom configurations

- [ ] **Set up logging retention**
  - Review Loki retention (default 31 days)
  - Configure log rotation

- [ ] **Create runbooks**
  - How to restart services
  - How to restore from backup
  - How to scale up/down

- [ ] **Set up on-call procedures**
  - Alert escalation
  - Contact information
  - Access procedures

## Pre-Launch

- [ ] **Test all services**
  ```bash
  make test-integration
  ```

- [ ] **Verify all URLs work**
  - Superset dashboards load
  - Dagster UI accessible
  - Grafana shows logs

- [ ] **Load test if needed**
  - Test with expected data volumes
  - Verify query performance

- [ ] **Review logs for errors**
  ```bash
  docker compose logs | grep -i error
  ```

## Post-Launch

- [ ] **Monitor first 24 hours closely**
  - Watch Grafana dashboards
  - Check for alert triggers

- [ ] **Verify backups are running**
  ```bash
  ls -la backups/
  ```

- [ ] **Document any issues found**
  - Create runbook entries
  - Update configuration as needed

## Ongoing Maintenance

### Weekly

- Review Grafana alerts
- Check disk usage
- Verify backup integrity

### Monthly

- Apply security updates
- Review resource usage
- Rotate secrets if needed

### Quarterly

- Full disaster recovery test
- Review and update documentation
- Assess if architecture level is appropriate

## Rollback Plan

If something goes wrong:

1. **Stop services:**
   ```bash
   make down
   ```

2. **Restore from backup:**
   ```bash
   make restore
   ```

3. **Start services:**
   ```bash
   make up
   ```

4. **Verify restoration:**
   ```bash
   make status
   make test-integration
   ```
