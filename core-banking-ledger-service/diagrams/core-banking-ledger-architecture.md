# Core Banking Ledger Architecture

```mermaid
flowchart TB
    CLIENT["Digital Channel / Partner API / Internal Service"]
    GATEWAY["API Gateway\nAuth, rate limit, routing"]
    API["Ledger Service API\nSpring Boot REST"]

    IDEMP["Idempotency Module\nRequest key, replay protection"]
    RISK["Risk And Compliance Hooks\nLimits, velocity, sanctions, fraud"]
    TXN["Transaction Orchestrator\nValidate, reserve, post, reverse"]
    LEDGER["Ledger Domain\nDouble-entry journal"]
    BALANCE["Balance Projection\nCurrent, available, held"]
    AUDIT["Audit Module\nActor, action, reason, trace"]
    RECON["Reconciliation Module\nCounts, totals, outbox state"]

    DB[("PostgreSQL\nAccounts, ledger entries,\ntransactions, holds, outbox")]
    CACHE[("Redis / Valkey\nRead-side cache")]
    OUTBOX["Outbox Publisher\nReliable event dispatch"]
    KAFKA["Kafka / Redpanda\nTransaction events"]
    OBS["Observability\nMetrics, traces, logs"]
    OPS["Operations Dashboard\nSLOs, exceptions, reconciliation"]

    CLIENT --> GATEWAY
    GATEWAY --> API
    API --> IDEMP
    IDEMP --> RISK
    RISK --> TXN
    TXN --> LEDGER
    TXN --> BALANCE
    TXN --> AUDIT
    TXN --> DB
    LEDGER --> DB
    BALANCE --> DB
    BALANCE --> CACHE
    AUDIT --> DB
    RECON --> DB
    DB --> OUTBOX
    OUTBOX --> KAFKA
    API --> OBS
    TXN --> OBS
    DB --> OBS
    KAFKA --> OBS
    OBS --> OPS
    RECON --> OPS
```

## Flow Summary

1. Client submits a money-movement request with an idempotency key.
2. API gateway authenticates, authorizes, rate-limits, and routes the request.
3. Ledger service checks idempotency before doing any financial work.
4. Risk and compliance hooks validate limits, velocity, and policy rules.
5. Transaction orchestrator creates balanced double-entry ledger records.
6. Database commit stores transaction state, ledger entries, audit events, and outbox event.
7. Outbox publisher sends committed events to Kafka or Redpanda.
8. Reconciliation jobs compare ledger totals, transaction counts, balances, and outbox status.
9. Observability dashboards track latency, failures, database pressure, and financial exceptions.

## Key Reliability Patterns

- Idempotency key for every write command.
- Single database transaction for account state, ledger entries, transaction status, audit event, and outbox row.
- Transactional outbox for event publishing.
- Immutable ledger entries with reversal-based correction.
- Reconciliation jobs for financial totals and operational consistency.
- Explicit exception queue for failed reconciliation or event publication.

