# Coexistence Data Sync

During phased migration, legacy and target systems often run together. Data synchronization keeps both sides aligned until traffic and ownership fully move to the target platform.

## Objectives

- Keep legacy and target records consistent during phased migration.
- Support parallel run and reconciliation.
- Prevent double writes, stale reads, and ownership ambiguity.
- Identify system of record by entity and migration phase.
- Provide rollback and replay options when sync fails.

## Sync Patterns

| Pattern | Description | Best Fit |
| --- | --- | --- |
| Snapshot backfill | One-time or repeated bulk load from legacy to target | Initial migration load |
| CDC | Capture changed records from legacy database | Ongoing legacy-to-target sync |
| Dual write | Application writes to both legacy and target | Short-lived controlled transition |
| Event replication | Domain events update target projections or legacy adapters | Target-led migration phase |
| Read model sync | Reporting projections updated from events or CDC | Reporting migration |

## Recommended Direction

1. Backfill target from legacy snapshots.
2. Enable legacy-to-target CDC or scheduled delta sync.
3. Run parallel validation and reconciliation.
4. Shift selected reads to target read models.
5. Shift writes by capability only after ownership is clear.
6. Stop sync and retire legacy paths after cutover sign-off.

## Entity Ownership During Coexistence

| Entity | Early Phase Owner | Coexistence Handling | Target Phase Owner |
| --- | --- | --- | --- |
| Client or party | Legacy | Backfill and delta sync | Target reference capability |
| Portfolio | Legacy | CDC or controlled dual maintenance | Portfolio service |
| Holding | Legacy | Batch or CDC sync with reconciliation | Holdings service |
| Trade | Legacy or upstream | Event or file-to-target ingestion | Trade ingestion service |
| Price | Legacy pipeline | Parallel load into target | Market data capability |
| Valuation | Legacy calculation | Parallel run comparison | Valuation service |
| Exception | Legacy operations | Dual visibility | Workflow or operations service |

## Sync Failure Modes

| Failure | Impact | Control |
| --- | --- | --- |
| Missed CDC event | Target stale or incomplete | Lag monitoring and replay |
| Out-of-order update | Incorrect lifecycle state | Versioning and event ordering rules |
| Duplicate update | Double counted record | Idempotency keys and natural keys |
| Conflicting write | Legacy and target disagree | Single system-of-record rule by phase |
| Partial batch load | Incomplete data | Batch control table and reconciliation |
| Mapping drift | Source-target mismatch | Mapping version control and test cases |

## Cutover Criteria

- Sync lag is within agreed threshold.
- Critical reconciliation checks pass.
- Target access and RBAC controls are validated.
- No unresolved high-severity mapping breaks remain.
- Rollback and replay procedures are ready.
- Business and technology sign-off is captured.

