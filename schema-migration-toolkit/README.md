# Schema Migration Toolkit

This project defines the schema migration controls needed when moving from shared legacy tables to service-owned data models.

## What This Project Solves

Financial migrations require database changes without breaking live applications. This project covers expand-contract migration, versioned schema releases, coexistence data sync, access migration, data quality checks, and release governance.

## Key Artifacts

| Artifact | Purpose |
| --- | --- |
| [schema-design-guide.md](docs/schema-design-guide.md) | Data modeling standards and anti-patterns to remove |
| [migration-tooling-strategy.md](docs/migration-tooling-strategy.md) | Flyway/Liquibase-style migration workflow |
| [schema-release-governance.md](docs/schema-release-governance.md) | Approval, rollback, constraint rollout, and expand-contract controls |
| [coexistence-data-sync.md](docs/coexistence-data-sync.md) | Change Data Capture, replay, lag thresholds, and bridge-loop prevention |
| [auth-rbac-migration.md](docs/auth-rbac-migration.md) | Authentication and Role-Based Access Control migration |
| [plan.md](plan.md) | Working plan for the toolkit |

## Diagrams

- [schema-evolution-flow.md](diagrams/schema-evolution-flow.md)
- [logical-finance-erd.md](diagrams/logical-finance-erd.md)
- [auth-rbac-migration-flow.md](diagrams/auth-rbac-migration-flow.md)
- [coexistence-data-sync-flow.md](diagrams/coexistence-data-sync-flow.md)
