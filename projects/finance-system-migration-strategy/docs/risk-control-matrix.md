# Risk Control Matrix

This matrix captures the main risks in a regulated finance system migration and the controls expected before production cutover.

| Risk | Impact | Control | Evidence |
| --- | --- | --- | --- |
| Data loss | Customer, account, transaction, or balance records do not migrate correctly | Zero-loss mapping, reconciliation, backout plan | Mapping spec, reconciliation report, sign-off |
| Data corruption | Invalid transformed values enter the target system | Validation rules, staging checks, exception queue | Failed-record report, validation logs |
| Duplicate records | Customer, account, or transaction duplication after migration | Deduplication rules and survivorship logic | Cleansing report, duplicate count trend |
| Performance regression | Target system cannot handle production load | Oracle tuning, load testing, query baseline | Test report, AWR-style baseline, dashboard |
| Security exposure | Sensitive data is exposed during migration | Encryption, masking, least privilege, secrets management | Access review, encryption validation |
| Audit gap | Migration actions cannot be reconstructed | Immutable audit events and release approvals | Audit event logs, approval records |
| Cutover failure | Business flow fails after traffic shift | Phased cutover, rollback, smoke tests | Cutover runbook, rollback validation |
| Reporting mismatch | Legacy and target reports disagree | Parallel run and report reconciliation | Comparison reports, exception approvals |

## Control Gates

1. Discovery gate: inventory and dependency map complete.
2. Data readiness gate: cleansing and mapping rules approved.
3. Performance gate: critical SQL and batch baselines reviewed.
4. Parallel-run gate: reconciliation tolerance achieved.
5. Security gate: encryption, access, and audit controls verified.
6. Cutover gate: rollback, monitoring, and support readiness confirmed.

