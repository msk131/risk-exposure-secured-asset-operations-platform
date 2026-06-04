# Service Interaction Flow

```mermaid
sequenceDiagram
    participant Channel
    participant Gateway
    participant Agreement
    participant Exposure
    participant Asset
    participant Instruction
    participant EventBus
    participant Audit

    Channel->>Gateway: Request secured asset operation
    Gateway->>Agreement: Validate agreement and rules
    Agreement-->>Gateway: Agreement context
    Gateway->>Exposure: Retrieve exposure requirement
    Exposure-->>Gateway: Exposure details
    Gateway->>Asset: Check eligible asset inventory
    Asset-->>Gateway: Available secured assets
    Gateway->>Instruction: Create operational instruction
    Instruction->>EventBus: InstructionRequested
    EventBus->>Audit: Record immutable audit event
    Instruction-->>Gateway: Instruction accepted
    Gateway-->>Channel: Operation status
```

## Notes

- Gateway handles routing, authentication integration, throttling, and API versioning.
- Services own business rules inside their boundaries.
- Audit receives events rather than relying on after-the-fact log scraping.

