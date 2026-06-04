# Auth And RBAC Migration

Authentication and authorization migration is part of schema and platform modernization. Legacy finance systems often keep user roles, entitlement flags, service accounts, and database grants across tables, application code, and operational procedures.

## Objectives

- Preserve correct access during phased migration.
- Map legacy users, groups, roles, and permissions to target RBAC controls.
- Remove excessive privileges and undocumented access.
- Support least privilege, segregation of duties, and auditability.
- Avoid access gaps while legacy and target systems coexist.

## Legacy Access Model

| Legacy Area | Typical Pattern | Migration Risk |
| --- | --- | --- |
| User table | Application-owned login and profile records | Duplicate identity source |
| Role table | Broad roles such as admin, operator, viewer | Over-permissioned access |
| Group mapping | Team or region-based groups | Unclear entitlement ownership |
| Entitlement flags | Fine-grained flags stored in tables | Hidden business rules |
| Service accounts | Shared credentials for jobs and integrations | Weak accountability |
| Database grants | Direct table or procedure access | Bypasses application controls |

## Target Access Model

| Target Area | Direction |
| --- | --- |
| Identity provider | Centralized authentication through enterprise identity provider |
| RBAC | Role-based access by capability, domain, and operational responsibility |
| ABAC where needed | Attribute-based checks for region, product, legal entity, or portfolio scope |
| Service identity | Dedicated service accounts with scoped permissions |
| Audit | Immutable access and entitlement change history |
| Privileged access | Approval workflow and time-bound elevation |

## Migration Approach

1. Inventory user, role, group, entitlement, and database grant tables.
2. Create a legacy-to-target role mapping matrix.
3. Classify permissions by capability and risk level.
4. Keep legacy and target authorization aligned during coexistence.
5. Freeze legacy entitlement changes before cutover.
6. Retire legacy role tables and direct grants after sign-off.

## RBAC Mapping Matrix

| Legacy Role | Target Role | Scope |
| --- | --- | --- |
| System Admin | Platform Admin | Environment or platform |
| Operations User | Operations Analyst | Region, product, or portfolio |
| Batch Operator | Batch Operations | Batch domain |
| Read Only User | Business Viewer | Assigned scope |
| Service Account | Service Identity | Service-specific |

## Controls

- Least-privilege review before cutover.
- Segregation-of-duties validation.
- Privileged access approval and expiry.
- Entitlement reconciliation during coexistence.
- Audit trail for access changes.
- Removal of direct database grants where application controls should apply.

