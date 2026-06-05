# Batch Operations Model

The batch operating model is centered around end-of-day processing.

## End-Of-Day Sequence

| Step | Description |
| --- | --- |
| File arrival | Confirm required upstream files have arrived |
| Load staging | Load files into staging tables |
| Validate | Run data quality and reference data checks |
| Transform | Move valid records into core tables |
| Calculate | Run portfolio valuation and cash balance updates |
| Reconcile | Compare counts, totals, valuations, and exceptions |
| Report | Generate operational and downstream reports |
| Sign off | Operations confirms completion or escalates breaks |

## Operational Dependencies

- upstream file delivery
- database availability
- scheduler availability
- reference data completeness
- manual exception review
- downstream report cutoffs

## Failure Handling

Failures are typically handled through manual review, rerun procedures, exception reports, and operations sign-off. This creates migration opportunities for automated validation, retry, alerting, and observability.

## Restartability And Control Tables

Legacy batch modernization requires explicit control records before high-risk migration. Every file load, transformation, valuation, reconciliation, and report job should record its state.

```sql
CREATE TABLE batch_run_control (
    run_id VARCHAR2(80) PRIMARY KEY,
    job_name VARCHAR2(120) NOT NULL,
    source_file_name VARCHAR2(255),
    source_file_checksum VARCHAR2(128),
    business_date DATE NOT NULL,
    status VARCHAR2(40) NOT NULL,
    input_row_count NUMBER,
    loaded_row_count NUMBER,
    rejected_row_count NUMBER,
    restart_step VARCHAR2(120),
    started_at TIMESTAMP,
    completed_at TIMESTAMP,
    approved_by VARCHAR2(120),
    approval_reference VARCHAR2(120)
);
```

## Idempotent File Ingestion

| Control | Purpose |
| --- | --- |
| File checksum | Detect duplicate or changed input files |
| Run identifier | Connect every load, reject, reconciliation, and report output |
| Restart step | Resume from the last safe checkpoint |
| Reject count | Prevent silent data loss |
| Approval reference | Preserve operator sign-off evidence |

## Operator Actions

| Action | Required Evidence |
| --- | --- |
| Rerun | Prior failed run, fixed root cause, new run identifier |
| Skip | Business approval and impact analysis |
| Hold | Reason, owner, and next review time |
| Approve | Reconciliation report and exception summary |
