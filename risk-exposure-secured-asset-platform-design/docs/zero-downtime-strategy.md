# Zero Downtime Strategy

Zero downtime requires coordinated application, database, routing, and data synchronization practices.

## Deployment Patterns

| Pattern | Use |
| --- | --- |
| Rolling deployment | Low-risk service updates |
| Blue/green deployment | Safer production rollout with fast rollback |
| Canary deployment | Gradual traffic increase with metric-based validation |
| Feature flags | Decouple deployment from user-visible release |
| Shadow traffic | Compare new behavior before production cutover |

## Database Change Pattern

Use expand-contract:

1. Expand schema with backward-compatible additions.
2. Deploy code that supports old and new schema.
3. Backfill or synchronize data.
4. Shift reads and writes gradually.
5. Contract obsolete schema only after consumers move.

## Runtime Requirements

- Backward-compatible APIs.
- Idempotent writes.
- Consumer replay support.
- Retry, timeout, and circuit-breaker patterns.
- Health checks and readiness probes.
- Fast rollback for application changes.
- Roll-forward or compensation plan for irreversible data changes.

## Cutover Readiness

- Metrics and alerts are active.
- Reconciliation checks pass.
- CDC lag is within threshold.
- RBAC/access checks pass.
- Rollback route is tested.
- Support team and business owner are available.
- Production approval evidence is captured.

