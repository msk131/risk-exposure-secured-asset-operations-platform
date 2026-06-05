# ADR-004: Use Header-Driven Shadow Traffic For Parallel Validation

## Status

Accepted

## Context

The target platform must be validated using production-like requests without sending duplicate settlement instructions, external calls, customer notifications, or live operational writes.

## Decision

Duplicate selected live requests at the gateway and tag them with `X-Shadow-Request: true`, a migration phase, and trace context. Shadow traffic must be isolated from live side effects.

## Decision Drivers

- Compare legacy and target behavior under realistic inputs.
- Protect external systems from duplicated calls.
- Keep shadow metrics separate from live production Service Level Objectives (SLOs).
- Preserve traceability for variance investigation.

## Options Considered

| Option | Benefit | Risk |
| --- | --- | --- |
| Synthetic test data only | Safe and simple | Misses production edge cases |
| Full mirrored traffic without isolation | High fidelity | Dangerous side effects and privacy exposure |
| Header-driven shadow traffic | High fidelity with controls | Requires gateway, dependency, and logging discipline |

## Consequences

- Services must recognize and preserve shadow metadata.
- External adapters must block, mock, or tokenize side-effecting calls.
- Shadow storage must be isolated from live operational tables.
- Observability must separate live and shadow signals.

## Rollback Or Reversal

Disable gateway mirroring, drain shadow topics, preserve comparison evidence, and confirm no shadow data entered live operational stores.

## Evidence Required

- Gateway routing rule
- Shadow dependency isolation proof
- PII masking validation
- Trace correlation dashboard
- Live-state mutation check

