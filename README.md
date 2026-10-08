Platform GitOps

Kubernetes platform management using GitOps approach with Flux CD on GCP.

## Stack

- GKE — Google Kubernetes Engine cluster
- Flux CD — GitOps operator, syncs cluster state with Git every 3 hours
- MariaDB Operator — database management in Kubernetes
- Velero — automated backups with GCP CSI snapshots
- External Secrets Operator — secrets management from GCP Secret Manager
- API Gateway — GKE L7 regional external load balancer

## Repository Structure

clusters/alpha/
  operators/       # Cluster operators (Helm releases via Flux)
    mariadb/       # MariaDB operator v26.6.0
    velero/        # Backup operator
    flux-system/   # GitOps operator
  assets/          # Cluster workloads
    mariadb/       # MariaDB instance for wiki
    backup/        # Velero backup schedules

## GitOps Flow

Git push → Flux detects changes → applies to cluster
Flux checks repository every 3 hours and reconciles desired state with actual cluster state.

## Backup Strategy

- Daily backups at 12:00 UTC
- 14 days retention (TTL 336h)
- GCP CSI disk snapshots for PVC data
- Tested full cluster restore from backup
