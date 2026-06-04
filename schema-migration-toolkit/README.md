# Schema Migration Toolkit

This project defines a schema design and migration toolkit for modernizing regulated finance systems while keeping production releases controlled, auditable, and reversible.

The goal is to demonstrate expertise in data modeling, schema evolution, migration automation, data quality validation, reconciliation, and database release governance.

## Toolkit Objectives

- Design logical and physical data models.
- Create ER diagrams for core business domains.
- Establish clear data ownership by capability or service.
- Manage versioned schema migrations through repeatable tooling.
- Use expand-contract patterns for low-risk schema evolution.
- Validate migrated data through reconciliation and quality checks.
- Maintain auditability for database changes.
- Define rollback or compensation strategies for high-risk changes.

## Core Capabilities

| Capability | Purpose |
| --- | --- |
| Data modeling | Define entities, relationships, keys, and ownership |
| Migration tooling | Version and automate database schema changes |
| Expand-contract migration | Support zero-downtime schema evolution |
| Data quality rules | Detect duplicates, corrupt records, missing values, and invalid states |
| Reconciliation | Compare legacy and target records after migration |
| Governance | Add approval, audit, rollback, and release controls |

## Recommended Tooling

| Tool | Best Fit |
| --- | --- |
| Flyway | SQL-first migration versioning |
| Liquibase | Structured changelogs, rollback metadata, governance-heavy environments |
| DB migration pipeline | CI/CD validation, dry run, deployment, post-checks |
| Reconciliation jobs | Source-target validation and exception reporting |
| ERD tooling | Mermaid, draw.io, dbdiagram.io, DBeaver, or SQL Developer Data Modeler |

## Diagrams

- [schema-evolution-flow.md](diagrams/schema-evolution-flow.md)
- [logical-finance-erd.md](diagrams/logical-finance-erd.md)

## Detailed Docs

- [schema-design-guide.md](docs/schema-design-guide.md)
- [migration-tooling-strategy.md](docs/migration-tooling-strategy.md)
- [schema-release-governance.md](docs/schema-release-governance.md)

## Plan

See [plan.md](plan.md).

