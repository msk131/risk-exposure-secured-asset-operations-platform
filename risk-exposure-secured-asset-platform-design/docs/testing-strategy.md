# Testing Strategy

The target platform should use layered testing to protect business behavior, data correctness, performance, security, and migration safety.

## Test Layers

| Test Type | Tools | Purpose |
| --- | --- | --- |
| Unit tests | JUnit 5, Mockito | Validate service logic, validators, mappers, calculations |
| Integration tests | Testcontainers | Validate service with PostgreSQL, Kafka, Redis, adapters |
| Contract tests | Pact or OpenAPI validation | Protect REST and event contracts |
| E2E tests | Playwright | Validate UI and API journeys |
| Migration tests | Custom reconciliation checks | Validate source-target mapping and data integrity |
| Performance tests | k6, Gatling, or JMeter | Validate latency, throughput, batch windows, API limits |
| Security tests | SAST, dependency scanning, secret scanning | Prevent common security and supply-chain issues |
| Resilience tests | Fault injection, retry/timeout tests | Validate failure handling, backpressure, and recovery |
| Data quality tests | Great Expectations-style checks or custom jobs | Validate completeness, accuracy, and referential integrity |

## Critical Test Scenarios

- Portfolio view loads correct holdings and valuations.
- Secured asset allocation is idempotent.
- Exposure calculation produces expected outputs.
- Reconciliation identifies missing, duplicate, and mismatched records.
- RBAC prevents unauthorized operations.
- CDC/delta sync handles replay and duplicate events.
- Blue/green or canary release does not interrupt active users.
- Database migration follows expand-contract rules.

## Performance Test Focus

- API latency under peak load.
- Kafka/MSK consumer lag.
- Database query response time.
- Batch and reconciliation duration.
- Cache hit rate.
- E2E operational dashboard responsiveness.

