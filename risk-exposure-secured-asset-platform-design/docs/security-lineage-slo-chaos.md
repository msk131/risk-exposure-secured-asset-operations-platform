# Security, Lineage, Service Objectives, And Chaos Verification

This document defines cross-cutting platform controls required before a bank-grade migration cutover.

## Zero-Trust Network And Service Identity

| Area | Standard |
| --- | --- |
| Network boundary | Use isolated Virtual Private Cloud (VPC), private subnets, restricted security groups, and private endpoints |
| Service access | Services use least-privilege Identity and Access Management (IAM) roles |
| Service identity | Mutual Transport Layer Security (mTLS) is required for service-to-service traffic where supported |
| Secrets | Secrets Manager owns credentials; rotation must be rehearsed |
| Encryption | Key Management Service (KMS) keys protect databases, object storage, backups, and evidence archives |
| Break-glass | Emergency access must be time-bound, approved, logged, and reviewed |

## Access Review Evidence

- Privileged user list.
- Service account permission diff.
- Secrets rotation report.
- Break-glass access report.
- Security group and endpoint review.

## Automated Data Lineage And Audit Evidence

| Metadata | Required Fields |
| --- | --- |
| Dataset | Name, owner, schema version, classification, retention |
| Event | Event type, schema version, producer, consumer, topic |
| Job | Run identifier, source, target, row counts, checksum, status |
| Report | Source snapshot, mapping version, calculation version, approval |
| Release | Commit, build, artifact, migration scripts, approvals |

Every production release should produce an evidence bundle containing:

- Architecture Decision Records (ADRs) used by the change.
- Database migration dry-run result.
- Reconciliation report.
- Security scan result.
- Service Level Objective (SLO) dashboard snapshot.
- Approval and rollback record.

## Service Level Objectives

| Capability | Service Level Objective (SLO) | Alert Threshold |
| --- | --- | --- |
| Critical write Application Programming Interface (API) | 99.95 percent successful requests monthly | Page on 5-minute error budget burn |
| Critical read Application Programming Interface (API) | p95 latency under 300 milliseconds | Warn at 250 milliseconds, page at 500 milliseconds |
| Change Data Capture (CDC) lag | Under 30 seconds for financial state | Page above 2 minutes |
| Outbox publishing delay | Oldest pending event under 60 seconds | Page above 5 minutes |
| Reconciliation freshness | Daily critical reconciliation complete before business sign-off | Page if late or critical break exists |
| Batch completion | End-of-day batch completes within approved window | Page if forecast misses window |

## Chaos Verification

| Experiment | Expected Result |
| --- | --- |
| Delete one service pod | No data loss; service recovers through Kubernetes scheduling |
| Aurora failover rehearsal | Writes pause safely; idempotency prevents duplicate commands |
| Kafka broker interruption | Consumers resume from committed offsets; no skipped event |
| Change Data Capture connector pause | Lag alert fires; replay catches up after resume |
| 200 millisecond network latency injection | Circuit breakers and timeouts protect downstream services |
| Object storage unavailable | Evidence writes retry and business transaction path remains protected |

Each experiment requires:

- Approved test window.
- Baseline metrics.
- Failure injection record.
- Recovery Time Objective (RTO) and Recovery Point Objective (RPO) result.
- Reconciliation after recovery.
- Evidence attached to cutover gate.

