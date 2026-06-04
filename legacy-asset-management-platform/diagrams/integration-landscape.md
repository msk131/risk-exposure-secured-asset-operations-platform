# Integration Landscape

```mermaid
flowchart LR
    TRADING[Trading System]
    MARKET[Market Data Provider]
    REF[Reference Data System]
    CASH[Cash System]
    LEGACY[Legacy Asset Management Platform]
    REPORTING[Reporting Platform]
    RISK[Risk Platform]
    AUDIT[Audit Archive]
    OPS[Operations Dashboard]

    TRADING -- File / MQ-Style --> LEGACY
    MARKET -- SFTP-Style Price File --> LEGACY
    REF -- Reference File Feed --> LEGACY
    CASH -- Cash Balance Feed --> LEGACY

    LEGACY -- Extracts --> REPORTING
    LEGACY -- File / MQ-Style --> RISK
    LEGACY -- Status Tables --> OPS
    LEGACY -- Audit Extract --> AUDIT
```

## Notes

- Integration is a mix of file, SOAP/XML-style, MQ-style, and direct reporting patterns.
- Contract ownership is weak compared with modern API/event-driven designs.

