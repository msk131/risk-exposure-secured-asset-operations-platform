# Batch Operations Dashboard

```mermaid
flowchart TB
    FILES[Inbound File Status]
    RUN[Batch Run Status]
    COUNTS[Record Counts]
    EXCEPT[Exception Summary]
    RECON[Reconciliation Result]
    DASH[Legacy Operations Dashboard]
    OPS[Operations User]
    REPORT[Download Report]
    SIGNOFF[Manual Sign-Off]
    RERUN[Manual Rerun Request]

    FILES --> DASH
    RUN --> DASH
    COUNTS --> DASH
    EXCEPT --> DASH
    RECON --> DASH
    DASH --> OPS
    OPS --> REPORT
    OPS --> SIGNOFF
    OPS --> RERUN
```

## Notes

- The dashboard is an operational control surface, not a modern observability system.
- Manual sign-off and rerun decisions create audit and consistency risks.

