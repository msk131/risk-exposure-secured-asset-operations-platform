# Core Domain Entity Relationship Diagram

This Entity Relationship Diagram (ERD) models the core entities for a Risk Exposure & Secured Asset Operations Platform. It separates mutable operational records from append-only ledger and audit records, and adds bitemporal fields for financial traceability.

```mermaid
erDiagram
    PARTY ||--o{ ACCOUNT : owns
    PARTY ||--o{ AGREEMENT : signs
    AGREEMENT ||--o{ EXPOSURE : governs
    AGREEMENT ||--o{ INSTRUCTION : authorizes
    ACCOUNT ||--o{ TRANSACTION : records
    ACCOUNT ||--o{ BALANCE_SNAPSHOT : has
    PRODUCT ||--o{ EXPOSURE : classifies
    SECURED_ASSET ||--o{ ASSET_ALLOCATION : supplies
    SECURED_ASSET ||--o{ ASSET_ELIGIBILITY_RULE : constrained_by
    EXPOSURE ||--o{ ASSET_ALLOCATION : requires
    EXPOSURE ||--o{ EXPOSURE_CALCULATION : calculated_as
    ASSET_ALLOCATION ||--o{ ALLOCATION_LEDGER_ENTRY : records
    INSTRUCTION ||--o{ AUDIT_EVENT : produces
    TRANSACTION ||--o{ AUDIT_EVENT : produces
    TRANSACTION ||--o{ TRANSACTION_LEDGER_ENTRY : records
    EXPOSURE ||--o{ TRANSACTION_OUTBOX : emits
    ASSET_ALLOCATION ||--o{ TRANSACTION_OUTBOX : emits

    PARTY {
        string party_id PK
        string party_type
        string legal_name
        string status
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
        datetime created_at
        datetime updated_at
    }

    ACCOUNT {
        string account_id PK
        string party_id FK
        string account_number
        string account_type
        string status
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
        datetime opened_at
        datetime closed_at
    }

    AGREEMENT {
        string agreement_id PK
        string party_id FK
        string agreement_type
        string status
        date effective_date
        date termination_date
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    PRODUCT {
        string product_id PK
        string product_type
        string product_code
        string eligibility_group
        string status
        int record_version
        datetime valid_from
        datetime valid_to
    }

    EXPOSURE {
        string exposure_id PK
        string agreement_id FK
        string product_id FK
        decimal exposure_amount
        decimal market_value
        date valuation_date
        string status
        string calculation_version
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    SECURED_ASSET {
        string secured_asset_id PK
        string product_id FK
        decimal available_quantity
        decimal available_value
        string location_code
        string status
        string custodian_reference
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    ASSET_ALLOCATION {
        string asset_allocation_id PK
        string secured_asset_id FK
        string exposure_id FK
        decimal allocated_quantity
        decimal allocated_value
        datetime allocated_at
        string status
        string allocation_reason
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    ASSET_ELIGIBILITY_RULE {
        string rule_id PK
        string secured_asset_id FK
        string product_id FK
        string rule_version
        string eligibility_status
        decimal haircut_percent
        datetime valid_from
        datetime valid_to
    }

    EXPOSURE_CALCULATION {
        string calculation_id PK
        string exposure_id FK
        string calculation_version
        decimal input_market_value
        decimal exposure_amount
        decimal collateral_required
        string rounding_mode
        datetime calculated_at
        datetime valid_from
        datetime transaction_time
    }

    ALLOCATION_LEDGER_ENTRY {
        string ledger_entry_id PK
        string asset_allocation_id FK
        string entry_type
        decimal quantity_delta
        decimal value_delta
        string currency
        string correlation_id
        datetime event_at
        datetime transaction_time
    }

    INSTRUCTION {
        string instruction_id PK
        string agreement_id FK
        string instruction_type
        string channel
        string status
        datetime requested_at
        datetime completed_at
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    TRANSACTION {
        string transaction_id PK
        string account_id FK
        string transaction_type
        decimal amount
        string currency
        datetime transaction_at
        string status
        string source_system
        string correlation_id
        int record_version
        datetime valid_from
        datetime valid_to
        datetime transaction_time
    }

    TRANSACTION_LEDGER_ENTRY {
        string ledger_entry_id PK
        string transaction_id FK
        string entry_type
        decimal amount
        string currency
        string account_id
        string correlation_id
        datetime event_at
        datetime transaction_time
    }

    BALANCE_SNAPSHOT {
        string balance_snapshot_id PK
        string account_id FK
        decimal ledger_balance
        decimal available_balance
        string currency
        datetime snapshot_at
        string source_system
        string correlation_id
        datetime valid_from
        datetime transaction_time
    }

    AUDIT_EVENT {
        string audit_event_id PK
        string event_source
        string entity_type
        string entity_id
        string action
        string actor_id
        string correlation_id
        string causation_id
        datetime event_at
        datetime transaction_time
    }

    TRANSACTION_OUTBOX {
        string event_id PK
        string aggregate_type
        string aggregate_id
        string event_type
        string schema_version
        string payload
        string trace_id
        string publish_status
        datetime created_at
        datetime published_at
    }
```

## Ownership Notes

| Entity | Owning Capability | Record Type |
| --- | --- | --- |
| `PARTY` | Party Service | Mutable operational record with bitemporal history fields |
| `ACCOUNT`, `BALANCE_SNAPSHOT`, `TRANSACTION` | Account Service | Operational record and point-in-time snapshots |
| `AGREEMENT` | Agreement Service | Operational agreement lifecycle record |
| `EXPOSURE`, `EXPOSURE_CALCULATION` | Exposure Service | Operational exposure state plus calculation evidence |
| `SECURED_ASSET`, `ASSET_ELIGIBILITY_RULE`, `ASSET_ALLOCATION` | Secured Asset Service | Operational asset inventory, rules, and allocation state |
| `TRANSACTION_LEDGER_ENTRY`, `ALLOCATION_LEDGER_ENTRY` | Ledger/Audit capability | Append-only financial movement evidence |
| `AUDIT_EVENT` | Audit Service | Append-only business and technical audit evidence |
| `TRANSACTION_OUTBOX` | Each writing service | Local transactional event handoff for Change Data Capture (CDC) publishing |

## Bitemporal Field Definitions

| Field | Definition |
| --- | --- |
| `valid_from` | Business-effective start time for the record state |
| `valid_to` | Business-effective end time for the record state |
| `transaction_time` | Time the platform recorded the state change |
| `record_version` | Monotonic version used for optimistic locking, ordering, and reconciliation |
| `correlation_id` | Identifier linking Application Programming Interface (API), event, audit, and reconciliation records for one business action |
