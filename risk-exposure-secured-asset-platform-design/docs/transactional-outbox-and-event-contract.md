# Transactional Outbox And Event Contract

This document defines the standard event publishing contract for the platform. Services must not publish critical business events directly to Kafka inside an active business transaction.

## Outbox Table

```sql
CREATE TABLE transaction_outbox (
    event_id UUID PRIMARY KEY,
    aggregate_type VARCHAR(80) NOT NULL,
    aggregate_id VARCHAR(120) NOT NULL,
    aggregate_version BIGINT NOT NULL,
    event_type VARCHAR(120) NOT NULL,
    schema_version VARCHAR(40) NOT NULL,
    payload JSONB NOT NULL,
    trace_id VARCHAR(120) NOT NULL,
    correlation_id VARCHAR(120) NOT NULL,
    causation_id VARCHAR(120),
    publish_status VARCHAR(30) NOT NULL DEFAULT 'PENDING',
    retry_count INTEGER NOT NULL DEFAULT 0,
    last_error TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,
    published_at TIMESTAMPTZ
);

CREATE INDEX idx_transaction_outbox_pending
    ON transaction_outbox(created_at)
    WHERE publish_status = 'PENDING';

CREATE UNIQUE INDEX uq_transaction_outbox_aggregate_version
    ON transaction_outbox(aggregate_type, aggregate_id, aggregate_version, event_type);
```

## Event Envelope

```json
{
  "eventId": "uuid",
  "eventType": "AssetAllocated",
  "schemaVersion": "1.0.0",
  "aggregateType": "AssetAllocation",
  "aggregateId": "allocation-123",
  "aggregateVersion": 7,
  "occurredAt": "2026-06-05T10:15:30Z",
  "producer": "secured-asset-service",
  "correlationId": "request-abc",
  "causationId": "instruction-456",
  "traceId": "trace-xyz",
  "migrationPhase": "parallel-run",
  "payload": {}
}
```

## Publishing Rules

- The outbox row must be written in the same local database transaction as the business state change.
- A Change Data Capture (CDC) publisher streams committed outbox rows to Managed Streaming for Apache Kafka (MSK/Kafka).
- Consumers must treat delivery as at least once and perform idempotency checks.
- Event keys must use stable aggregate identifiers to preserve per-aggregate ordering.
- Poison events move to a Dead Letter Queue (DLQ) with trace context and raw payload evidence.

## Operational Metrics

| Metric | Purpose |
| --- | --- |
| `outbox_pending_total` | Number of unpublished outbox records |
| `outbox_oldest_pending_age_seconds` | Detects publishing delay before read models drift |
| `outbox_publish_failures_total` | Counts publishing failures by service and event type |
| `event_consumer_duplicates_total` | Confirms at-least-once delivery handling is active |

