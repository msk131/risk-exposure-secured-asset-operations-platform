# High-Level Design

```mermaid
flowchart TB
    CHANNELS["Channels / Operations Users / Partners"]
    ROUTER["API Gateway\nrouting, auth, rate limits"]
    LEGACY["Legacy Finance Platform\nmonolith, Oracle, batch jobs"]
    CDC["CDC / Coexistence Sync"]

    RISK["Risk Exposure Service"]
    ASSET["Secured Asset Service"]
    TXN["Transaction Service"]
    INSTR["Instruction Service"]
    RECON["Reconciliation Service"]
    REPORT["Reporting Service"]

    DB1[("Risk DB")]
    DB2[("Asset DB")]
    DB3[("Transaction DB")]
    OUTBOX["Transactional Outbox"]
    KAFKA["Kafka / Event Bus"]
    OBS["Observability\nlogs, metrics, traces"]

    CHANNELS --> ROUTER
    ROUTER --> LEGACY
    ROUTER --> RISK
    ROUTER --> ASSET
    ROUTER --> TXN
    ROUTER --> INSTR
    LEGACY --> CDC
    CDC --> RISK
    CDC --> ASSET
    CDC --> TXN
    RISK --> DB1
    ASSET --> DB2
    TXN --> DB3
    RISK --> OUTBOX
    ASSET --> OUTBOX
    TXN --> OUTBOX
    OUTBOX --> KAFKA
    KAFKA --> RECON
    KAFKA --> REPORT
    RECON --> OBS
    REPORT --> OBS
    RISK --> OBS
    ASSET --> OBS
    TXN --> OBS
```

## Notes

- Legacy and target systems coexist during migration.
- Service-owned databases replace shared schema coupling.
- Domain events feed reconciliation, reporting, and downstream consumers.
- Observability and reconciliation are part of the migration control plane.
