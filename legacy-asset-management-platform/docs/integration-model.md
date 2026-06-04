# Integration Model

The legacy platform integrates through a mix of file feeds, SOAP/XML-style services, MQ-style messaging, and reporting extracts.

## Upstream Integrations

| Source | Integration Pattern | Purpose |
| --- | --- | --- |
| Trading system | File feed or MQ-style message | Trade activity |
| Market data provider | SFTP-style price file | Daily prices |
| Reference data system | File feed | Asset and client reference data |
| Cash system | File feed | Portfolio cash balances |

## Downstream Integrations

| Consumer | Integration Pattern | Purpose |
| --- | --- | --- |
| Reporting platform | Extract or reporting table | Operational and client reports |
| Risk platform | File or MQ-style message | Exposure and valuation data |
| Operations dashboard | Database/status table view | Batch and exception status |
| Audit archive | File extract or audit table | Historical evidence |

## Migration Relevance

These integrations create modernization opportunities around API-first access, event-driven publishing, versioned contracts, automated reconciliation, and clearer data ownership.

