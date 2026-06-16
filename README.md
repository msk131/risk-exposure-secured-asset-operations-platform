# Risk Exposure & Secured Asset Operations Platform

This repository documents a minimal modernization design for a legacy financial operations platform. The focus is risk exposure, secured assets, transactions, reconciliation, reporting, service-owned data, event-driven integration, and controlled migration from legacy systems.

## Design Focus

| Area | Design Point |
| --- | --- |
| Legacy boundary | Identify monolith modules, shared Oracle schemas, batch jobs, file feeds, and manual reconciliation points. |
| Target services | Separate risk exposure, secured asset, transaction, instruction, reconciliation, and reporting capabilities. |
| Data ownership | Move from shared database access to service-owned schemas and explicit integration contracts. |
| Event flow | Publish domain events through outbox-backed Kafka topics for reporting and downstream consumers. |
| Migration safety | Use phased routing, CDC/sync, shadow validation, reconciliation, and rollback controls. |
| Operations | Track SLOs, audit trails, exception queues, release health, and decommission readiness. |

## Diagrams

| Diagram | Purpose |
| --- | --- |
| [diagrams/01-high-level-design.md](diagrams/01-high-level-design.md) | Target modernization architecture. |
| [diagrams/02-low-level-design.md](diagrams/02-low-level-design.md) | Transaction and reconciliation flow. |

## Current Status

Design-only repository. Implementation details are intentionally kept out of scope.
