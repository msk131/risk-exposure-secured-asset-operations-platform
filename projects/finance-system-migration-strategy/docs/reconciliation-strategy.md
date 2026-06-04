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

