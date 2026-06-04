# Data Pipeline Overview

The legacy data pipeline is file-driven and batch-oriented.

## Conceptual Flow

1. Upstream systems place files into an inbound SFTP-style location.
2. Operations or scheduler triggers shell-script style orchestration.
3. Files are loaded into Oracle staging tables through SQL*Loader-style processing.
4. PL/SQL-style validation checks record quality, reference data, totals, and status values.
5. Invalid records are routed to exception tables.
6. Valid records are transformed into core tables.
7. Valuation and reconciliation procedures execute.
8. Reports are generated for operations and downstream reporting teams.
9. Files are archived or marked for rejection.

## Data Feeds

| Feed | Purpose |
| --- | --- |
| Position file | Updates holdings and quantities |
| Price file | Updates daily price and valuation inputs |
| Trade file | Provides trade activity and settlement context |
| Cash balance file | Provides daily cash balance by portfolio and currency |
| Reference data file | Updates assets, products, and client reference data |

## Control Points

- file arrival check
- file naming and business date validation
- record count check
- staging load status
- validation status
- exception count
- reconciliation pass/fail
- operator sign-off

