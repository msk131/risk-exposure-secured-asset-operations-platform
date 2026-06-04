# CI/CD And DevSecOps Strategy

GitHub Actions is the recommended CI/CD system for the portfolio because it is visible in GitHub and supports environments, approvals, secrets, and deployment protection.

## Pipeline Stages

| Stage | Purpose |
| --- | --- |
| Pull request checks | Format, lint, static analysis, unit tests |
| Build | Compile services and frontend artifacts |
| Test | Integration, contract, migration, and E2E suites |
| Security | SAST, dependency scan, secret scan, container image scan |
| Package | Build container images and publish to ECR |
| Deploy dev | Automatic deployment to development |
| Deploy staging | Controlled deployment with E2E and performance tests |
| Production approval | Manual approval and change evidence |
| Progressive deploy | Canary or blue/green rollout |
| Post-deploy validation | Smoke tests, metrics, logs, traces, and rollback readiness |

## DevSecOps Controls

- Protected branches and pull request reviews.
- Required status checks before merge.
- Environment-specific secrets.
- Deployment approvals for staging and production.
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

