# Data Ownership

Clear ownership prevents the target platform from recreating legacy shared-database coupling.

## Ownership Matrix

| Entity | Owning Service | Read Access Pattern |
| --- | --- | --- |
| Party | Party Service | API or replicated read model |
| Account | Account Service | API or reporting projection |
| Agreement | Agreement Service | API and domain events |
| Exposure | Exposure Service | API and reporting projection |
| Secured Asset | Secured Asset Service | API and allocation events |
| Instruction | Instruction Service | API and lifecycle events |
| Transaction | Account or Transaction capability | API and reporting projection |
| Balance Snapshot | Account Service | API and reporting projection |
| Audit Event | Audit Service | Append-only query API |

## Data Rules

- No service writes directly to another service database.
- Reporting consumes events or read models.
- Audit events are append-only.
- Historical snapshots are retained based on business and regulatory needs.
- Personally identifiable or sensitive fields are classified and protected.

## Schema Evolution

Use an expand-contract approach:

1. Add new schema elements.
2. Deploy code that supports both old and new contracts.
3. Backfill or project data.
4. Move consumers.
5. Remove obsolete schema after sign-off.

