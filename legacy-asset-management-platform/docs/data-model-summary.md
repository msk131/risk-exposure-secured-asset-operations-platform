# Data Model Summary

This document describes the conceptual entities in the legacy asset management platform.

## Core Entities

| Entity | Purpose |
| --- | --- |
| Client | Investor, institution, or customer relationship |
| Portfolio | Managed account or investment portfolio |
| Holding | Asset quantity and current position |
| Trade | Buy, sell, transfer, or adjustment activity |
| Price | Daily asset price or valuation input |
| Valuation | Portfolio or holding value by date |
| Cash Balance | Cash position by portfolio and currency |
| Batch Run | Operational record of a batch execution |
| Exception | Validation, reconciliation, or processing break |

## Legacy Data Traits

- Shared schema used by application, batch, and reporting components.
- Mixed operational and reporting data in the same database.
- Business rules split across application code and database routines.
- Audit captured through status tables and database-trigger-style patterns.

