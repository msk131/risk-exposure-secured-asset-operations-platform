# Event-Driven Reporting

```mermaid
flowchart LR
    PARTY[Party Service]
    ACCOUNT[Account Service]
    AGREEMENT[Agreement Service]
    EXPOSURE[Exposure Service]
    ASSET[Secured Asset Service]
    INSTRUCTION[Instruction Service]
    BUS[Event Bus]
    PROJECTOR[Reporting Projector]
    READ[(Reporting Read Model)]
    REPORTS[Reports and Dashboards]
    AUDIT[Audit Service]

    PARTY --> BUS
    ACCOUNT --> BUS
    AGREEMENT --> BUS
    EXPOSURE --> BUS
    ASSET --> BUS
    INSTRUCTION --> BUS
    BUS --> PROJECTOR
    PROJECTOR --> READ
    READ --> REPORTS
    BUS --> AUDIT
```

## Reporting Principles

- Do not query operational service databases directly for broad reporting.
- Use events to maintain read models.
- Track projection lag and replay capability.
- Keep audit and reporting concerns separate.

