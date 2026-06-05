# ADR-003: Use Orchestrated Saga For Cross-Service Financial Workflows

## Status

Accepted

## Context

Asset allocation, substitution, instruction completion, and exposure recalculation can cross multiple services. A partial failure can leave the platform showing allocated collateral without updated exposure, creating uncovered risk.

## Decision

Use an orchestrated saga for cross-service financial workflows. Temporal is the preferred long-running workflow engine; Amazon Web Services Step Functions is acceptable for simpler platform-managed workflows.

## Decision Drivers

- Explicit state machine for financial operations.
- Deterministic retries, timeout ceilings, and compensation.
- Better operator visibility into stuck or partially completed workflows.
- Audit trail for every transition.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Choreographed events only | Loose coupling | Hard to reason about failure and compensation |
| Service-local orchestration | Simple for one service | Cross-service ownership becomes unclear |
| Orchestrated saga | Clear control and auditability | Requires workflow runtime and operational discipline |

## Consequences

- Saga state is a regulated operational record.
- Every step must define retry, timeout, and compensation behavior.
- A reconciliation sweeper must detect stuck workflows.
- Teams must avoid hidden cross-service writes outside the saga.

## Rollback Or Reversal

Force stuck sagas into a terminal compensated state through an approved operations procedure, reconcile business records, and emit audit events for manual intervention.

## Evidence Required

- Saga state diagram
- Compensation matrix
- Timeout policy
- Stuck-workflow dashboard
- Reconciliation sweeper report

