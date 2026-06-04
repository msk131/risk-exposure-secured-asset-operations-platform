# Core Domain ERD

This starter ER diagram models the core entities for a Risk Exposure & Secured Asset Operations Platform.

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
    EXPOSURE ||--o{ ASSET_ALLOCATION : requires
    INSTRUCTION ||--o{ AUDIT_EVENT : produces
    TRANSACTION ||--o{ AUDIT_EVENT : produces

    PARTY {
        string party_id PK
        string party_type
        string legal_name
        string status
        datetime created_at
        datetime updated_at
    }

    ACCOUNT {
        string account_id PK
        string party_id FK
        string account_number
        string account_type
        string status
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
    }

    PRODUCT {
        string product_id PK
        string product_type
        string product_code
        string eligibility_group
        string status
    }

    EXPOSURE {
        string exposure_id PK
        string agreement_id FK
        string product_id FK
        decimal exposure_amount
        decimal market_value
        date valuation_date
        string status
    }

    SECURED_ASSET {
        string secured_asset_id PK
        string product_id FK
        decimal available_quantity
        decimal available_value
        string location_code
        string status
    }

    ASSET_ALLOCATION {
        string asset_allocation_id PK
        string secured_asset_id FK
        string exposure_id FK
        decimal allocated_quantity
        decimal allocated_value
        datetime allocated_at
        string status
    }

    INSTRUCTION {
        string instruction_id PK
        string agreement_id FK
        string instruction_type
        string channel
        string status
        datetime requested_at
        datetime completed_at
    }

    TRANSACTION {
        string transaction_id PK
        string account_id FK
        string transaction_type
        decimal amount
        string currency
        datetime transaction_at
        string status
    }

    BALANCE_SNAPSHOT {
        string balance_snapshot_id PK
        string account_id FK
        decimal ledger_balance
        decimal available_balance
        string currency
        datetime snapshot_at
    }

    AUDIT_EVENT {
        string audit_event_id PK
        string event_source
        string entity_type
        string entity_id
        string action
        string actor_id
        datetime event_at
    }
```

