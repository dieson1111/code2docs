---
title: "03 Context and Scope - {{system-name}}"
type: system
system: "{{system-name}}"
arc42-chapter: 3
status: draft
created: "{{date}}"
updated: "{{date}}"
tags:
  - "{{system-tag}}"
  - arc42
  - integration
---

# 03 Context and Scope — {{system-name}}

## Business Context

> Who interacts with this system and for what purpose?

```mermaid
C4Context
    title Business Context — {{system-name}}

    Person(user, "End User", "{{description}}")
    Person(admin, "Admin", "{{description}}")

    System(system, "{{system-name}}", "{{core purpose}}")

    System_Ext(payment, "Payment Gateway", "{{e.g. Stripe}}")
    System_Ext(email, "Email Service", "{{e.g. SendGrid}}")

    Rel(user, system, "{{interaction}}")
    Rel(admin, system, "{{interaction}}")
    Rel(system, payment, "{{interaction}}")
    Rel(system, email, "{{interaction}}")
```

| Partner | Description | Interface |
|---------|-------------|-----------|
| {{partner}} | {{what it does}} | {{REST API / webhook / SDK}} |

## Technical Context

> How does the system connect technically to its environment?

```mermaid
C4Container
    title Technical Context — {{system-name}}

    Person(user, "User", "Mobile / Web")

    Container(api, "API Server", "Node.js / Express", "Handles all API requests")
    ContainerDb(db, "Database", "MongoDB", "Stores all application data")
    Container(mobile, "Mobile App", "Flutter", "User-facing application")

    Rel(user, mobile, "Uses")
    Rel(mobile, api, "REST API calls")
    Rel(api, db, "Reads/Writes")
```

| Channel | From | To | Protocol | Notes |
|---------|------|------|----------|-------|
| {{channel}} | {{source}} | {{target}} | {{e.g. HTTPS REST}} | {{notes}} |

## Facts

> [!NOTE] Fact
> {{Verified integrations and context boundaries.}}

## Assumptions

> [!WARNING] Assumption
> {{Inferred external dependencies.}}

## Open Questions

> [!CAUTION] Open Question
> {{Unclear integration points or boundaries.}}

## Related Notes

- [[02 Constraints - {{system-name}}]]
- [[04 Solution Strategy - {{system-name}}]]
- [[05 Building Block View - {{system-name}}]]
