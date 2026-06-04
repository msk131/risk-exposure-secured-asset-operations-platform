# Oracle Performance Playbook

## Diagnostic Workflow

1. Confirm business impact.
2. Capture baseline metrics.
3. Identify top SQL and wait events.
4. Review execution plans and table/index statistics.
5. Identify the smallest safe change.
6. Validate with production-like data volume.
7. Deploy with monitoring and rollback.
8. Compare post-release metrics to baseline.

## Evidence To Capture

| Evidence | Why It Matters |
| --- | --- |
| SQL ID | Links analysis to the exact statement |
| Execution plan | Shows join method, access path, and cost model |
| Elapsed time | Measures user or batch impact |
| CPU time | Identifies compute pressure |
| Buffer gets | Shows logical IO intensity |
| Physical reads | Shows disk IO pressure |
| Rows processed | Helps validate cardinality and selectivity |
| Wait events | Shows where sessions spend time |
| Bind variables | Explains plan variation and selectivity |
| Table/index stats | Determines optimizer quality |

## Common Patterns

| Symptom | Likely Cause | Possible Fix |
| --- | --- | --- |
| High buffer gets | Poor access path | Index review, predicate rewrite |
| Full table scan | Missing index or low selectivity | Index, partitioning, query rewrite |
| Plan instability | Bind peeking, skew, stale stats | Stats strategy, SQL plan management |
| Slow batch | Row-by-row processing | Set-based SQL, batching, commit tuning |
| Lock waits | Long transactions or hot rows | Transaction redesign, concurrency control |
| Temp pressure | Large sorts or hash joins | Query rewrite, indexing, memory review |
| High parse time | Literal SQL or poor cursor reuse | Bind variables, cursor sharing review |

## Tuning Principles

- Tune from evidence, not guesswork.
- Fix the biggest bottleneck first.
- Prefer small, reversible changes.
- Avoid adding indexes without workload impact review.
- Validate functional equivalence after SQL rewrites.
- Compare before-and-after metrics.
- Monitor production after release.

