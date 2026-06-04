# Migration Architecture

This document defines a general migration architecture for moving a legacy finance system to a modern target application.

The pattern can be used for regulated finance domains such as banking, lending, payments, reporting, risk, operations, or customer servicing.

## High-Level Coexistence Architecture

```mermaid
flowchart TB
    CHANNELS["Channels and Integrations"]
    ROUTER["Routing Layer and API Gateway"]
    LEGACY["Legacy Platform"]
    LEGACYDB[("Legacy Database")]
    NEWSVC["New Application Services"]
    NEWDB[("Target Service Databases")]
    CDC["CDC and Data Sync"]
    STAGE[("Migration Staging")]
    RECON["Reconciliation Engine"]
    AUDIT["Immutable Audit Store"]
    OBS["Observability Dashboards"]

    CHANNELS --> ROUTER
    ROUTER --> LEGACY
    ROUTER --> NEWSVC

    LEGACY --> LEGACYDB
    NEWSVC --> NEWDB

    LEGACYDB --> CDC
    CDC --> STAGE
    STAGE --> NEWDB

    LEGACYDB --> RECON
    NEWDB --> RECON
    RECON --> AUDIT

    LEGACY --> OBS
    NEWSVC --> OBS
    CDC --> OBS
    RECON --> OBS
```

## Data Migration Flow

```mermaid
flowchart LR
    SRC[("Legacy Repository")]
    PROFILE["Data Profiling"]
    CLEANSE["Data Cleansing"]
    MAP["Zero-Loss Mapping"]
    STAGE[("Migration Staging")]
    VALIDATE["Validation Rules"]
    TARGET[("Target Databases")]
    RECON["Reconciliation Reports"]
    EXCEPT["Exception Queue"]

    SRC --> PROFILE
    PROFILE --> CLEANSE
    CLEANSE --> MAP
    MAP --> STAGE
    STAGE --> VALIDATE
    VALIDATE --> TARGET
    SRC --> RECON
    TARGET --> RECON
    VALIDATE --> EXCEPT
    RECON --> EXCEPT
```

## Cutover Sequence

```mermaid
sequenceDiagram
    participant User
    participant Gateway
    participant Legacy
    participant NewApp
    participant Reconciliation
    participant Audit

    User->>Gateway: Request
    Gateway->>Legacy: Existing flow
    Gateway->>NewApp: Shadow or migrated flow
    Legacy-->>Reconciliation: Legacy output
    NewApp-->>Reconciliation: New output
    Reconciliation-->>Audit: Validation result
    Audit-->>Gateway: Cutover readiness
    Gateway->>NewApp: Shift eligible traffic
```

## Architecture Decisions

- Use phased migration instead of big-bang replacement.
- Use API-first routing to decouple channels from legacy database internals.
- Use parallel run for high-risk business flows.
- Use reconciliation reports before production cutover.
- Use immutable audit events for migration evidence.
- Use encryption and least-privilege access for migration jobs.
