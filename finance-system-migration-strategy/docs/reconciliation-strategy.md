# Reconciliation Strategy

Reconciliation proves that legacy and target systems produce equivalent business outcomes before final cutover.

## Reconciliation Scope

| Data Area | Checks |
| --- | --- |
| Customer and party data | Key matching, name/status mapping, duplicate detection |
| Account data | Account status, ownership, product relationship, lifecycle dates |
| Balances | Ledger balance, available balance, currency, snapshot timestamp |
| Transactions | Count, amount, currency, posting status, transaction date |
| Agreements and products | Eligibility, status, effective dates, rule mapping |
| Reports | Totals, grouping, filters, calculated fields, exceptions |
| Audit events | Actor, action, timestamp, entity reference, source system |

## Reconciliation Flow

1. Extract legacy data from approved source snapshots.
2. Transform through documented zero-loss mapping rules.
3. Load into migration staging and target stores.
4. Compare source and target outputs by entity and business rule.
5. Send breaks to exception handling.
6. Approve, remediate, or defer exceptions with documented rationale.
7. Repeat until cutover thresholds are met.

## Tolerance Examples

| Check | Tolerance |
| --- | --- |
| Record counts for critical entities | 100 percent match |
| Monetary totals | Exact match by currency unless rounding rule is approved |
| Status values | 100 percent mapped or explicitly excepted |
| Report totals | Exact match for regulated reports |
| Non-critical descriptive fields | Approved variance list |

## Exception Handling

Each exception should capture:

- Source record identifier
- Target record identifier if created
- Entity type
- Failed rule
- Severity
- Root cause
- Owner
- Decision
- Approval reference

## Deterministic Hash Strategy

Reconciliation jobs must compare both individual records and grouped financial totals. Before hashing, fields are canonicalized so harmless formatting differences do not create false breaks.

| Field Type | Canonicalization Rule |
| --- | --- |
| Timestamp | Convert to Coordinated Universal Time (UTC) and ISO-8601 format |
| Currency amount | Normalize to approved scale per currency |
| Null value | Represent as explicit token `<NULL>` |
| Text | Trim, normalize whitespace, and apply agreed case rules |
| Status | Map through approved legacy-to-target status table |
| Identifier | Use stable business key, not environment-specific surrogate key |

Example hash input:

```text
entity=EXPOSURE|id=EXP-123|currency=USD|amount=1000000.00|status=ACTIVE|version=7
```

## Exception Severity Model

| Severity | Definition | Service Level Agreement (SLA) |
| --- | --- | --- |
| Critical | Data loss, duplicate financial movement, ledger mismatch, or regulated report mismatch | Block cutover; same-day executive and business sign-off required |
| High | Exposure, secured asset, account, agreement, or transaction state mismatch | Block affected flow; remediation plan within 1 business day |
| Medium | Non-financial lifecycle or operational reporting mismatch | Remediate or approve exception before traffic shift |
| Low | Cosmetic or descriptive mismatch with no financial impact | Approve through variance list |

## Monetary Rounding And Drift Policy

| Data Area | Rule |
| --- | --- |
| Ledger balances | Exact match by account, currency, and snapshot timestamp |
| Regulated reports | Exact match unless a documented rounding rule is approved |
| Exposure calculations | Match approved precision, scale, and rounding mode per product |
| Currency conversion | Compare source rate, target rate, converted amount, and rounding mode |
| Non-critical analytics | Tolerance must be documented and approved by business owner |

### Required Test Vectors

- Zero quantity allocation.
- Negative exposure adjustment.
- High-value portfolio greater than 10 million in notional value.
- Multi-currency conversion with more than two decimal places.
- Boundary rounding at half-even and half-up thresholds.
