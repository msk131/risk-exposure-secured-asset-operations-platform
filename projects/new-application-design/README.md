# New Application Design

This project defines the target-state design for a modern Risk Exposure & Secured Asset Operations Platform.

## Design Objectives

- Create an API-first platform that supports web, mobile, branch, and partner channels.
- Replace tightly coupled legacy behavior with domain-aligned services.
- Give services clear ownership of their data.
- Support transaction history, account balances, agreements, exposures, secured assets, instructions, and audit trails.
- Enable event-driven reporting and integration.
- Build observability, security, and DevSecOps into the platform from the start.

## Target Architecture

```mermaid
flowchart LR
    subgraph Channels
        WEB[Web]
        MOB[Mobile]
        BRANCH[Branch / Assisted Channel]
        PARTNER[Partner / Third Party]
    end

    subgraph Edge
        APIGW[API Gateway]
        IAM[Identity and Access Control]
    end

    subgraph Services
        PARTY[Party Service]
        ACCOUNT[Account Service]
        AGREEMENT[Agreement Service]
        EXPOSURE[Exposure Service]
        ASSET[Secured Asset Service]
        INSTRUCTION[Instruction Service]
        REPORTING[Reporting Service]
        AUDIT[Audit Service]
    end

    subgraph Data
        PARTYDB[(Party DB)]
        ACCOUNTDB[(Account DB)]
        AGREEMENTDB[(Agreement DB)]
        EXPOSUREDB[(Exposure DB)]
        ASSETDB[(Secured Asset DB)]
        AUDITDB[(Immutable Audit Store)]
        READMODEL[(Reporting Read Model)]
    end

    BUS[Event Bus]

    WEB --> APIGW
    MOB --> APIGW
    BRANCH --> APIGW
    PARTNER --> APIGW
    APIGW --> IAM

    APIGW --> PARTY
    APIGW --> ACCOUNT
    APIGW --> AGREEMENT
    APIGW --> EXPOSURE
    APIGW --> ASSET
    APIGW --> INSTRUCTION
    APIGW --> REPORTING

    PARTY --> PARTYDB
    ACCOUNT --> ACCOUNTDB
    AGREEMENT --> AGREEMENTDB
    EXPOSURE --> EXPOSUREDB
    ASSET --> ASSETDB
    AUDIT --> AUDITDB
    REPORTING --> READMODEL

    PARTY --> BUS
    ACCOUNT --> BUS
    AGREEMENT --> BUS
    EXPOSURE --> BUS
    ASSET --> BUS
    INSTRUCTION --> BUS
    BUS --> AUDIT
    BUS --> READMODEL
```

## Domain Services

| Service | Responsibility |
| --- | --- |
| Party Service | Customer, counterparty, legal entity, or participant reference data |
| Account Service | Account profile, account status, and balance context |
| Agreement Service | Agreement rules, eligibility, and lifecycle |
| Exposure Service | Risk exposure, position, valuation, and obligation view |
| Secured Asset Service | Eligible assets, inventory, allocation, and substitution |
| Instruction Service | Operational instructions, workflow status, and settlement-style actions |
| Reporting Service | Read models, operational reports, and channel-safe projections |
| Audit Service | Immutable business and technical audit events |

## Data Modeling

ER diagrams live in [diagrams](diagrams/).

Start with [core-domain-erd.md](diagrams/core-domain-erd.md).

## API Design Principles

- Expose business capabilities through REST APIs.
- Keep APIs versioned and contract-tested.
- Avoid direct channel access to service databases.
- Use idempotency keys for write operations.
- Emit domain events for important lifecycle changes.
- Keep audit and trace identifiers across service calls.

## Omnichannel Experience

The target platform should provide consistent data and workflows across:

- Web
- Mobile
- Branch or assisted channels
- Partner and third-party integrations
- Operational dashboards

Channel parity depends on a consistent core data model, clear API contracts, and controlled read models.

