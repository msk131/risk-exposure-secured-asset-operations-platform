# CI/CD And DevSecOps Strategy

GitHub Actions is the recommended CI system for the portfolio because it is visible in GitHub and supports environments, approvals, secrets, and deployment protection. Argo CD is the recommended continuous delivery tool for Kubernetes because it supports GitOps-based deployment, drift detection, auditable desired state, and safer environment promotion.

## Recommended Delivery Model

```text
Developer PR
-> GitHub Actions CI
-> Build and scan container image
-> Push image to Amazon ECR
-> Update Helm values or GitOps manifest
-> Argo CD detects desired-state change
-> Argo CD deploys to Amazon EKS
-> Canary or blue/green rollout
-> Observability validation
```

## Tool Responsibilities

| Tool | Responsibility |
| --- | --- |
| GitHub Actions | Build, test, scan, package, and prepare deployment change |
| Amazon ECR | Store signed/scanned container images |
| Helm | Package Kubernetes application manifests and values |
| Argo CD | Sync GitOps manifests to EKS and detect drift |
| Amazon EKS | Managed Kubernetes runtime |
| Prometheus, Grafana, OpenTelemetry, Splunk/AppDynamics | Validate release health and runtime behavior |

## Pipeline Stages

| Stage | Purpose |
| --- | --- |
| Pull request checks | Format, lint, static analysis, unit tests |
| Build | Compile services and frontend artifacts |
| Test | Integration, contract, migration, and E2E suites |
| Security | SAST, dependency scan, secret scan, container image scan |
| Package | Build container images and publish to ECR |
| GitOps update | Update Helm values or Kubernetes manifests in the deployment repo/path |
| Deploy dev | Argo CD syncs development environment |
| Deploy staging | Controlled deployment with E2E and performance tests |
| Production approval | Manual approval and change evidence |
| Progressive deploy | Canary or blue/green rollout |
| Post-deploy validation | Smoke tests, metrics, logs, traces, and rollback readiness |

## DevSecOps Controls

- Protected branches and pull request reviews.
- Required status checks before merge.
- Environment-specific secrets.
- Deployment approvals for staging and production.
- GitOps manifests reviewed and versioned.
- Argo CD drift detection enabled for deployed environments.
- Container image scanning.
- Dependency vulnerability scanning.
- Infrastructure-as-code validation.
- Database migration dry runs.
- Change records for high-risk releases.

## Quality Gates

- Unit tests pass.
- Integration tests pass.
- Contract tests pass.
- Security scans have no critical unresolved issues.
- Migration dry run passes.
- Performance baseline is not violated.
- Observability dashboards are ready before production rollout.
