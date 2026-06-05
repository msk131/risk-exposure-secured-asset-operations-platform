# ADR-002: Use Transactional Outbox For Domain Event Publishing

## Status

Accepted

## Context

Microservices must update their local database and publish domain events. Publishing directly to Kafka inside a business transaction can fail after the database commit, causing downstream services to miss critical financial state.

## Decision

Every critical write service must persist domain events into a local `transaction_outbox` table in the same database transaction as the business state change. A Change Data Capture (CDC) publisher then streams outbox records to Kafka.

## Decision Drivers

- Prevent dual-write data loss.
- Preserve auditability of emitted events.
- Support replay after broker or network failure.
- Keep service write logic locally transactional.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Direct Kafka publish | Simple code path | Database commit can succeed while event publish fails |
| Two-phase commit | Strong atomicity | Operationally heavy and brittle across heterogeneous systems |
| Transactional outbox | Local atomicity and replay | Requires outbox cleanup, publisher monitoring, and duplicate handling |

## Consequences

- Consumers must support at-least-once delivery.
- Outbox delay becomes an operational metric.
- Event envelope and schema versioning must be standardized.
- Outbox cleanup must retain enough evidence for audit and replay.

## Rollback Or Reversal

If outbox publishing fails, stop downstream consumers, replay unpublished records by `created_at` and aggregate version, and reconcile target read models before traffic resumes.

## Evidence Required

- Outbox table definition
- Event envelope contract
- Consumer idempotency proof
- Outbox publishing dashboard
- Dead Letter Queue (DLQ) policy

