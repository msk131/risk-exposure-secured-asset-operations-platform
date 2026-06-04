# Deployment View

```mermaid
flowchart TB
    subgraph Runtime
        GW[API Gateway]
        SVC[Domain Services]
        WORKERS[Workers and Projectors]
    end

    subgraph Platform
        K8S[Kubernetes]
        SECRETS[Secrets Manager]
        CONFIG[Config Management]
        CI[CI/CD Pipeline]
    end

    subgraph Data
        DB[(Service Databases)]
        BUS[(Event Bus)]
        AUDIT[(Audit Store)]
        READ[(Reporting Read Model)]
    end

    subgraph Observability
        METRICS[Metrics]
        LOGS[Logs]
        TRACES[Traces]
        ALERTS[Alerts]
    end

    CI --> K8S
    K8S --> GW
    K8S --> SVC
    K8S --> WORKERS
    SECRETS --> SVC
    CONFIG --> SVC
    SVC --> DB
    SVC --> BUS
    WORKERS --> READ
    BUS --> AUDIT
    GW --> METRICS
    SVC --> LOGS
    SVC --> TRACES
    METRICS --> ALERTS
```

## Deployment Notes

- Services should be independently deployable.
- Database migrations should run through controlled release pipelines.
- Secrets should not be stored in application repositories.
- Observability should be part of the release definition of done.

