# Migration Planning

This project defines a practical migration plan for moving from a legacy financial operations platform to a modern Risk Exposure & Secured Asset Operations Platform.

## Migration Objectives

- Preserve data integrity with zero-loss mapping for customer, account, transaction, balance, agreement, and secured asset records.
- Cleanse duplicate, outdated, corrupt, and incomplete data before migration.
- Run legacy and new flows in parallel before final cutover.
- Maintain immutable audit trails during and after migration.
- Protect sensitive data through encryption, privacy controls, and least-privilege access.
- Minimize downtime through phased cutover and rehearsed rollback.
- Improve Oracle database performance before and during migration.

## Migration Architecture

Detailed architecture: [migration-architecture.md](docs/migration-architecture.md)

The migration architecture supports coexistence between the legacy platform and the new application. Traffic can move gradually by capability, customer segment, region, channel, or workflow.

## Data Integrity And Migration Controls

| Control | Purpose |
| --- | --- |
| Zero-loss mapping | Ensure critical records move with complete accuracy |
| Data cleansing | Remove or quarantine duplicate, outdated, corrupt, or inconsistent records |
| Parallel run | Operate legacy and new flows concurrently for validation |
| Reconciliation | Compare counts, balances, statuses, lifecycle events, and exceptions |
| Cutover sign-off | Require business and technology approval before final traffic migration |

## Regulatory, Security, And Compliance Controls

| Control | Purpose |
| --- | --- |
| Immutable audit trail | Capture migration actions, approvals, exceptions, and data changes |
| Encryption at rest | Protect stored sensitive data in databases, backups, staging, and archives |
| Encryption in transit | Protect API, event, replication, and migration traffic |
| Access controls | Enforce least privilege for users, services, and migration jobs |
| Privacy controls | Support retention, minimization, masking, and controlled access |

## Migration Phases

### Phase 1: Discovery

- Inventory applications, jobs, tables, stored procedures, interfaces, and owners.
- Map business capabilities and critical user journeys.
- Identify shared databases and hidden consumers.
- Establish performance, reliability, and data quality baselines.

### Phase 2: Data And Performance Readiness

- Define zero-loss data mapping rules.
- Profile and cleanse legacy data.
- Identify top Oracle SQL and batch bottlenecks.
- Tune critical SQL, indexes, statistics, and batch execution paths.
- Prepare reconciliation rules and exception workflows.

### Phase 3: Coexistence Build

- Introduce API gateway or routing layer.
- Create migration staging and synchronization jobs.
- Add audit, observability, and migration dashboards.
- Prepare target schema and service-owned data stores.

### Phase 4: Parallel Run

- Run legacy and new flows together.
- Compare transaction history, balances, statuses, and operational outputs.
- Review exceptions and fix mapping or logic gaps.
- Agree readiness thresholds for traffic migration.

### Phase 5: Phased Cutover

- Move low-risk capabilities first.
- Shift traffic by workflow, channel, segment, or region.
- Schedule high-risk transitions during low-activity windows.
- Monitor business and technical metrics.
- Execute rollback if validation fails.

### Phase 6: Decommission

- Retire obsolete tables, jobs, interfaces, and code paths.
- Archive historical data based on retention rules.
- Remove dual-run processes after sign-off.
- Update operational documentation and support runbooks.

## Oracle Performance Improvement Plan

| Area | Activities |
| --- | --- |
| Baseline | Capture AWR-style metrics, top SQL, batch duration, wait events, and volume profile |
| SQL tuning | Review execution plans, predicates, joins, bind variables, and cardinality |
| Index strategy | Add, adjust, or remove indexes based on access patterns and maintenance cost |
| Statistics | Review stale or misleading optimizer statistics |
| Batch tuning | Replace row-by-row processing, tune commits, and reduce database round trips |
| Regression control | Add performance validation before production rollout |

## Success Metrics

- Reconciliation pass rate meets agreed threshold.
- No critical data loss across mapped entities.
- Batch window reduced for selected workloads.
- Downtime stays within approved window.
- Production incidents reduce after migration.
- Legacy components are retired with evidence.

