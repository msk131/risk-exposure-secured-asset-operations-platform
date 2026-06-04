# Legacy Technology Stack

This document describes the conceptual 2005-era technology baseline.

## Application Layer

| Area | Legacy Pattern |
| --- | --- |
| UI | JSP-style server-rendered pages |
| Web framework | Struts-style controller/action flow |
| Runtime | Java EE 5 style application server |
| Packaging | WAR/EAR-style deployment |
| Session model | Server-side session-heavy interaction |
| Configuration | Environment-specific server and deployment configuration |

## Database Layer

| Area | Legacy Pattern |
| --- | --- |
| Database | Oracle-style shared schema |
| Business logic | PL/SQL-style packages and triggers |
| Loading | SQL*Loader-style staging loads |
| Reporting | Views, denormalized reporting tables, and manual extracts |
| Audit | Database audit tables and trigger-based change capture |

## Batch And Integration Layer

| Area | Legacy Pattern |
| --- | --- |
| File transfer | SFTP-style inbound and outbound drops |
| Batch orchestration | shell-script and Control-M/cron-style scheduling |
| Integration | SOAP/XML-style service calls |
| Messaging | MQ-style message exchange |
| Monitoring | batch logs, status tables, email alerts, and operations dashboard |

## Modernization Relevance

This stack creates useful migration discussion points around service decomposition, data ownership, pipeline modernization, testing gaps, observability, and release automation.

