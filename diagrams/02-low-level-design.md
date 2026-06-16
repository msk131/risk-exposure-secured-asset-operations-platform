# Low-Level Design

```mermaid
sequenceDiagram
    participant User as Operations User
    participant Gateway as API Gateway
    participant Txn as Transaction Service
    participant Risk as Risk Exposure Service
    participant Asset as Secured Asset Service
    participant DB as Service Databases
    participant Outbox as Outbox
    participant Kafka as Event Bus
    participant Recon as Reconciliation
    participant Report as Reporting

    User->>Gateway: Submit financial operation
    Gateway->>Txn: Create transaction/instruction
    Txn->>Risk: Validate exposure impact
    Txn->>Asset: Validate secured asset state
    Txn->>DB: Commit transaction state
    Txn->>Outbox: Store domain event
    Outbox->>Kafka: Publish committed event
    Kafka->>Recon: Compare totals and states
    Kafka->>Report: Update operational reporting
    Recon->>DB: Persist breaks and evidence
    Gateway-->>User: Return operation status
```

## Key Controls

| Control | Purpose |
| --- | --- |
| Service-owned data | Avoid shared-schema coupling. |
| Transactional outbox | Prevent lost events after database commit. |
| Idempotency | Prevent duplicate operational instructions. |
| Reconciliation | Validate risk, asset, transaction, and reporting state. |
| Audit trail | Preserve actor, decision, timestamp, and correlation ID. |
| Rollback route | Keep legacy fallback during phased migration. |
