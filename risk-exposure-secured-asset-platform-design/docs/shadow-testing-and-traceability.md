# Shadow Testing, Traceability, And Privacy Controls

Shadow testing compares the target platform against the legacy platform using production-like traffic without allowing duplicate side effects.

## Gateway Contract

| Header | Purpose |
| --- | --- |
| `X-Shadow-Request: true` | Marks duplicated traffic as shadow-only |
| `X-Migration-Phase` | Identifies discovery, parallel-run, canary, or cutover phase |
| `traceparent` | World Wide Web Consortium (W3C) distributed trace context |
| `X-Correlation-Id` | Business correlation identifier shared across logs, events, and audit |
| `X-Causation-Id` | Identifier of the upstream command or event that caused the action |

## Side-Effect Blocking

| Dependency | Shadow Behavior |
| --- | --- |
| Settlement or payment network | Block and record intended request in shadow evidence store |
| Customer notification | Suppress delivery and write notification preview only |
| Credit bureau or third-party validation | Use sandbox, cached response, or deterministic mock |
| Downstream reporting submission | Write to isolated shadow projection |
| Live operational database | Read-only unless explicitly approved for shadow-owned store |

## Trace Separation

- Shadow traces must keep the same root correlation identifier as the live request.
- Shadow spans must include `environment=shadow`.
- Shadow latency and error metrics must not count against live production Service Level Objectives (SLOs).
- Reconciliation dashboards must support side-by-side trace comparison.

## Personally Identifiable Information Controls

| Data Class | Control |
| --- | --- |
| Account number | Tokenize before shadow persistence |
| Party legal name | Mask or tokenize depending on test need |
| Tax identifier | Irreversible hash in shadow environment |
| Email or phone | Mask before logs, traces, and Dead Letter Queues (DLQs) |
| Free-text operations notes | Scan and redact before persistence |

## Evidence Required

- Gateway mirroring rule.
- External adapter side-effect test.
- Log and trace privacy scan.
- Shadow/live reconciliation report.
- Proof that no shadow request mutated live state.

