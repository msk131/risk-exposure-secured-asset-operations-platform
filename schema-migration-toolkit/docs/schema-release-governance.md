# Schema Release Governance

## Governance Checklist

- Migration has a unique version.
- Script is repeatable in lower environments.
- Impacted tables and consumers are identified.
- Data volume and runtime are estimated.
- Rollback or compensation plan exists.
- Reconciliation checks are defined.
- Audit and approval evidence is captured.
- Monitoring is ready for production release.

## Change Risk Levels

| Risk Level | Examples | Required Controls |
| --- | --- | --- |
| Low | Add nullable column, add comment, create view | Peer review and dry run |
| Medium | Add index, add table, backfill non-critical data | Performance review and rollback plan |
| High | Drop column, change data type, migrate critical data | Approval, reconciliation, rollback rehearsal |

## Expand-Contract Governance

1. Expand schema without breaking existing consumers.
2. Deploy code that supports old and new structures.
3. Backfill or synchronize data.
4. Move consumers gradually.
5. Contract old schema only after sign-off.

## Foreign Key And Constraint Rollout Strategy

| Constraint Stage | Standard | Why |
| --- | --- | --- |
| Expand | Create new foreign keys nullable or disabled where legacy writes are still active | Prevent mixed-version inserts from failing |
| Bridge | Use `DEFERRABLE INITIALLY DEFERRED` where parent and child writes can arrive in one transaction | Avoid transient parent-child ordering failures |
| Backfill | Validate orphan counts before enabling enforcement | Prove old data is safe |
| Validate | Use online or non-blocking validation where the database supports it | Avoid production lock escalation |
| Contract | Enforce `NOT NULL` and validated foreign keys only after consumer sign-off | Make the final model strict after migration risk is gone |

## Deterministic Write Ordering

All services must update parent-child aggregates in the same order:

1. Reference or party record.
2. Account or agreement parent.
3. Exposure or secured asset operational state.
4. Allocation, transaction, or instruction child.
5. Ledger and audit records.
6. Outbox event.

## Constraint Validation Evidence

- Orphan-row query output.
- Row counts before and after backfill.
- Lock and runtime estimate.
- Rollback or compensation script.
- Business owner approval for rejected records.
