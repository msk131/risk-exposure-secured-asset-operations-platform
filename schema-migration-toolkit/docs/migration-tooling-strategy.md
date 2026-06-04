# Migration Tooling Strategy

## Tool Selection

### Flyway

Use when the team prefers SQL-first migrations with simple versioning.

Best for:

- Application-owned schemas
- Straightforward migration history
- Teams comfortable reviewing SQL scripts

### Liquibase

Use when the team needs richer metadata and rollback support.

Best for:

- Enterprise governance
- Multi-format changelogs
- More formal database release controls

## Suggested Folder Layout

```text
schema-migration-toolkit/
├── migrations/
│   ├── V001__create_reference_tables.sql
│   ├── V002__create_operational_tables.sql
│   └── V003__add_audit_columns.sql
├── reconciliation/
│   ├── rules/
│   └── reports/
└── docs/
```

## Pipeline Flow

1. Developer creates migration script.
2. Pull request runs syntax and naming checks.
3. Migration dry run executes in test database.
4. Schema diff is reviewed.
5. Data migration or backfill runs in controlled environment.
6. Reconciliation checks execute.
7. Approval is captured.
8. Production migration runs with monitoring.

