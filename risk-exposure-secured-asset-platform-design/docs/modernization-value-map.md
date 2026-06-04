# Modernization Value Map

This map explains what the 2005-era legacy platform could not achieve well and what the modern stack enables.

## Capability Map

| Legacy Constraint | Modern Capability |
| --- | --- |
| JSP and Struts-style dashboard | React and TypeScript operations console |
| PL/SQL-heavy batch logic | Java Spring Boot services and testable business rules |
| File drops and manual load checks | CDC, Kafka/MSK, staged validation, replayable pipelines |
| Manual reconciliation | Automated reconciliation service and exception workflow |
| Server logs and status tables | OpenTelemetry, Prometheus, Grafana, Splunk/AppDynamics |
| Manual deployments | GitHub Actions, Helm, Argo CD, and Amazon EKS |
| Shared Oracle schema | Service-owned databases and read models |
| Broad legacy roles and grants | OIDC, RBAC/ABAC, least privilege, entitlement reconciliation |

## Cost Optimization

- Use autoscaling on EKS for demand-based capacity.
- Use managed services such as Aurora PostgreSQL, MSK, Redis, OpenSearch, and S3 to reduce undifferentiated operations.
- Apply S3 lifecycle policies for reports, archives, and replay files.
- Use performance testing and right-sizing to avoid overprovisioned infrastructure.
- Track cost by service, environment, and business capability.

## Security And Compliance

- Centralize authentication through OIDC-compatible identity provider.
- Apply RBAC and ABAC for operation, portfolio, region, and product scope.
- Use KMS for encryption keys and Secrets Manager for credentials.
- Capture immutable audit events for access, data changes, deployments, and approvals.
- Add SAST, dependency scanning, secret scanning, and container scanning into CI.

## Observability And Resiliency

- Use OpenTelemetry for traces and service correlation.
- Use Prometheus and Grafana for service metrics and SLO dashboards.
- Use Splunk or AppDynamics for enterprise monitoring integration where required.
- Track CDC lag, reconciliation status, Kafka consumer lag, API latency, and error rates.
- Use retry, timeout, circuit breaker, idempotency, and replay patterns.

## AI ML And Data Engineering Opportunities

- Use AI coding assistants for refactoring support, test generation, and documentation drafts.
- Use anomaly detection for batch duration, reconciliation breaks, CDC lag, and valuation variance.
- Use Apache Spark or Apache Beam for large-scale transformation and enrichment.
- Use object storage for replayable files, reports, lineage evidence, and archive data.

