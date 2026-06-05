# Generate Real Diagrams

This repository stores diagrams as Mermaid Markdown. GitHub can render these diagrams automatically, and the same source can be exported as PNG, SVG, or PDF for presentations and architecture documents.

## Where Diagrams Live

| Project | Diagram Folder |
| --- | --- |
| General Finance System Migration Strategy | `finance-system-migration-strategy/diagrams` |
| Database Optimization Expert Strategy | `database-optimization-expert-strategy/diagrams` |
| Schema Migration Toolkit | `schema-migration-toolkit/diagrams` |
| Legacy Asset Management Platform | `legacy-asset-management-platform/diagrams` |
| Risk Exposure & Secured Asset Platform Design | `risk-exposure-secured-asset-platform-design/diagrams` |

## Export Options

- Preview directly in GitHub.
- Preview locally in VS Code with a Mermaid extension.
- Export with Mermaid CLI using `mmdc`.
- Recreate polished versions in draw.io for formal presentations.

## Mermaid CLI Example

```bash
npm install -g @mermaid-js/mermaid-cli
mmdc -i risk-exposure-secured-asset-platform-design/diagrams/target-cloud-architecture.md -o target-cloud-architecture.svg
```

## Recommended Architecture Diagram Set

| Diagram | Why It Matters |
| --- | --- |
| `migration-roadmap.md` | Shows phased migration leadership |
| `data-reconciliation-flow.md` | Shows zero-loss migration and controls |
| `oracle-tuning-workflow.md` | Shows database optimization expertise |
| `schema-evolution-flow.md` | Shows schema migration governance |
| `auth-rbac-migration-flow.md` | Shows access-control migration strategy |
| `coexistence-data-sync-flow.md` | Shows phased migration data sync |
| `legacy-architecture.md` | Shows the 2005-era source system baseline |
| `legacy-data-pipeline.md` | Shows old-tech file and batch processing |
| `target-cloud-architecture.md` | Shows AWS target architecture |
| `ci-cd-pipeline.md` | Shows DevSecOps delivery flow |
| `gitops-deployment-flow.md` | Shows GitHub Actions, Helm, Argo CD, and EKS delivery |
| `zero-downtime-release-flow.md` | Shows progressive release strategy |
| `testing-pyramid.md` | Shows test strategy |
| `legacy-to-modern-capability-map.md` | Shows what the modern stack enables over the legacy baseline |
