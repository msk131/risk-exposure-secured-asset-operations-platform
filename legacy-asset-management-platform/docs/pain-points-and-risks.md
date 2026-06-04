# Pain Points And Risks

## Technical Pain Points

| Pain Point | Risk |
| --- | --- |
| Monolithic application | Slow change, high regression risk |
| Shared Oracle schema | Hidden coupling and unclear data ownership |
| PL/SQL-heavy logic | Hard-to-test business rules |
| shell-script batch orchestration | Manual support and inconsistent controls |
| SQL*Loader-style ingestion | Limited validation and observability |
| SOAP/XML-style integrations | Brittle contracts and slower change |
| Manual reconciliation | Operational risk and delayed issue detection |
| Server-rendered dashboard | Limited workflow and alerting |

## Business Risks

- delayed end-of-day completion
- reconciliation breaks discovered late
- manual rerun and sign-off dependency
- limited audit evidence for operational decisions
- difficult onboarding for new engineers
- high production support cost

## Migration Drivers

- improve delivery speed
- reduce operational risk
- modernize integration patterns
- improve observability
- automate reconciliation
- separate data ownership by capability
- reduce database performance bottlenecks

