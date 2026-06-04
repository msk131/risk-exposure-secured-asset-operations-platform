# Cloud Architecture Strategy

AWS is the recommended cloud target for this portfolio architecture.

## Recommended AWS Services

| Concern | AWS Service Direction |
| --- | --- |
| Container platform | Amazon EKS for Kubernetes-based platform engineering |
| Simpler container alternative | Amazon ECS Fargate where operational overhead should be lower |
| Relational databases | Amazon Aurora PostgreSQL |
| CDC and migration | AWS DMS or Debezium-style CDC into Kafka/MSK |
| Event streaming | Amazon MSK |
| Cache | Amazon ElastiCache for Redis |
| Search | Amazon OpenSearch Service |
| Object storage | Amazon S3 for files, reports, archives, and exports |
| Secrets | AWS Secrets Manager |
| Encryption keys | AWS KMS |
| API edge | API Gateway or ingress controller on EKS |
| Observability | CloudWatch, OpenTelemetry, Prometheus, Grafana |
| Container registry | Amazon ECR |

## Target Cloud Principles

- Use multi-account separation for dev, test, staging, and production.
- Keep workloads in private subnets where possible.
- Use least-privilege IAM roles for services and pipelines.
- Encrypt data at rest and in transit.
- Use managed services for databases, cache, streaming, and search where practical.
- Apply the AWS Well-Architected pillars: operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability.

## EKS Versus ECS

| Option | Use When |
| --- | --- |
| EKS | You want Kubernetes, Helm, platform engineering, service mesh, and portable microservice operations |
| ECS Fargate | You want simpler container hosting with less Kubernetes operational overhead |

Recommended portfolio choice: **EKS** for the target architecture, with ECS Fargate listed as a simpler alternative.

