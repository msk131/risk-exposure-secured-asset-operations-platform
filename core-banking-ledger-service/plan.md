# Core Banking Ledger Service Plan

This plan follows a simple operating rhythm: understand why the capability matters, define what must be true, analyze the risk and architecture, then act in small production-quality increments.

## Phase 0: Why

The purpose of this project is to build a banking-grade service that can safely process financial transactions under real production constraints.

Business reasons:

- Money movement must be correct, explainable, and recoverable.
- Duplicate transfers from retries must be prevented.
- Ledger state must support audit, reporting, reconciliation, and dispute investigation.
- Transaction APIs must remain stable under high concurrency and partial failures.
- Integration events must not be lost after a transaction commits.

Engineering reasons:

- Demonstrates advanced Java and Spring Boot service design.
- Shows database-backed correctness, not only API coding.
- Uses event-driven patterns safely through transactional outbox.
- Proves performance with load tests and observability.
- Creates a strong base for future banking services such as payments, risk, fraud, and reconciliation.

## Phase 1: What

Build the smallest serious version of the ledger service.

Initial domain scope:

- Customer-owned accounts.
- Current and available balances.
- Debit, credit, and transfer transactions.
- Account holds.
- Double-entry ledger entries.
- Transaction lifecycle.
- Idempotency records.
- Audit events.
- Outbox events.
- Reconciliation runs.

Out of scope for first version:

- Card network integration.
- ACH, wire, UPI, SWIFT, or real external payment rails.
- Multi-region active-active deployment.
- Complex fraud ML.
- Full customer onboarding and KYC.

## Phase 2: How

Architecture approach:

- Start as one Spring Boot microservice with internal modular boundaries.
- Use Spring Modulith to keep account, ledger, transaction, audit, outbox, and reconciliation modules clean.
- Use PostgreSQL as the source of truth.
- Use jOOQ for critical ledger queries and reporting queries.
- Use Flyway for schema versioning.
- Use Kafka or Redpanda for downstream transaction events.
- Use Testcontainers so integration tests run against real PostgreSQL and Kafka-compatible infrastructure.

Core modules:

| Module | Responsibility |
| --- | --- |
| account | Account identity, status, product type, and balance view. |
| ledger | Immutable double-entry journal records. |
| transaction | Transfer command processing and lifecycle state. |
| idempotency | Request deduplication and replay-safe response handling. |
| risk | Limit, velocity, and policy checks before posting. |
| audit | Traceable audit event capture. |
| outbox | Reliable event publication after commit. |
| reconciliation | Financial and operational consistency checks. |

## Phase 3: Analyze

Critical risks to design for early:

| Risk | Control |
| --- | --- |
| Duplicate transfer | Idempotency table with unique request key and stored result. |
| Unbalanced ledger | Database constraint plus application invariant check before commit. |
| Overspending | Available balance check with account-level concurrency control. |
| Lost event | Transactional outbox committed with business transaction. |
| Incorrect rollback | Reversal transaction instead of mutating posted ledger entries. |
| Race condition | Optimistic locking or account-level serialization for hot accounts. |
| Slow balance query | Balance projection table and cache with safe invalidation. |
| Weak auditability | Immutable audit event for every command and state transition. |
| Hidden performance bottleneck | Load tests, JFR profiling, p95/p99 dashboards, database query plans. |

Key design decisions to prove:

- Whether balance is calculated from ledger entries on demand or maintained in a projection.
- Whether transfers lock account rows or use optimistic version checks.
- Whether Kafka or Redpanda is used for local development.
- Whether first implementation uses Spring Data JDBC only or jOOQ from day one.
- Whether GraalVM Native Image is worth using for this service or kept as an optional deployment path.

## Phase 4: Act

Build in vertical slices so every step produces working, tested behavior.

### Slice 1: Project Foundation

- Create Spring Boot 4.x project with Java 25.
- Add package/module structure.
- Add health endpoint and build pipeline.
- Add Docker Compose for PostgreSQL and Kafka or Redpanda.
- Add Flyway baseline migration.
- Add Testcontainers setup.

Done when:

- Application starts locally.
- Health endpoint works.
- Integration test connects to PostgreSQL.
- CI runs build and tests.

### Slice 2: Account And Balance Foundation

- Create account schema.
- Create balance projection schema.
- Implement account creation API.
- Implement account lookup API.
- Add validation, audit event, and integration tests.

Done when:

- Account can be created and queried.
- Invalid account states are rejected.
- Audit events are recorded.

### Slice 3: Ledger Posting

- Create transaction and ledger entry schema.
- Implement debit, credit, and transfer command.
- Enforce double-entry balance invariant.
- Add current and available balance updates.
- Add reversal model.

Done when:

- Transfer posts two balanced ledger entries.
- Reversal posts compensating entries.
- Ledger history remains immutable.

### Slice 4: Idempotency And Concurrency

- Add idempotency table.
- Require idempotency key for write commands.
- Return original response for duplicate request key.
- Add concurrent transfer tests.
- Add insufficient-funds handling.

Done when:

- Duplicate request cannot duplicate money movement.
- Concurrent requests preserve balance correctness.

### Slice 5: Outbox And Events

- Add outbox table.
- Create transaction-posted event.
- Publish outbox records to Kafka or Redpanda.
- Add retry and dead-letter handling.
- Add event contract tests.

Done when:

- Every posted transaction creates an outbox event.
- Publisher can recover after restart.
- Failed publications are visible operationally.

### Slice 6: Reconciliation And Operations

- Add reconciliation run API.
- Compare ledger entries, balance projections, transaction counts, and outbox state.
- Add exception severity model.
- Add Prometheus metrics and tracing.
- Add Grafana dashboard definitions.

Done when:

- Reconciliation identifies mismatches.
- Dashboards show latency, throughput, failures, and exception counts.

### Slice 7: Performance And Hardening

- Add k6 or Gatling load tests.
- Add JMH benchmarks for critical calculation paths.
- Tune database indexes and transaction boundaries.
- Add security configuration with OAuth2 resource server.
- Add container image, SBOM, and vulnerability scan.

Done when:

- p95 and p99 latency targets are documented and measured.
- Load test results are reproducible.
- Security and dependency checks run in CI.

## Initial Non-Functional Targets

These are starting targets. They should be adjusted after baseline testing.

| Metric | Target |
| --- | --- |
| Transfer API p95 latency | Under 100 ms under expected local load profile |
| Transfer API p99 latency | Under 250 ms under expected local load profile |
| Duplicate transaction rate | 0 accepted duplicates |
| Ledger imbalance tolerance | 0 |
| Outbox publish loss tolerance | 0 lost committed events |
| Reconciliation critical break tolerance | 0 |
| API availability target | 99.9 percent for single-region baseline |

## First Implementation Decision

Start with:

- Java 25 LTS.
- Spring Boot 4.x.
- Maven or Gradle after checking local project style.
- PostgreSQL.
- Flyway.
- jOOQ for ledger queries.
- Spring Web MVC REST.
- Testcontainers.
- Docker Compose.

Do not start with GraphQL, WebFlux, multi-region deployment, or too many services. Those can come later after the financial core is correct.

