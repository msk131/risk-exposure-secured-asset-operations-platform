# Target Technology Strategy

This strategy defines the recommended technology choices for modernizing the legacy asset management baseline into a resilient Risk Exposure & Secured Asset Operations Platform.

## Recommended Stack

| Layer | Recommended Choice | Reason |
| --- | --- | --- |
| Cloud | AWS | Strong fit for regulated workloads, managed databases, containers, networking, security, and observability |
| Backend | Java 21, Spring Boot 3 | Clear modernization path from legacy Java EE to modern Java services |
| APIs | REST, OpenAPI | Simple, contract-driven integration for channels and partners |
| Events | Apache Kafka or Amazon MSK | Event-driven ingestion, audit, projections, and decoupling |
| Integration | Spring Integration or Apache Camel | Useful for legacy adapters, file feeds, MQ-style flows, and transformation |
| Frontend | React, TypeScript, Vite | Modern operational dashboard and workflow UI |
| UI Component System | Material UI or Ant Design | Enterprise-ready tables, forms, filters, tabs, and dashboards |
| Cache | Redis or Amazon ElastiCache | Low-latency reads, session-independent caching, and rate-control support |
| Search | OpenSearch | Operational search, exception search, and audit/report lookup |
| Observability | OpenTelemetry, Prometheus, Grafana, CloudWatch | Metrics, logs, traces, dashboards, and alerting |

## Backend Technology Direction

- Use Java 21 and Spring Boot 3 for all new services.
- Use REST APIs for synchronous business capabilities.
- Use events for lifecycle changes, reporting projections, audit, and integration decoupling.
- Use idempotency keys for critical write APIs.
- Use OpenAPI contracts and versioned API evolution.
- Use Testcontainers for integration tests involving databases, Kafka, and external adapters.

## Frontend Technology Direction

- Build an operational React and TypeScript UI.
- Focus on portfolio, exposure, secured asset, allocation, reconciliation, exception, and audit workflows.
- Use dense tables, filters, tabs, status indicators, and drill-down detail views.
- Use Playwright for E2E coverage of key operational flows.

## Technology Principles

- Prefer managed cloud services where they reduce operational burden.
- Keep business data owned by the service that owns the business capability.
- Avoid direct database access from channels or other services.
- Use events and read models for reporting and cross-domain views.
- Treat audit, security, observability, and testing as architecture features, not add-ons.

