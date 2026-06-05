# Legacy Asset Management Platform

This project defines the legacy baseline used for migration planning.

It represents a 2005-era financial operations platform built around a monolithic application, shared Oracle schema, batch processing, file feeds, SOAP/XML integrations, message queues, and manual reconciliation.

## What This Project Solves

Before designing a migration, the existing system must be understood clearly. This project documents the legacy architecture, operating model, data flow, integration style, and migration risks so the modernization plan has a realistic source-state baseline.

## Key Artifacts

| Artifact | Purpose |
| --- | --- |
| [system-overview.md](docs/system-overview.md) | Business capabilities and operating context |
| [legacy-technology-stack.md](docs/legacy-technology-stack.md) | Legacy application, database, batch, and integration stack |
| [data-pipeline-overview.md](docs/data-pipeline-overview.md) | File ingestion and batch data flow |
| [batch-operations-model.md](docs/batch-operations-model.md) | End-of-day processing and restartability controls |
| [data-model-summary.md](docs/data-model-summary.md) | Conceptual legacy data model |
| [integration-model.md](docs/integration-model.md) | Upstream and downstream integration patterns |
| [pain-points-and-risks.md](docs/pain-points-and-risks.md) | Migration drivers and risk areas |
| [migration-readiness-assessment.md](docs/migration-readiness-assessment.md) | Readiness view for modernization planning |

## Diagrams

- [legacy-architecture.md](diagrams/legacy-architecture.md)
- [legacy-data-pipeline.md](diagrams/legacy-data-pipeline.md)
- [legacy-data-model.md](diagrams/legacy-data-model.md)
- [integration-landscape.md](diagrams/integration-landscape.md)
- [migration-pain-points-map.md](diagrams/migration-pain-points-map.md)

