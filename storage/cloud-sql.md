## Cloud SQL
managed SQL service: MySQL, PostgreSQL, Microsoft SQL server

**capacity: 64
IOPS: 60000**
- updates are automatically patched
- scale out with read replicas
- HA configuration: primary instance + standby instance.
  - synchronous replication to each zones persistent disk
  - all writes made to the primary instance are replicated to disks in both zones before a transaction is reported as committed
  - instance or zone failure => PD is attached to standby instance which becomes the new primary instance (failover)
- automated and on-demand backups with point-in-time recovery.
- import and export databases using mysqldump or import and export CSV files
- Cloud SQL does not support user-defined functions

### Choosing a connection type
- application is hosted within the same Google Cloud project & colocated in the same region:
  - Private IP connection: traffic is never exposed to the Internet
- application is hosted in another region or project/trying to connect to Cloud SQL from outside on GCP
  - automate SSL connection -> Cloud SQL Proxy: handles authentication, encryption and key rotation for you.
  - manual SSL connection -> Manual SSL connection: generate and periodically rotate the certificates yourself.
  - cannot use SSL -> authorized networks: unencrypted connection by authorizing a specific IP address to connect to your SQL Server over its external IP address.

gcloud sql
