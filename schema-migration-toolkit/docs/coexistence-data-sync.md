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

## Change Data Capture Ordering, Replay, And Lag Guardrails

| Control | Standard |
| --- | --- |
| Source | Approved Oracle redo/archive log stream captured through Debezium Change Data Capture (CDC) |
| Event key | Stable business aggregate identifier, such as `account_id`, `agreement_id`, `secured_asset_id`, or `exposure_id` |
| Ordering | Consumers preserve ordering per aggregate key and reject older aggregate versions |
| Schema version | Every event carries source schema version and target mapping version |
| Replay | Replay starts from a documented connector offset into migration staging before target writes resume |
| Ownership | Only one side is system of record per entity and migration phase |

### Lag Thresholds

| Flow | Warning | Cutover Blocker |
| --- | --- | --- |
| Reference data | 5 minutes | More than 15 minutes |
| Account and agreement state | 60 seconds | More than 5 minutes |
| Exposure and secured asset state | 30 seconds | More than 2 minutes |
| Transaction history | 30 seconds | More than 2 minutes |
| Audit events | 60 seconds | More than 5 minutes |

### Failure Recovery

| Failure | Recovery |
| --- | --- |
| Missed event | Pause consumers, replay from last verified offset, reconcile affected aggregate range |
| Duplicate event | Use idempotency key and aggregate version to reject duplicate processing |
| Out-of-order event | Quarantine event, wait for missing prior version, alert if gap exceeds threshold |
| Schema drift | Stop connector, route records to staging quarantine, approve mapping update before replay |
| Replay/live collision | Freeze writes for affected aggregate, reconcile legacy and target state, then choose authoritative owner |

## Bidirectional Sync Loop Prevention

Bridge-phase writes must carry an origin marker so migration-generated writes do not echo back into the source side.

| Origin Marker | Example | Handling |
| --- | --- | --- |
| Application context | `migration_origin=legacy_to_target_sync` | Target triggers ignore reverse replication |
| Client tag | `client_identifier=migration-bridge` | Oracle session logic excludes bridge writes |
| Event header | `x-migration-origin=target_to_legacy_sync` | Change Data Capture (CDC) filters prevent looped event publishing |

### Bridge Test Matrix

| Scenario | Expected Result |
| --- | --- |
| Legacy write syncs to target | Target receives one update, no reverse write occurs |
| Target write during rollback window syncs to legacy | Legacy receives one update, no echo event returns |
| Same aggregate updated by old and new code | Conflict is routed to exception workflow |
| Rollback during mixed-version deployment | Bridge remains active until all target-originated writes are reconciled |
