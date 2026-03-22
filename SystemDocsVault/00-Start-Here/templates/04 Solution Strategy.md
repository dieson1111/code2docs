---
title: "04 Solution Strategy - {{system-name}}"
type: system
system: "{{system-name}}"
arc42-chapter: 4
status: draft
created: "{{date}}"
updated: "{{date}}"
tags:
  - "{{system-tag}}"
  - arc42
---

# 04 Solution Strategy — {{system-name}}

## Technology Decisions

| Area | Decision | Rationale |
|------|----------|-----------|
| Runtime | {{e.g. Node.js}} | {{why}} |
| Framework | {{e.g. Express}} | {{why}} |
| Database | {{e.g. MongoDB}} | {{why}} |
| Frontend | {{e.g. Flutter}} | {{why}} |
| Auth | {{e.g. JWT}} | {{why}} |

## Top-Level Decomposition

> How is the system broken down at the highest level?

```mermaid
flowchart TB
    subgraph "{{system-name}}"
        A[API Layer] --> B[Business Logic / Modules]
        B --> C[Data Access Layer]
        C --> D[(Database)]
    end
    E[Mobile App] --> A
```

## Key Design Decisions

| Quality Goal | Approach | Related ADR |
|-------------|----------|-------------|
| {{e.g. Security}} | {{e.g. JWT + middleware guards}} | [[ADR - {{nnn}} {{title}}]] |
| {{e.g. Scalability}} | {{approach}} | {{ADR link or N/A}} |

## Facts

> [!NOTE] Fact
> {{Verified strategy decisions from code/config.}}

## Assumptions

> [!WARNING] Assumption
> {{Inferred design rationale.}}

## Open Questions

> [!CAUTION] Open Question
> {{Unclear strategic decisions.}}

## Related Notes

- [[03 Context and Scope - {{system-name}}]]
- [[05 Building Block View - {{system-name}}]]
- [[09 Architectural Decisions - {{system-name}}]]
