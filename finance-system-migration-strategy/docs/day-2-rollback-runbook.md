# Day-2 Phased Migration Rollback Runbook

This runbook defines how to route traffic back to the legacy Oracle platform after target traffic has already started.

## Rollback Triggers

| Trigger | Decision |
| --- | --- |
| Critical reconciliation break | Stop target writes for affected flow |
| Data loss or duplicate financial movement | Immediate rollback and incident bridge |
| Change Data Capture (CDC) lag beyond blocker threshold | Pause cutover and hold traffic shift |
| Unrecoverable target service outage | Route traffic back to legacy |
| Security or privacy exposure | Stop shadow and target flows until evidence is reviewed |

## Decision Authority

Rollback requires approval from:

- Migration lead.
- Business operations owner.
- Production support lead.
- Database owner.
- Security or risk owner when data exposure is involved.

## Sequence

1. Declare rollback and freeze new target traffic.
2. Disable target write routes at the Application Programming Interface (API) gateway.
3. Keep read-only target dashboards available for investigation.
4. Stop target writers and background processors for affected aggregates.
5. Drain pending `transaction_outbox` records or mark them `ROLLBACK_HELD`.
6. Pause Change Data Capture (CDC) consumers from target to legacy, if bridge sync is active.
7. Reconcile target-accepted transactions against legacy Oracle.
8. Apply approved compensating writes to Oracle for any target-only accepted transaction.
9. Verify Oracle sequence, business keys, and aggregate versions are ahead of or equal to target state.
10. Route affected traffic back to the legacy platform.
11. Run smoke tests for account, agreement, exposure, secured asset, transaction, and audit flows.
12. Keep target consumers paused until reconciliation is signed off.
13. Publish rollback completion notice and preserve evidence bundle.

## Target-Accepted Transaction Handling

| State | Action |
| --- | --- |
| Accepted by target, not published to outbox | Compensate or replay into Oracle through approved bridge job |
| Accepted by target, outbox pending | Hold outbox, reconcile business state, then either publish or cancel with audit event |
| Accepted by target, already reflected in legacy | Mark as reconciled and prevent duplicate replay |
| Accepted by target, failed in legacy compensation | Create critical exception and keep aggregate frozen |

## Validation Queries

Each rollback rehearsal must provide queries for:

- Count by entity and status.
- Sum by account, currency, and business date.
- Maximum aggregate version by entity.
- Unpublished outbox records.
- Change Data Capture (CDC) connector offsets.
- Critical exception count.

## Completion Criteria

- Legacy routes are serving the affected business flow.
- No target-only financial transaction remains unreconciled.
- Outbox and Change Data Capture (CDC) positions are documented.
- Business operations confirms workflow continuity.
- Audit evidence is attached to the release record.

