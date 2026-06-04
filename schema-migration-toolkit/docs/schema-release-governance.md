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

