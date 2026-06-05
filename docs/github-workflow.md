# GitHub Workflow For Staff-Level Migration Work

This document defines issue labels, issue types, and project-board flow for the modernization workspace.

## Labels

| Label | Purpose |
| --- | --- |
| `area/architecture` | Architecture design, decisions, and standards |
| `area/migration` | Migration, coexistence, cutover, and rollback |
| `area/data` | Data integrity, reconciliation, lineage, and ownership |
| `area/oracle` | Oracle performance, schema, statistics, and batch concerns |
| `area/security` | Security, privacy, access, and compliance controls |
| `area/operations` | Runbooks, dashboards, alerts, and support processes |
| `area/performance` | Latency, throughput, batch, and database performance |
| `risk/data-loss` | Work item can affect financial data correctness |
| `risk/downtime` | Work item can affect platform availability |
| `tier/critical` | Critical path for bank-grade migration readiness |
| `type/adr` | Architecture Decision Record |
| `type/spike` | Research or proof work |
| `type/epic` | Larger workstream |
| `type/runbook` | Operational procedure |

## Project Board Workflow

| State | Definition |
| --- | --- |
| Discovery | Problem is identified and initial scope is being clarified |
| Design | Architecture, options, and tradeoffs are being documented |
| Review | Staff-level review, risk review, or business review is in progress |
| Implementation-ready | Acceptance criteria and evidence are clear enough to execute |
| Validation | Tests, reconciliation, operational checks, or evidence review are in progress |
| Approved | Required sign-offs are complete |
| Done | Artifact or implementation is complete and linked |

## Evidence By Issue Type

| Issue Type | Required Evidence |
| --- | --- |
| Architecture Decision Record | Decision, alternatives, consequences, rollback path |
| Technical Spike | Options compared, recommendation, risks, follow-up issues |
| Epic | Child issues, acceptance criteria, delivery sequence |
| Runbook | Trigger, step-by-step procedure, validation, owner |
| Data Control | Mapping, reconciliation, exception handling, approval |
| Security Control | Data classification, access review, monitoring, audit evidence |
| Performance Remediation | Baseline, change, before/after result, rollback |

