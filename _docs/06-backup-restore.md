---
title: "Backup & Restore"
permalink: /docs/backup-restore/
excerpt: "Backing up and restoring your Kyros data."
last_modified_at: 2025-03-20
toc: true
---

Kyros includes scripts for backing up and restoring PostgreSQL, MinIO, Grafana, and Dagster data.

## Quick Backup

```bash
make backup
```

This creates a timestamped backup in `backups/YYYY-MM-DD_HH-MM-SS/`.

## Backup Components

### PostgreSQL

```bash
make backup-postgres
```

Creates:
- `postgres_all.sql.gz` - All databases
- `postgres_<dbname>.sql.gz` - Individual databases

### MinIO

```bash
make backup-minio
```

Creates:
- `minio.tar.gz` - All buckets and objects

### Grafana

```bash
make backup-grafana
```

Creates:
- `grafana.tar.gz` - Dashboards and data

## Full Backup

```bash
./scripts/backup.sh all
```

Or specify output directory:

```bash
./scripts/backup.sh all /path/to/backup
```

## Restore

### Interactive Restore

```bash
make restore
```

Lists available backups and prompts for selection.

### Direct Restore

```bash
./scripts/restore.sh backups/2024-01-15_10-30-00 all
```

### Restore Specific Component

```bash
./scripts/restore.sh backups/2024-01-15_10-30-00 postgres
./scripts/restore.sh backups/2024-01-15_10-30-00 minio
./scripts/restore.sh backups/2024-01-15_10-30-00 grafana
```

## Backup Contents

Each backup directory contains:

```
backups/2024-01-15_10-30-00/
├── manifest.json           # Backup metadata
├── postgres_all.sql.gz     # All PostgreSQL databases
├── postgres_sales.sql.gz   # Individual database
├── minio.tar.gz            # MinIO data
├── grafana.tar.gz          # Grafana dashboards/data
└── dagster_workspace.tar.gz # Dagster project files
```

## Scheduled Backups

### Using Cron

Add to crontab (`crontab -e`):

```bash
# Daily backup at 2 AM
0 2 * * * cd /path/to/kyros && ./scripts/backup.sh all

# Weekly full backup on Sunday
0 3 * * 0 cd /path/to/kyros && ./scripts/backup.sh all /backups/weekly

# Keep last 7 daily backups
0 4 * * * find /path/to/kyros/backups -maxdepth 1 -mtime +7 -type d -exec rm -rf {} \;
```

### Using Dagster

Create a Dagster job for automated backups with monitoring.

## Backup Storage

### Local Storage

Default: `./backups/`

### Remote Storage

Copy backups to cloud storage:

```bash
# AWS S3
aws s3 sync backups/ s3://your-bucket/kyros-backups/

# MinIO (another instance)
mc mirror backups/ myminio/kyros-backups/
```

## Disaster Recovery

### Full Recovery Steps

1. Install fresh Kyros:
   ```bash
   git clone https://github.com/benjamin-berhault/kyros.git
   cd kyros
   ```

2. Copy backup to `backups/`:
   ```bash
   cp -r /path/to/backup backups/
   ```

3. Start services:
   ```bash
   cp presets/level-2.env .env
   make up
   ```

4. Wait for services to be healthy:
   ```bash
   make status
   ```

5. Restore data:
   ```bash
   make restore
   ```

## Best Practices

1. **Test restores regularly** - A backup is only good if you can restore it
2. **Store backups offsite** - Don't keep backups only on the same machine
3. **Encrypt sensitive backups** - Use GPG or similar for encryption
4. **Document your backup schedule** - Know what's backed up and when
5. **Monitor backup jobs** - Set up alerts for failed backups
