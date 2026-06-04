# Target Cloud Architecture

```mermaid
flowchart TB
    USERS["Web Mobile Operations Users"]
    EDGE["API Gateway or Ingress"]
    EKS["Amazon EKS Microservices"]
    ARGO["Argo CD GitOps"]
    HELM["Helm Charts"]
    ECR["Amazon ECR"]
    MSK["Amazon MSK Event Streaming"]
    AURORA[("Aurora PostgreSQL Service Databases")]
    REDIS[("ElastiCache Redis")]
    SEARCH[("OpenSearch")]
    S3[("S3 Reports Archive Files")]
    AUDIT[("Audit Store")]
    OBS["CloudWatch OpenTelemetry Prometheus Grafana"]
    SECRETS["Secrets Manager and KMS"]

    USERS --> EDGE
    EDGE --> EKS
    ECR --> EKS
    HELM --> ARGO
    ARGO --> EKS
    EKS --> AURORA
    EKS --> REDIS
    EKS --> MSK
    MSK --> SEARCH
    MSK --> AUDIT
    EKS --> S3
    EKS --> OBS
    MSK --> OBS
    AURORA --> OBS
    SECRETS --> EKS
```

## Notes

- EKS is the recommended portfolio target for Kubernetes-based platform engineering.
- Aurora PostgreSQL is the default relational database for service-owned data.
- MSK supports event-driven integration, audit, reporting, and migration CDC flows.
