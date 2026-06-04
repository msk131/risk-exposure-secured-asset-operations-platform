# Generate Real Diagrams

This repository stores diagrams as Mermaid Markdown. GitHub can render these diagrams automatically, and the same source can be exported as PNG, SVG, or PDF for presentations and architecture documents.

## Where Diagrams Live

| Project | Diagram Folder |
| --- | --- |
| General Finance System Migration Strategy | `projects/finance-system-migration-strategy/diagrams` |
| Risk Exposure & Secured Asset Platform Design | `projects/risk-exposure-secured-asset-platform-design/diagrams` |

## Option 1: Use GitHub Preview

This is the easiest option.

1. Push the repository to GitHub.
2. Open any `.md` file that contains a Mermaid diagram.
3. GitHub renders the diagram automatically.
4. Use the rendered view for review and discussion.

Best for:

- Portfolio review
- GitHub README presentation
- Quick architecture explanation

## Option 2: Use VS Code Preview

Install a Mermaid preview extension in VS Code.

Suggested extensions:

- Markdown Preview Mermaid Support
- Mermaid Markdown Syntax Highlighting

Steps:

1. Open the repository in VS Code.
2. Open a diagram Markdown file.
3. Use Markdown preview.
4. Adjust the Mermaid text until the diagram is clear.

Best for:

- Local editing
- Fast diagram iteration
- Checking layout before GitHub commit

## Option 3: Export With Mermaid CLI

Mermaid CLI can export diagrams as image files.

Install:

```bash
npm install -g @mermaid-js/mermaid-cli
```

Export one diagram:

```bash
mmdc -i projects/finance-system-migration-strategy/diagrams/migration-roadmap.md -o finance-migration-roadmap.svg
```

Export as PNG:

```bash
mmdc -i projects/risk-exposure-secured-asset-platform-design/diagrams/deployment-view.md -o risk-exposure-deployment-view.png
```

Best for:

- LinkedIn posts
- Resume attachments
- PowerPoint decks
- Architecture review documents

## Option 4: Use Mermaid Live Editor

Use the Mermaid Live Editor when you want a browser-based editing experience.

Steps:

1. Open a diagram Markdown file.
2. Copy only the Mermaid code inside the fenced block.
3. Paste it into Mermaid Live Editor.
4. Export as SVG or PNG.

Best for:

- Quick export
- Visual tweaking
- Sharing a diagram outside GitHub

## Option 5: Recreate In Draw.io

For more formal architecture documents, use the Mermaid diagrams as drafts and recreate them in draw.io.

Recommended workflow:

1. Start with the Mermaid diagram to agree the architecture.
2. Recreate the final version in draw.io.
3. Export PNG/SVG for documents.
4. Keep the Mermaid file as the source architecture note in GitHub.

Best for:

- Formal architecture packs
- Interview presentation visuals
- Executive-friendly diagrams

## Recommended Portfolio Diagram Set

For a strong portfolio presentation, export these first:

| Diagram | Why It Matters |
| --- | --- |
| `migration-roadmap.md` | Shows phased migration leadership |
| `data-reconciliation-flow.md` | Shows zero-loss migration and controls |
| `cutover-runbook-flow.md` | Shows production risk management |
| `core-domain-erd.md` | Shows data modeling ability |
| `service-interaction-flow.md` | Shows microservice interaction design |
| `deployment-view.md` | Shows platform and DevSecOps awareness |

## How To Improve Diagram Quality

- Keep diagrams focused on one idea.
- Use short node names.
- Avoid too many crossing lines.
- Split large diagrams into separate views.
- Use architecture layers: channel, API, service, data, integration, observability.
- Add short notes below each diagram explaining the design decision.

## Suggested Export Naming

Use clear filenames when exporting:

```text
finance-migration-roadmap.svg
finance-data-reconciliation-flow.svg
finance-cutover-runbook-flow.svg
risk-exposure-core-domain-erd.svg
risk-exposure-service-interaction-flow.svg
risk-exposure-deployment-view.svg
```

