# Service Design

This document describes the target service model for a Risk Exposure & Secured Asset Operations Platform.

## Service Boundaries

| Service | Owns | Publishes |
| --- | --- | --- |
| Party Service | Parties, legal entities, participants, status | `PartyCreated`, `PartyUpdated` |
| Account Service | Accounts, balances, account lifecycle | `AccountOpened`, `BalanceUpdated` |
| Agreement Service | Agreements, rules, eligibility, lifecycle | `AgreementActivated`, `AgreementChanged` |
| Exposure Service | Exposure snapshots, valuations, obligations | `ExposureCalculated`, `ExposureChanged` |
| Secured Asset Service | Asset inventory, eligibility, allocation | `AssetRegistered`, `AssetAllocated` |
| Instruction Service | Operational instructions and workflow status | `InstructionRequested`, `InstructionCompleted` |
| Reporting Service | Read models and report projections | Internal projection updates |
| Audit Service | Immutable audit event store | Audit records |

## API Categories

| API Category | Example |
| --- | --- |
| Reference APIs | Party lookup, product lookup, agreement lookup |
| Operational APIs | Create instruction, allocate asset, update workflow status |
| Query APIs | Exposure view, balance view, allocation status |
| Reporting APIs | Operational report, exception report, audit report |
| Admin APIs | Configuration, rule version, data quality exception management |

## Cross-Cutting Requirements

- Request ID and trace ID on every API call.
- Idempotency key for critical write operations.
- Authorization enforced at API and service layers.
- Audit event emitted for business-significant changes.
- Contract tests for APIs and events.
- Backward-compatible API versioning during migration.

