# Schema Design Guide

## Design Principles

- Model business entities explicitly.
- Keep data ownership clear.
- Avoid shared mutable tables across services.
- Include lifecycle and audit fields for operational records.
- Separate operational tables from reporting read models.
- Design for traceability, migration safety, and reconciliation.

## Naming Standards

| Object | Convention | Example |
| --- | --- | --- |
| Table | Singular business noun | `account` |
| Primary key | `<table>_id` | `account_id` |
| Foreign key | Referenced table key | `party_id` |
| Status column | Explicit lifecycle state | `account_status` |
| Audit fields | Consistent metadata | `created_at`, `updated_at` |

## Standard Operational Columns

- Primary key
- Business reference
- Lifecycle status
- Created timestamp
- Created by
- Updated timestamp
- Updated by
- Version for optimistic locking where needed

## Anti-Patterns To Remove

- Multiple applications writing to the same table.
- Business rules hidden in unversioned stored procedures.
- Reporting directly against operational write tables.
- Columns with undocumented encoded meanings.
- Batch updates without audit context.

