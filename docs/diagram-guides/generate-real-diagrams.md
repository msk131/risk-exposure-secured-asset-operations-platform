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
mmdc -i schema-migration-toolkit/diagrams/schema-evolution-flow.md -o schema-evolution-flow.svg
```

## Recommended Portfolio Diagram Set

| Diagram | Why It Matters |
| --- | --- |
| `migration-roadmap.md` | Shows phased migration leadership |
| `data-reconciliation-flow.md` | Shows zero-loss migration and controls |
| `oracle-tuning-workflow.md` | Shows database optimization expertise |
| `schema-evolution-flow.md` | Shows schema migration governance |
| `legacy-architecture.md` | Shows the 2005-era source system baseline |
| `legacy-data-pipeline.md` | Shows old-tech file and batch processing |
| `logical-finance-erd.md` | Shows data modeling ability |
| `deployment-view.md` | Shows platform and DevSecOps awareness |
