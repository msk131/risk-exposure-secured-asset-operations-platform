# Migration Roadmap

```mermaid
flowchart LR
    DISC[Discovery\nInventory, dependencies, data profile]
    READY[Readiness\nCleansing, mapping, performance baseline]
    BUILD[Coexistence Build\nRouting, staging, sync, observability]
    PARALLEL[Parallel Run\nLegacy and target validation]
    CUTOVER[Phased Cutover\nTraffic shift and monitoring]
    DECOM[Decommission\nRetire legacy assets]

    DISC --> READY
    READY --> BUILD
    BUILD --> PARALLEL
    PARALLEL --> CUTOVER
    CUTOVER --> DECOM

    READY -. risk controls .-> PARALLEL
    PARALLEL -. reconciliation gate .-> CUTOVER
```

## Notes

- Use discovery to reduce unknowns before design commitments.
- Use readiness to improve data quality and Oracle performance before migration pressure increases.
- Use coexistence to avoid big-bang replacement.
- Use parallel run and reconciliation as the main cutover gate.

