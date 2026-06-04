# Legacy Architecture

```mermaid
flowchart TB
    USERS[Operations Users]
    DASH[JSP / Struts-Style Dashboard]
    MONO[Java EE 5 Style Monolith]
    ORACLE[(Shared Oracle Schema)]
    PLSQL[PL/SQL-Heavy Business Logic]
    BATCH[Shell / Scheduler Batch Jobs]
    FILES[SFTP-Style File Feeds]
    SOAP[SOAP/XML-Style Services]
    MQ[MQ-Style Messaging]
    REPORTS[Manual Reports / Extracts]

    USERS --> DASH
    DASH --> MONO
    MONO --> ORACLE
    ORACLE --> PLSQL
    FILES --> BATCH
    BATCH --> ORACLE
    MONO --> SOAP
    MQ --> MONO
    ORACLE --> REPORTS
    PLSQL --> REPORTS
```

## Notes

- The monolith, database, batch jobs, and reporting are tightly coupled.
- The shared schema acts as the integration point for several processes.
- Operations visibility is dashboard and report driven rather than observability driven.

