# Production Rollout Controls

Database optimization changes can improve performance, but they can also create regressions if released without controls.

## Change Types

| Change | Risk | Control |
| --- | --- | --- |
| SQL rewrite | Functional mismatch | Regression tests and result comparison |
| New index | DML slowdown or plan change | Workload impact review |
| Drop index | Query regression | Usage analysis and rollback script |
| Partitioning | Data movement or operational complexity | Rehearsal and runtime estimate |
| Statistics change | Plan instability | Baseline comparison and rollback strategy |
| Batch tuning | Partial processing or locking | Restartability and reconciliation |

## Release Checklist

- Baseline captured.
- SQL and execution plan reviewed.
- Functional test passed.
- Performance test passed with representative volume.
- Rollback script or compensation path exists.
- Monitoring dashboard is ready.
- Production support team is briefed.
- Post-release validation window is defined.

## Post-Release Validation

Measure:

- Top SQL elapsed time
- Buffer gets
- Physical reads
- CPU time
- Wait events
- Batch duration
- Error rate
- Lock wait trends
- Application response time

