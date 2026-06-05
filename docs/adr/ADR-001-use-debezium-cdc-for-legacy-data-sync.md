# ADR-001: Use Debezium Change Data Capture For Legacy Data Synchronization

## Status

Accepted

## Context

During phased migration, the legacy Oracle platform and the target service-owned data stores must coexist. The target platform needs current data without forcing all legacy applications to change at once.

## Decision

Use Debezium-based Change Data Capture (CDC) to stream approved legacy Oracle changes into migration staging and target projections. Snapshot backfill remains the initial load mechanism; CDC is the ongoing delta synchronization mechanism.

## Decision Drivers

- Avoid direct dual writes from legacy application code wherever possible.
- Preserve ordering, replayability, and audit evidence for migrated entities.
- Support parallel run and reconciliation before traffic cutover.
- Reduce coupling between the legacy monolith and target services.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Batch delta jobs | Simple to operate initially | Higher staleness, weaker ordering, harder replay |
| Application dual write | Immediate target update | Dual-write failure can create data loss or split-brain state |
| Debezium Change Data Capture (CDC) | Log-based, replayable, auditable | Requires connector monitoring, schema drift handling, and lag thresholds |

## Consequences

- CDC connector lag becomes a cutover gate.
- Schema changes must be coordinated with CDC schema registry rules.
- Consumers must be idempotent because CDC delivery can be at least once.
- Replay procedures must be tested before production migration.

## Rollback Or Reversal

Pause CDC connectors, stop target consumers, replay from the last approved offset into a quarantined staging area, reconcile against Oracle snapshots, and only resume target projection after variance is approved.

## Evidence Required

- CDC source table list and ownership map
- Connector lag dashboard
- Replay runbook
- Reconciliation report
- Schema compatibility test

