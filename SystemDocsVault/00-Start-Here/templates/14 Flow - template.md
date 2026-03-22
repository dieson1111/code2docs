---
title: "Flow - {{flow-name}}"
type: flow
system: "{{system-name}}"
status: draft
created: "{{date}}"
updated: "{{date}}"
tags:
  - "{{system-tag}}"
  - "{{module-tag}}"
  - flow
---

# Flow — {{flow-name}}

## Overview

> One-paragraph summary: what this flow does, who triggers it, and what the expected outcome is.

{{Describe the business flow.}}

## Trigger

| Attribute | Value |
|-----------|-------|
| **Initiator** | {{e.g. User action / Cron job / Webhook}} |
| **Entry point** | {{e.g. POST /api/v1/bookings}} |
| **Preconditions** | {{what must be true before this flow starts}} |

## Happy Path

```mermaid
sequenceDiagram
    participant Client
    participant API
    participant ModuleA as {{Module A}}
    participant ModuleB as {{Module B}}
    participant DB

    Client->>API: {{request}}
    API->>ModuleA: {{action}}
    ModuleA->>ModuleB: {{action}}
    ModuleB->>DB: {{query/mutation}}
    DB-->>ModuleB: {{result}}
    ModuleB-->>ModuleA: {{result}}
    ModuleA-->>API: {{response}}
    API-->>Client: {{response}}
```

## Error / Alternative Paths

```mermaid
flowchart TD
    A[Start] --> B{Validation OK?}
    B -->|No| C[Return 400 error]
    B -->|Yes| D{Auth OK?}
    D -->|No| E[Return 401 error]
    D -->|Yes| F[Process request]
    F --> G{Success?}
    G -->|No| H[Return 500 error]
    G -->|Yes| I[Return 200 success]
```

| Scenario | Condition | Response | HTTP Code |
|----------|-----------|----------|-----------|
| Validation failure | {{condition}} | {{error message}} | 400 |
| Auth failure | {{condition}} | {{error message}} | 401 |
| Not found | {{condition}} | {{error message}} | 404 |
| Server error | {{condition}} | {{error message}} | 500 |

## Modules Involved

| Module | Role in This Flow |
|--------|-------------------|
| [[Module - {{module-a}}]] | {{what it does here}} |
| [[Module - {{module-b}}]] | {{what it does here}} |

## Side Effects

- {{e.g. Sends notification email}}
- {{e.g. Updates user balance}}
- {{e.g. Creates audit log entry}}

## Facts

> [!NOTE] Fact
> {{Verified flow behaviour from code.}}

## Assumptions

> [!WARNING] Assumption
> {{Inferred flow behaviour or ordering.}}

## Open Questions

> [!CAUTION] Open Question
> {{Unclear flow behaviour or edge cases.}}

## Related Notes

- [[06 Runtime View - {{system-name}}]]
- {{Link to related modules, APIs, entities}}
