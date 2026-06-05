# Staff Engineer Deep-Dive Issue Backlog

This backlog reviews the current project workspace as a bank-grade modernization blueprint. The repository already explains the migration journey well, but the next staff-engineer step is to convert strategy into enforceable engineering contracts: measurable thresholds, failure-mode handling, rollback sequencing, audit evidence, and ownership decisions.

For full forms and short definitions of abbreviations used in this backlog, see [Glossary](glossary.md).

## Current Workspace Gaps

| Area | Current State | Staff-Level Gap |
| --- | --- | --- |
| Main README | Strong portfolio narrative and project map | Needs a scannable target stack, Non-Functional Requirement (NFR) block, and direct links to critical diagrams and runbooks |
| Migration architecture | Shows coexistence, Change Data Capture (CDC), staging, reconciliation, and audit | Missing explicit Change Data Capture (CDC) ordering, replay, lag, and cutover guardrail thresholds |
| Reconciliation | Defines scope and broad tolerances | Missing deterministic hash strategy, rounding policy, severity model, and exception Service Level Agreement (SLA) |
| Core Entity Relationship Diagram (ERD) | Captures main business entities | Missing bitemporal fields, immutable ledger/audit separation, ownership metadata, and outbox structures |
| Microservices | Defines service boundaries and data ownership | Missing event schemas, transactional outbox contract, idempotency store, and saga state model |
| Zero downtime | Mentions blue/green, canary, shadow, and expand-contract | Missing exact rollback runbook and irreversible-data-change handling |
| Oracle optimization | Covers top Structured Query Language (SQL), plans, indexes, stats, waits, and rollout | Missing Structured Query Language (SQL) plan baseline policy, online index/redefinition approach, and partition lifecycle |
| Schema migration | Covers Flyway/Liquibase and expand-contract | Missing dual-sync loop prevention, Foreign Key (FK) validation sequencing, and migration script quality gates |
| Observability | Lists metrics, logs, traces, dashboards, alerts | Missing concrete Service Level Objectives (SLOs), alert thresholds, trace propagation contract, and regulator-ready evidence |
| GitHub workflow | Has one generic issue template | Needs typed issue templates, labels, Architecture Decision Records (ADRs), and project board phases |

## Priority Issue List

### ~~Issue 1: [DOCS] Promote README From Portfolio Narrative To System Architecture Entry Point~~

Status: Done. The root README now includes a system architecture snapshot, target technology stack, critical architecture views, and direct links to reconciliation, schema governance, cutover, and Oracle performance artifacts.

Type: Documentation Architecture

Problem: The root README explains the journey well, but a bank reviewer must scan system tier, compliance bounds, Recovery Time Objective (RTO), Recovery Point Objective (RPO), target stack, and critical controls immediately.

Acceptance Criteria:

- ~~Add a "System Constraints" block covering availability, Recovery Time Objective (RTO), Recovery Point Objective (RPO), data-loss tolerance, auditability, privacy, and regulatory context.~~
- ~~Add a "Target Tech Stack" table covering Amazon Web Services (AWS), Elastic Kubernetes Service (EKS), Managed Streaming for Apache Kafka (MSK/Kafka), Aurora PostgreSQL, Redis, OpenSearch, Simple Storage Service (S3), Argo CD, Helm, GitHub Actions, Flyway/Liquibase, OpenTelemetry, and Identity and Access Management (IAM)/secrets.~~
- ~~Embed or surface the core Entity Relationship Diagram (ERD) and target architecture diagram directly from the README.~~
- ~~Link to the rollback runbook, reconciliation strategy, schema governance, and Oracle performance playbook.~~

### ~~Issue 2: [ADR] Introduce Architecture Decision Records For Binding Migration Decisions~~

Status: Done. Added [Architecture Decision Record template](adr/adr-template.md) and accepted Architecture Decision Records (ADRs) for Change Data Capture (CDC), transactional outbox, orchestrated saga, shadow traffic, service-owned databases, and expand-contract migration.

Type: Architecture Governance

Problem: The repo has many "direction" statements, but enterprise teams need durable decisions with context, tradeoffs, consequences, and status.

Acceptance Criteria:

- Create `docs/adr/` with a lightweight Architecture Decision Record (ADR) template.
- Add Architecture Decision Records (ADRs) for Debezium/Change Data Capture (CDC), transactional outbox, orchestrated saga, header-driven shadow traffic, service-owned databases, and expand-contract schema migration.
- Each ADR must include decision drivers, rejected alternatives, operational consequences, and rollback implications.

### ~~Issue 3: [DATA] Upgrade Core Entity Relationship Diagram For Bitemporal And Immutable Ledger Semantics~~

Status: Done. Upgraded [Core Domain Entity Relationship Diagram](../risk-exposure-secured-asset-platform-design/diagrams/core-domain-erd.md) with bitemporal fields, ledger entries, calculation evidence, allocation history, ownership notes, and outbox structure.

Type: Data Architecture

Problem: The current ERD models operational entities, but bank-grade risk and asset systems need historical truth, business effective time, transaction time, versioning, and immutable audit/ledger records.

Acceptance Criteria:

- Add `valid_from`, `valid_to`, `transaction_time`, `source_system`, `record_version`, and `correlation_id` where appropriate.
- Separate mutable operational tables from append-only ledger/audit event tables.
- Model valuation snapshots, exposure calculations, asset eligibility rules, and allocation lifecycle history explicitly.
- Add notes explaining which service owns each entity and which entities are read models.

### ~~Issue 4: [DATA] Define Transactional Outbox Table And Event Envelope Contract~~

Status: Done. Added [Transactional Outbox And Event Contract](../risk-exposure-secured-asset-platform-design/docs/transactional-outbox-and-event-contract.md).

Type: Distributed Systems / Data Consistency

Problem: The docs mention event-driven integration and idempotency, but there is no concrete outbox schema or event envelope. Without this, teams may publish directly to Kafka and create dual-write data loss.

Acceptance Criteria:

- Define `transaction_outbox` schema with event ID, aggregate type, aggregate ID, event type, payload, schema version, trace ID, created timestamp, processing status, and retry metadata.
- Define a standard event envelope for all domain events.
- Require outbox write in the same local transaction as business state.
- Specify CDC publishing behavior, delivery guarantee, duplicate handling, and poison-event routing.

### ~~Issue 5: [DATA] Build Idempotency And Deduplication Standard For All Critical Writes~~

Status: Done. Added [Idempotency And Deduplication Standard](../risk-exposure-secured-asset-platform-design/docs/idempotency-and-deduplication-standard.md).

Type: Transactional Safety

Problem: The platform states "idempotency keys for critical write operations," but does not define how keys are generated, stored, expired, or audited.

Acceptance Criteria:

- Define an idempotency key format by channel, operation, business reference, and version.
- Add a service-level idempotency table or Redis-backed check-and-set design.
- Specify conflict behavior when the same key arrives with a different payload.
- Require metrics for accepted, replayed, rejected, and expired idempotency keys.

### ~~Issue 6: [SAGA] Define Distributed Transaction State Machine And Compensation Rules~~

Status: Done. Added [Saga State Machine And Compensation Rules](../risk-exposure-secured-asset-platform-design/docs/saga-state-machine.md).

Type: Resiliency Architecture

Problem: Exposure, secured asset, instruction, and agreement workflows will cross service boundaries. The repo needs an explicit saga model to avoid partial state and uncovered risk.

Acceptance Criteria:

- Identify workflows requiring orchestration, such as asset allocation, substitution, instruction completion, and exposure recalculation.
- Define saga states, timeout ceilings, retries, terminal states, and compensating actions.
- Choose orchestration direction: Temporal, AWS Step Functions, or service-level orchestration.
- Add a reconciliation sweeper for stuck or orphaned sagas.

### ~~Issue 7: [MIGRATION] Define Change Data Capture Ordering, Replay, And Lag Guardrails~~

Status: Done. Added ordering, replay, lag thresholds, and failure recovery to [Coexistence Data Sync](../schema-migration-toolkit/docs/coexistence-data-sync.md).

Type: Migration Data Sync

Problem: Coexistence docs mention Change Data Capture (CDC) and replay, but do not define ordering guarantees, lag thresholds, or what happens when replay collides with live writes.

Acceptance Criteria:

- Define Change Data Capture (CDC) source, keying strategy, ordering key, schema versioning, and replay procedure.
- Add cutover thresholds for Change Data Capture (CDC) lag by entity and flow.
- Define ownership rules when legacy and target update the same logical record.
- Add recovery steps for missed events, duplicate events, out-of-order events, and schema drift.

### ~~Issue 8: [MIGRATION] Prevent Bidirectional Sync Loops During Expand-Contract Bridge Phase~~

Status: Done. Added origin markers, Change Data Capture (CDC) filters, and bridge test matrix to [Coexistence Data Sync](../schema-migration-toolkit/docs/coexistence-data-sync.md).

Type: Data Integrity

Problem: Bridge-phase sync between old and new schemas can create echo writes or infinite update loops.

Acceptance Criteria:

- Add a session/application context marker for writes created by migration sync.
- Define trigger or Change Data Capture (CDC) filters that ignore migration-originated changes.
- Capture before/after examples for legacy-to-target and target-to-legacy writes.
- Add a test matrix for rollback during mixed-version deployments.

### ~~Issue 9: [MIGRATION] Define Foreign Key And Constraint Rollout Strategy For Live Schema Splits~~

Status: Done. Added constraint rollout stages, write ordering, and validation evidence to [Schema Release Governance](../schema-migration-toolkit/docs/schema-release-governance.md).

Type: Database Concurrency

Problem: Splitting monolithic Oracle tables into service-owned schemas can introduce deadlocks, blocking validation, and mixed-version write failures.

Acceptance Criteria:

- Define when constraints are nullable, deferrable, not validated, or validated online.
- Add deterministic parent-child write ordering rules.
- Add migration checks for orphan rows before enabling constraints.
- Define rollback/compensation behavior for failed constraint validation.

### ~~Issue 10: [RECON] Build Deterministic Reconciliation Hash And Exception Severity Model~~

Status: Done. Added deterministic hash strategy and exception severity model to [Reconciliation Strategy](../finance-system-migration-strategy/docs/reconciliation-strategy.md).

Type: Data Verification

Problem: The reconciliation docs define broad checks, but staff-level migration needs repeatable row-group hashing, severity, ownership, and exception SLAs.

Acceptance Criteria:

- Define canonicalization rules for dates, time zones, currency scale, nulls, status mappings, and text normalization.
- Add entity-level and aggregate-level hash comparison.
- Define severity levels for financial, regulatory, operational, and cosmetic breaks.
- Add SLA and approval rules for each exception class.

### ~~Issue 11: [RECON] Formalize Floating-Point And Monetary Rounding Drift Thresholds~~

Status: Done. Added monetary rounding and drift policy with test vectors to [Reconciliation Strategy](../finance-system-migration-strategy/docs/reconciliation-strategy.md).

Type: Technical Spike

Problem: Legacy Oracle `NUMBER` calculations and target Java/PostgreSQL numeric logic can differ in precision, scale, and rounding.

Acceptance Criteria:

- Inventory all monetary and risk calculation fields.
- Define approved precision, scale, rounding mode, and tolerance by calculation.
- Require exact match for ledger balances and regulated reporting totals unless formally excepted.
- Add test vectors for edge cases such as currency conversion, zero quantity, negative exposure, and high-value portfolios.

### ~~Issue 12: [CUTOVER] Create Day-2 Rollback Runbook For Phased Migration~~

Status: Done. Added [Day-2 Phased Migration Rollback Runbook](../finance-system-migration-strategy/docs/day-2-rollback-runbook.md).

Type: Operations / Disaster Recovery

Problem: The repo has a cutover flow, but not an executable rollback runbook for failures after traffic has already moved.

Acceptance Criteria:

- Define rollback triggers, decision authority, communication steps, and freeze windows.
- Sequence stop-target-writes, drain outbox, pause CDC, reconcile deltas, verify Oracle sequence/key alignment, and route traffic back.
- Define how to handle transactions accepted by the target but not yet reflected in legacy.
- Add validation queries and dashboard checks required before declaring rollback complete.

### ~~Issue 13: [SHADOW] Enforce Side-Effect-Free Shadow Testing~~

Status: Done. Added side-effect blocking and gateway contract to [Shadow Testing, Traceability, And Privacy Controls](../risk-exposure-secured-asset-platform-design/docs/shadow-testing-and-traceability.md).

Type: Production Validation

Problem: Shadow traffic must not send duplicate wires, external bureau calls, customer alerts, settlement instructions, emails, or report submissions.

Acceptance Criteria:

- Add `X-Shadow-Request` or equivalent routing metadata at the gateway.
- Block or virtualize all external side-effecting calls from shadow services.
- Store shadow outputs in isolated topics/tables.
- Require audit evidence that shadow traffic cannot mutate live operational state.

### ~~Issue 14: [SHADOW] Add Distributed Trace Context Across Legacy And Target Runs~~

Status: Done. Added trace headers and shadow/live metric separation to [Shadow Testing, Traceability, And Privacy Controls](../risk-exposure-secured-asset-platform-design/docs/shadow-testing-and-traceability.md).

Type: Observability

Problem: When legacy and target outputs differ, teams need a single correlation trail across both execution paths.

Acceptance Criteria:

- Standardize W3C `traceparent`, correlation ID, causation ID, migration phase, and shadow/live tags.
- Ensure API gateway, services, CDC, reconciliation jobs, and audit store preserve IDs.
- Add dashboard views comparing live and shadow latency, errors, and reconciliation breaks by trace.
- Keep shadow metrics separated from production SLO metrics.

### ~~Issue 15: [SECURITY] Tokenize Personally Identifiable Information Before Mirroring Live Traffic To Shadow Environments~~

Status: Done. Added Personally Identifiable Information (PII) tokenization and masking controls to [Shadow Testing, Traceability, And Privacy Controls](../risk-exposure-secured-asset-platform-design/docs/shadow-testing-and-traceability.md).

Type: Security / Privacy

Problem: Mirroring real production payloads can leak account numbers, party names, tax identifiers, Personally Identifiable Information (PII), and other sensitive values into less restricted systems or logs.

Acceptance Criteria:

- Classify sensitive fields in API payloads, events, logs, and database snapshots.
- Add masking/tokenization at the gateway or ingestion boundary before shadow persistence.
- Define reversible versus irreversible tokenization rules.
- Add validation proving logs, traces, DLQs, and dashboards do not expose raw PII.

### ~~Issue 16: [KAFKA] Define Poison Pill Handling And Non-Blocking Dead Letter Queue Policy~~

Status: Done. Added retry, deserialization, and Dead Letter Queue (DLQ) policy to [Kafka Resiliency Policy](../risk-exposure-secured-asset-platform-design/docs/kafka-resiliency-policy.md).

Type: Streaming Resiliency

Problem: Kafka consumers can stall a whole partition if deserialization or validation repeatedly fails.

Acceptance Criteria:

- Catch deserialization failures before business processing.
- Route malformed payloads to a Dead Letter Queue (DLQ) with raw bytes, schema identifier, topic, partition, offset, and trace context.
- Commit offsets only after safe Dead Letter Queue (DLQ) handoff.
- Add retry topics with bounded backoff for transient failures and immediate Dead Letter Queue (DLQ) routing for unrecoverable schema failures.

### ~~Issue 17: [KAFKA] Define Message Keying And Version Checks For Asset Event Ordering~~

Status: Done. Added Kafka keying and aggregate-version checks to [Kafka Resiliency Policy](../risk-exposure-secured-asset-platform-design/docs/kafka-resiliency-policy.md).

Type: Event Consistency

Problem: Kafka ordering is partition-scoped. Asset lifecycle events can corrupt target state if keyed inconsistently or processed out of order.

Acceptance Criteria:

- Key all asset and exposure events by stable aggregate ID.
- Add monotonic aggregate version to each event.
- Reject, buffer, or quarantine events that arrive with missing prior versions.
- Add metrics for out-of-order, duplicate, stale, and future-version events.

### ~~Issue 18: [ORACLE] Define SQL Plan Baseline And Optimizer Statistics Governance~~

Status: Done. Added Structured Query Language (SQL) plan baseline and statistics governance to [Oracle Performance Playbook](../database-optimization-expert-strategy/docs/oracle-performance-playbook.md).

Type: Oracle Performance Engineering

Problem: The Oracle playbook discusses plan instability, but does not set policy for SQL plan baselines, SQL profiles, stats refresh, or production plan regression detection.

Acceptance Criteria:

- Capture baselines for top critical SQL by elapsed time, CPU, buffer gets, and business criticality.
- Define when SQL Plan Management or SQL Profiles are allowed.
- Add non-blocking stats refresh policy for large and volatile tables.
- Add pre/post deployment checks for plan hash changes and cardinality drift.

### ~~Issue 19: [ORACLE] Plan Online Indexing And Partitioning For High-Growth Ledger Tables~~

Status: Done. Added online index and partition strategy to [Oracle Performance Playbook](../database-optimization-expert-strategy/docs/oracle-performance-playbook.md).

Type: Database Operations

Problem: Index and partition changes can lock or destabilize active banking tables if done without online strategy and rehearsal.

Acceptance Criteria:

- Identify candidate high-growth transaction, audit, history, and reporting tables.
- Define online index creation/rebuild approach and rollback plan.
- Define partitioning strategy by business date, region, product, or lifecycle.
- Include archive/purge policy aligned to retention and regulatory evidence needs.

### ~~Issue 20: [BATCH] Modernize Legacy Batch Restartability And Control Tables~~

Status: Done. Added restartability, control table, idempotent ingestion, and operator actions to [Batch Operations Model](../legacy-asset-management-platform/docs/batch-operations-model.md).

Type: Batch Operations

Problem: The legacy baseline includes shell scripts, SQL*Loader, Control-M/cron-style scheduling, and manual sign-off. The migration plan should define restartability before moving critical batch flows.

Acceptance Criteria:

- Add batch control tables for run ID, source file, checksum, row counts, rejects, restart point, and sign-off.
- Define idempotent file ingestion and duplicate-file detection.
- Add reconciliation between batch input, staging, target writes, and reports.
- Define operator dashboard requirements for rerun, skip, hold, and approve actions.

### ~~Issue 21: [SECURITY] Define Zero-Trust Network And Service Identity Model~~

Status: Done. Added zero-trust network and service identity controls to [Security, Lineage, Service Objectives, And Chaos Verification](../risk-exposure-secured-asset-platform-design/docs/security-lineage-slo-chaos.md).

Type: Platform Security

Problem: The target AWS architecture mentions IAM and security controls, but not concrete boundaries for private traffic, service identity, secrets, and mTLS.

Acceptance Criteria:

- Define Virtual Private Cloud (VPC), subnet, security group, endpoint, and PrivateLink boundaries.
- Require service-to-service identity, Mutual Transport Layer Security (mTLS), and least-privilege Identity and Access Management (IAM) roles.
- Define secrets rotation and break-glass access process.
- Add evidence requirements for access reviews and privileged migration jobs.

### ~~Issue 22: [COMPLIANCE] Generate Automated Data Lineage And Audit Evidence~~

Status: Done. Added lineage metadata and release evidence bundle to [Security, Lineage, Service Objectives, And Chaos Verification](../risk-exposure-secured-asset-platform-design/docs/security-lineage-slo-chaos.md).

Type: Governance / BCBS 239 Readiness

Problem: The repo mentions auditability, but a bank-grade design must prove how risk data moved, transformed, and reached reports.

Acceptance Criteria:

- Define lineage metadata for datasets, services, events, jobs, schemas, and reports.
- Emit lineage from CI/CD and runtime jobs into a central registry.
- Link reconciliation reports to source snapshots, mapping versions, code versions, and approvals.
- Add a release evidence bundle checklist for regulated production changes.

### ~~Issue 23: [SLO] Define Production Service Level Objectives, Error Budgets, And Alert Thresholds~~

Status: Done. Added Service Level Objectives (SLOs), thresholds, and paging triggers to [Security, Lineage, Service Objectives, And Chaos Verification](../risk-exposure-secured-asset-platform-design/docs/security-lineage-slo-chaos.md).

Type: Operational Excellence

Problem: Observability docs list dashboard categories, but not specific Service Level Objectives (SLOs) or alert policy.

Acceptance Criteria:

- Define Service Level Objectives (SLOs) for Application Programming Interface (API) latency, availability, reconciliation freshness, Change Data Capture (CDC) lag, outbox publishing delay, and batch completion.
- Add severity thresholds and paging policy.
- Define error-budget burn alerts for critical workflows.
- Separate migration readiness alerts from steady-state product alerts.

### ~~Issue 24: [CHAOS] Add Controlled Failure Testing For Migration Runtime~~

Status: Done. Added controlled failure experiments and evidence requirements to [Security, Lineage, Service Objectives, And Chaos Verification](../risk-exposure-secured-asset-platform-design/docs/security-lineage-slo-chaos.md).

Type: Resilience Verification

Problem: The testing strategy mentions resilience tests, but does not define concrete failure experiments required before cutover.

Acceptance Criteria:

- Add experiments for pod loss, database failover, Kafka broker interruption, CDC connector pause, network latency, and object-store unavailability.
- Define expected RTO/RPO and data integrity checks per experiment.
- Require reconciliation after each failure injection.
- Capture test evidence as a cutover gate.

### ~~Issue 25: [GITHUB] Improve Repository Issue Templates, Labels, And Project Board~~

Status: Done. Added typed issue templates and [GitHub Workflow For Staff-Level Migration Work](github-workflow.md).

Type: Engineering Workflow

Problem: The repo has one generic work-item issue template. Staff-level execution needs typed issues and consistent labels for architecture, risk, operations, security, and data integrity.

Acceptance Criteria:

- Add issue templates for ADR, spike, epic, runbook, data control, security control, and performance remediation.
- Add labels such as `area/migration`, `area/data`, `area/oracle`, `area/security`, `risk/data-loss`, `risk/downtime`, `tier/critical`, `type/adr`, `type/runbook`, and `type/spike`.
- Define a project board workflow: discovery, design, review, implementation-ready, validation, approved, done.
- Link each issue type to required evidence and sign-off.

## Recommended Sequencing

| Sequence | Issues | Why |
| --- | --- | --- |
| 1 | Issues 1, 2, 25 | Make the workspace easier to scan, govern, and execute |
| 2 | Issues 3, 4, 5, 6 | Lock down data consistency and distributed transaction contracts |
| 3 | Issues 7, 8, 9, 10, 11, 12 | Make coexistence, reconciliation, and rollback credible |
| 4 | Issues 13, 14, 15, 16, 17 | Harden shadow testing and streaming resiliency |
| 5 | Issues 18, 19, 20 | Deepen the legacy Oracle and batch modernization story |
| 6 | Issues 21, 22, 23, 24 | Add regulator-grade security, evidence, Service Level Objectives (SLOs), and resilience verification |
