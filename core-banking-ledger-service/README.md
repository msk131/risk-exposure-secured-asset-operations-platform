# Core Banking Ledger Service

This project designs and builds a high-performance Java-based banking microservice for ledger-backed money movement, account balance integrity, idempotent transaction processing, auditability, and operational resilience.

The service is intentionally focused on one of the most critical banking capabilities: **safe movement of money**. It is not a generic CRUD banking API. Every transaction must be traceable, idempotent, balanced, observable, and recoverable.

## Why This Project Matters

Banks depend on ledger systems to record financial truth. If a ledger service loses a transaction, duplicates a payment, miscalculates a balance, or cannot explain how money moved, the impact can become regulatory, financial, operational, and reputational.

This project demonstrates how to design a banking-grade microservice that handles:

- Financial transaction correctness.
- Double-entry ledger accounting.
- Idempotent API writes.
- High-throughput transaction processing.
- Strong audit trails.
- Event-driven integration.
- Reconciliation and exception handling.
- Operational monitoring and rollback-safe recovery.

## What We Are Building

The first version is a **Core Banking Ledger Service** that supports accounts, balances, transfers, holds, ledger entries, transaction status tracking, audit events, and reliable event publishing.

Primary capabilities:

| Capability | Purpose |
| --- | --- |
| Account ledger | Maintain financial account state using immutable ledger entries. |
| Double-entry transfer | Debit one account and credit another with balanced journal entries. |
| Idempotency | Prevent duplicate financial movement from client retries or network timeouts. |
| Transaction lifecycle | Track pending, posted, rejected, reversed, and failed states. |
| Balance query | Return available, current, and held balances with low latency. |
| Outbox publishing | Publish transaction events reliably after database commit. |
| Audit trail | Capture who did what, when, from where, and why. |
| Reconciliation | Compare ledger totals, transaction counts, and event publication state. |
| Risk hooks | Allow transaction limit, velocity, fraud, and compliance checks before posting. |

## How We Will Build It

The service will be built as a production-grade Java microservice using a pragmatic 2026 stack:

| Layer | Technology |
| --- | --- |
| Runtime | Java 25 LTS |
| Framework | Spring Boot 4.x |
| Architecture | Spring Modulith first, deployable as a microservice |
| API | REST for commands and queries, OpenAPI for contracts |
| Persistence | PostgreSQL with Flyway migrations |
| Data access | jOOQ for ledger and reporting queries, Spring Data JDBC where useful |
| Messaging | Kafka or Redpanda with transactional outbox |
| Cache | Redis or Valkey for safe read-side acceleration |
| Security | Spring Security, OAuth2/OIDC JWT, mTLS-ready service boundaries |
| Observability | Micrometer, OpenTelemetry, Prometheus, Grafana, Loki |
| Testing | JUnit 5, Testcontainers, ArchUnit, JMH, k6 or Gatling |
| Delivery | Docker, Kubernetes-ready manifests, GitHub Actions, SBOM, vulnerability scanning |

## Design Principles

- **Ledger is append-only**: never mutate financial history; correct through reversal or adjustment.
- **Money movement is atomic**: debit and credit entries must commit together or not at all.
- **Every write is idempotent**: client retries must not duplicate transfers.
- **Database constraints protect invariants**: correctness does not live only in application code.
- **Events are published after commit**: use outbox pattern instead of best-effort messaging.
- **Read models can be optimized**: financial truth remains in the ledger.
- **Audit is a first-class feature**: every decision must be traceable.
- **Performance must be measured**: p95/p99 latency, throughput, lock contention, and database pressure are tracked from the beginning.

## Core APIs

Initial REST API surface:

```text
POST /api/v1/accounts
GET  /api/v1/accounts/{accountId}
GET  /api/v1/accounts/{accountId}/balances

POST /api/v1/transfers
GET  /api/v1/transfers/{transferId}
POST /api/v1/transfers/{transferId}/reverse

POST /api/v1/accounts/{accountId}/holds
DELETE /api/v1/accounts/{accountId}/holds/{holdId}

POST /api/v1/reconciliation/runs
GET  /api/v1/reconciliation/runs/{runId}
```

## Critical Invariants

The service must enforce these rules:

- A posted transfer must always create balanced debit and credit ledger entries.
- A transaction request with the same idempotency key must return the original result.
- An account cannot spend more than its available balance unless explicitly configured.
- Ledger entries are immutable after posting.
- Reversal creates new compensating entries; it does not edit original entries.
- Every state change must produce an audit event.
- Every committed transaction event must eventually be published or visible in the outbox exception queue.

## Project Structure

```text
core-banking-ledger-service/
├── README.md
├── plan.md
└── diagrams/
    └── core-banking-ledger-architecture.md
```

Future implementation folders:

```text
src/main/java
src/main/resources/db/migration
src/test/java
docker
k8s
load-tests
docs
```

## Current Status

This project is starting with planning and architecture before code. The next step is to create the Spring Boot project skeleton, database schema, and first vertical slice for account creation and ledger-backed transfer posting.

