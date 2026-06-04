# Legacy Asset Management Platform

This project is a documentation-only baseline for a believable 2005-era asset management platform.

It intentionally contains no runnable code, SQL, shell scripts, XML files, CSV files, WSDL files, or implementation artifacts. The purpose is to describe the legacy system through architecture notes, operating model documentation, tables, and Mermaid diagrams so it can be used as the source baseline for future migration planning.

## Legacy Scenario

The platform supports asset management operations across clients, portfolios, holdings, trades, prices, valuations, cash balances, exceptions, reconciliation, and end-of-day operations.

The system is intentionally modeled as an older enterprise finance platform:

- Java EE 5 style monolith
- JSP/Servlet/Struts-style user interface
- Oracle shared schema
- PL/SQL-heavy validation, transformation, and valuation processing
- shell-script orchestrated batch pipeline
- SQL*Loader-style file ingestion
- SOAP/XML-style integrations
- MQ-style message exchange
- SFTP-style upstream file drops
- Control-M/cron-style scheduling
- manual reconciliation and operator sign-off

## Documentation

| Document | Purpose |
| --- | --- |
| [system-overview.md](docs/system-overview.md) | Business capability and operational overview |
| [legacy-technology-stack.md](docs/legacy-technology-stack.md) | 2005-era technology baseline |
| [data-pipeline-overview.md](docs/data-pipeline-overview.md) | Legacy file and batch pipeline |
| [batch-operations-model.md](docs/batch-operations-model.md) | End-of-day operations and dependencies |
| [dashboard-and-operations.md](docs/dashboard-and-operations.md) | Legacy dashboard concept and operator workflow |
| [data-model-summary.md](docs/data-model-summary.md) | Conceptual data model summary |
| [integration-model.md](docs/integration-model.md) | SOAP, MQ, file-feed, and reporting integrations |
| [pain-points-and-risks.md](docs/pain-points-and-risks.md) | Migration drivers and risk areas |
| [migration-readiness-assessment.md](docs/migration-readiness-assessment.md) | Readiness assessment for future migration planning |

## Diagrams

| Diagram | Purpose |
| --- | --- |
| [legacy-architecture.md](diagrams/legacy-architecture.md) | Legacy application, data, batch, integration, and reporting view |
| [legacy-data-pipeline.md](diagrams/legacy-data-pipeline.md) | SFTP/file ingestion through reconciliation and reporting |
| [batch-operations-dashboard.md](diagrams/batch-operations-dashboard.md) | Operations dashboard information flow |
| [legacy-data-model.md](diagrams/legacy-data-model.md) | Conceptual ERD for asset management data |
| [integration-landscape.md](diagrams/integration-landscape.md) | Upstream and downstream integration landscape |
| [migration-pain-points-map.md](diagrams/migration-pain-points-map.md) | Legacy constraints mapped to modernization opportunities |

## How This Baseline Will Be Used

This project acts as the source system for a later migration plan. It provides enough legacy context to support:

- monolith-to-microservice migration planning
- Oracle optimization strategy
- schema migration and data ownership planning
- batch pipeline modernization
- API and event-driven target architecture design
- observability, DevSecOps, and operational resilience planning

