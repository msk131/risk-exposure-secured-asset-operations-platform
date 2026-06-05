# Risk Exposure & Secured Asset Platform Design

This project defines the target-state platform architecture.

## What This Project Solves

The legacy platform needs to evolve into a service-oriented architecture with clear data ownership, controlled APIs, event-driven integration, auditability, observability, security, and zero-downtime release patterns.

## Core Platform Scope

- Party, account, agreement, exposure, secured asset, instruction, reporting, and audit services.
- Service-owned databases instead of shared mutable schemas.
- Event-driven reporting and audit flows.
- Transactional outbox, idempotency, saga, shadow testing, and Kafka resiliency controls.
- Cloud, deployment, testing, security, observability, and operational readiness.

## Key Artifacts

| Artifact | Purpose |
| --- | --- |
| [service-design.md](docs/service-design.md) | Service boundaries, API categories, and cross-cutting requirements |
| [data-ownership.md](docs/data-ownership.md) | Service ownership for core data entities |
| [microservice-database-strategy.md](docs/microservice-database-strategy.md) | Service-owned database strategy |
| [transactional-outbox-and-event-contract.md](docs/transactional-outbox-and-event-contract.md) | Event publishing contract and outbox table |
| [idempotency-and-deduplication-standard.md](docs/idempotency-and-deduplication-standard.md) | Safe retry and duplicate protection |
| [saga-state-machine.md](docs/saga-state-machine.md) | Cross-service workflow and compensation model |
| [shadow-testing-and-traceability.md](docs/shadow-testing-and-traceability.md) | Parallel validation, side-effect blocking, and trace context |
| [kafka-resiliency-policy.md](docs/kafka-resiliency-policy.md) | Retry, Dead Letter Queue, ordering, and version checks |
| [security-lineage-slo-chaos.md](docs/security-lineage-slo-chaos.md) | Security, lineage, service objectives, and resilience verification |
| [zero-downtime-strategy.md](docs/zero-downtime-strategy.md) | Deployment and cutover safety |
| [testing-strategy.md](docs/testing-strategy.md) | Test layers and critical scenarios |

## Diagrams

- [core-domain-erd.md](diagrams/core-domain-erd.md)
- [target-cloud-architecture.md](diagrams/target-cloud-architecture.md)
- [service-interaction-flow.md](diagrams/service-interaction-flow.md)
- [event-driven-reporting.md](diagrams/event-driven-reporting.md)
- [deployment-view.md](diagrams/deployment-view.md)
- [zero-downtime-release-flow.md](diagrams/zero-downtime-release-flow.md)
