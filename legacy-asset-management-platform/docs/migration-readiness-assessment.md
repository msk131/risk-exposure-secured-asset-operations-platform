# Migration Readiness Assessment

This assessment prepares the legacy asset management platform for future migration planning.

## Readiness Dimensions

| Dimension | Current State | Migration Need |
| --- | --- | --- |
| Application architecture | Monolithic Java EE style | Domain decomposition |
| UI | JSP/Struts-style dashboard | Modern operations UI or dashboards |
| Database | Shared Oracle schema | Service-owned schemas or controlled read models |
| Business logic | Split across app and PL/SQL | Refactor into testable services |
| Pipeline | File and batch oriented | Event-driven or managed data pipeline |
| Integration | SOAP/XML, MQ-style, files | REST APIs, events, versioned contracts |
| Reconciliation | Manual report-driven process | Automated reconciliation workflow |
| Observability | Logs/status tables | Metrics, traces, alerts, dashboards |
| Release process | Manual deployment and scripts | CI/CD and DevSecOps controls |

## Migration Candidate Areas

1. Portfolio query and reporting read model.
2. Price ingestion pipeline.
3. Valuation calculation service.
4. Reconciliation workflow.
5. Batch operations dashboard.
6. Integration contract modernization.
7. Shared schema decomposition.

## Recommended Migration Sequence

1. Baseline current data, batch, and performance metrics.
2. Improve observability around batch and reconciliation.
3. Extract reporting read model.
4. Modernize ingestion and validation pipeline.
5. Refactor valuation logic into service capability.
6. Introduce API and event contracts.
7. Decommission legacy paths after parallel-run validation.

