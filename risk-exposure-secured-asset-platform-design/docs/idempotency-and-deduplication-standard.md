# Idempotency And Deduplication Standard

Critical write operations must be safe to retry. This protects channels, services, event consumers, and migration replay from duplicate financial actions.

## Idempotency Key Format

```text
<channel>:<operation>:<business-reference>:<version>
```

Example:

```text
branch:allocate-secured-asset:instruction-456:3
```

## Service Table

```sql
CREATE TABLE idempotency_record (
    idempotency_key VARCHAR(250) PRIMARY KEY,
    operation_name VARCHAR(120) NOT NULL,
    business_reference VARCHAR(160) NOT NULL,
    request_hash VARCHAR(128) NOT NULL,
    response_hash VARCHAR(128),
    status VARCHAR(40) NOT NULL,
    first_seen_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,
    expires_at TIMESTAMPTZ NOT NULL,
    correlation_id VARCHAR(120) NOT NULL
);
```

## Check-And-Set Flow

1. Receive write request with idempotency key.
2. Canonicalize the request payload and calculate `request_hash`.
3. Insert `idempotency_record` with status `IN_PROGRESS`.
4. If insert fails because the key already exists, compare `request_hash`.
5. Return the stored response for an identical replay.
6. Reject the request if the same key has a different payload.
7. Store the final response hash and status after business commit.

## Retention

| Operation Type | Minimum Retention |
| --- | --- |
| Financial allocation, substitution, settlement-style instruction | 7 years or aligned with audit retention policy |
| Channel write request | 72 hours minimum |
| Event consumer deduplication | 72 hours minimum or topic retention window |
| Migration replay deduplication | Full migration window plus sign-off period |

## Metrics

- `idempotency_accepted_total`
- `idempotency_replayed_total`
- `idempotency_conflict_total`
- `idempotency_expired_total`

