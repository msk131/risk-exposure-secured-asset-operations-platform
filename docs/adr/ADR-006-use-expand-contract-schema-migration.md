# ADR-006: Use Expand-Contract Schema Migration

## Status

Accepted

## Context

Banking platforms cannot stop writes for risky schema changes. Rolling deployments mean old and new code versions may access the same database during a release.

## Decision

Use expand-contract for all non-trivial schema changes. Expand with backward-compatible structures, bridge or backfill data, deploy compatible code, shift reads/writes, and contract obsolete structures only after sign-off.

## Decision Drivers

- Avoid downtime during database releases.
- Support mixed application versions during rolling deployments.
- Enable rollback before irreversible contraction.
- Preserve audit evidence for database changes.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Big-bang schema change | Simple release plan | Downtime and rollback risk |
| In-place destructive change | Fast implementation | Breaks old code and makes rollback hard |
| Expand-contract | Supports zero downtime | Requires longer lifecycle and governance |

## Consequences

- New columns must be nullable or have safe defaults initially.
- Backfills must be idempotent and restartable.
- Contract phase requires consumer sign-off.
- Rollback plans must handle writes made by new code.

## Rollback Or Reversal

During expand or bridge, route traffic back to old code and continue syncing from new to old if needed. After contract, rollback becomes compensation or restore-based and requires explicit approval.

## Evidence Required

- Migration script dry run
- Backfill verification
- Consumer sign-off
- Reconciliation report
- Contract approval

