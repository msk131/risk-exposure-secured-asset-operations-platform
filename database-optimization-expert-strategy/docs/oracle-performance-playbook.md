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

## SQL Plan Baseline And Optimizer Statistics Governance

| Control | Standard |
| --- | --- |
| Critical SQL list | Maintain top 20 Structured Query Language (SQL) statements by elapsed time, central processing unit (CPU), buffer gets, physical reads, and business criticality |
| Plan capture | Store SQL identifier, plan hash, bind profile, row counts, and before/after metrics |
| Baseline trigger | Use SQL Plan Management when a critical query has repeated plan regression or cardinality instability |
| Statistics refresh | Use non-blocking scheduled statistics refresh with production-safe sampling and runtime limits |
| Histogram policy | Use histograms only where data skew is proven and monitored |
| Deployment gate | Block release when a critical plan hash changes without approval |

## Online Index And Partition Strategy

| Change | Production-Safe Direction |
| --- | --- |
| New index | Create online where supported; validate Data Manipulation Language (DML) overhead before release |
| Index rebuild | Rebuild online during approved low-risk window and monitor locks, undo, and input/output |
| Partition conversion | Use online redefinition where supported and rehearse with production-like volume |
| Partition key | Prefer business date for transaction, audit, ledger, and history tables |
| Archive policy | Move aged partitions to approved cold storage only after retention and audit review |

## Regression Detection

Every database change must compare:

- Plan hash value.
- Elapsed time.
- Buffer gets per execution.
- Physical reads.
- Rows processed.
- Lock wait time.
- Batch duration.
