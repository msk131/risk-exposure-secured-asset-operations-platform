# ADR-005: Use Service-Owned Databases

## Status

Accepted

## Context

The legacy platform uses a shared Oracle schema. Hidden database coupling slows change, makes ownership unclear, and increases migration risk.

## Decision

Each target microservice owns its operational database schema. Other services may read data only through Application Programming Interfaces (APIs), events, or approved read models.

## Decision Drivers

- Clear ownership for data quality and schema evolution.
- Reduced hidden coupling between services.
- Safer independent deployments.
- Stronger audit and access-control boundaries.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Shared target database | Familiar and easy reporting | Recreates legacy coupling |
| Shared reporting database only | Efficient query path | Can become unauthorized operational dependency |
| Service-owned databases | Clear boundaries | Requires eventing, read models, and ownership discipline |

## Consequences

- Cross-service joins are not allowed in operational paths.
- Reporting must use projections or governed read models.
- Schema changes require owner approval.
- Data contracts must be versioned and tested.

## Rollback Or Reversal

If a service boundary is wrong, publish a migration ADR, build a temporary compatibility projection, backfill the new owner, and retire the old owner after reconciliation.

## Evidence Required

- Data ownership matrix
- API and event contracts
- Schema migration plan
- Access review
- Read model governance

