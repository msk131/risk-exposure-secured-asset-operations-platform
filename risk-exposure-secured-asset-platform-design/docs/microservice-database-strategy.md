# Microservice Database Strategy

Each microservice should own its operational data. Shared database access should be removed during modernization.

## Recommended Database Choices

| Capability | Recommended Database | Notes |
| --- | --- | --- |
| Party / Client Service | Aurora PostgreSQL | Relational reference data, constraints, ownership history |
| Account / Portfolio Service | Aurora PostgreSQL | Portfolio lifecycle, account relationships, operational state |
| Holdings Service | Aurora PostgreSQL | Partition by business date, portfolio, or asset where volume requires |
| Exposure Service | Aurora PostgreSQL | Calculation inputs, exposure snapshots, obligation status |
| Secured Asset Service | Aurora PostgreSQL | Inventory, eligibility, allocation, substitution |
| Valuation Service | Aurora PostgreSQL + Redis | Persist valuations, cache latest calculated views |
| Reconciliation Service | Aurora PostgreSQL | Source-target comparisons, exception workflow, sign-off |
| Audit Service | DynamoDB or append-only PostgreSQL | High-volume immutable audit events |
| Reporting Service | Aurora PostgreSQL read model + OpenSearch | Query-optimized projections and search |
| Market Data Service | Aurora PostgreSQL or time-series optimized store | Depends on volume and retention requirements |

## Database Design Principles

- One service owns writes for its database.
- Other services access data through APIs, events, or read models.
- Reporting does not query operational write tables directly.
- Audit records are append-only.
- Schema changes use expand-contract migration.
- Critical entities use stable identifiers and lifecycle status fields.

## Migration Considerations

- Start with read models and reporting projections before moving critical writes.
- Use CDC or delta sync from the legacy Oracle database during coexistence.
- Validate source-target alignment through reconciliation.
- Shift system-of-record ownership by entity and phase.
- Retire legacy tables only after traffic, consumers, and operational ownership have moved.

