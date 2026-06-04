# Cutover Runbook Flow

```mermaid
flowchart TD
    START[Start Cutover Window]
    FREEZE[Change Freeze]
    SNAP[Final Legacy Snapshot]
    MIGRATE[Run Final Migration Jobs]
    SMOKE[Run Smoke Tests]
    RECON[Run Reconciliation]
    DECIDE{Go / No-Go?}
    SHIFT[Shift Traffic Gradually]
    MONITOR[Monitor Business and Technical Metrics]
    COMPLETE[Complete Cutover]
    ROLLBACK[Execute Rollback]
    REVIEW[Post-Cutover Review]

    START --> FREEZE
    FREEZE --> SNAP
    SNAP --> MIGRATE
    MIGRATE --> SMOKE
    SMOKE --> RECON
    RECON --> DECIDE
    DECIDE -- Go --> SHIFT
    SHIFT --> MONITOR
    MONITOR --> COMPLETE
    COMPLETE --> REVIEW
    DECIDE -- No-Go --> ROLLBACK
    ROLLBACK --> REVIEW
```

## Minimum Go Criteria

- Critical reconciliation checks pass.
- Security and audit controls are active.
- Target application health checks pass.
- Rollback path is still valid.
- Support and business owners are available.

