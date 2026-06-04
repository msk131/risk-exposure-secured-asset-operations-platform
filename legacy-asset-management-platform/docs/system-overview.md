# System Overview

This legacy platform supports asset management operations for client portfolios.

## Core Capabilities

| Capability | Description |
| --- | --- |
| Client management | Maintains client reference information and service status |
| Portfolio management | Tracks portfolios, mandates, investment strategy, and ownership |
| Holdings management | Maintains current positions by asset, portfolio, quantity, and value |
| Trade capture | Receives trade activity from upstream execution or order systems |
| Price ingestion | Loads market prices and reference prices from daily file feeds |
| Valuation | Calculates portfolio and holding-level valuation |
| Cash balances | Tracks cash position by portfolio and currency |
| Reconciliation | Compares records, totals, valuations, and exceptions across sources |
| Batch operations | Runs end-of-day ingestion, validation, valuation, and reporting |

## Operating Model

Operations teams monitor batch runs through a legacy dashboard. Exception handling is partly manual, with business sign-off required before downstream reporting is considered complete.

## Legacy Characteristics

- Business rules are split across Java server code, PL/SQL packages, database triggers, and operational runbooks.
- File-based feeds are a major source of daily processing.
- The database is treated as a shared system of record by several application components.
- Reconciliation is report-driven and operator-led.
- Observability is limited to batch logs, database logs, status tables, and manual checks.

