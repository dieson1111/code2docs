---
title: Documentation Style Guide
type: vault-guide
status: reviewed
created: 2026-03-22
updated: 2026-03-22
tags:
  - meta
  - vault-structure
  - style-guide
---

# Documentation Style Guide

## General Principles

- **Explain WHY, not just WHAT** — don't restate code; explain responsibility, rules, edge cases, risks
- **Small notes over giant documents** — one module = one note, one flow = one note
- **Strong linking over deep nesting** — use `[[wiki-links]]` generously
- **Mark uncertainty explicitly** — facts, assumptions, and open questions must be visually distinct

## Writing Structure

Every document should follow this structure:

```
# Title
## Overview (1-3 sentences)
## Main Content (sections vary by template)
## Facts
## Assumptions
## Open Questions
## Related Notes
```

## Uncertainty Handling

Use Obsidian callouts to distinguish knowledge quality:

### Facts (verified from code)
```markdown
> [!NOTE] Fact
> Users are authenticated via JWT tokens issued by the auth module.
```

### Assumptions (inferred, not confirmed)
```markdown
> [!WARNING] Assumption
> The token expiry is set to 24 hours based on config, but this may vary per environment.
```

### Open Questions (unknown)
```markdown
> [!CAUTION] Open Question
> It is unclear whether the payment retry logic handles partial refunds. Needs confirmation from the team.
```

## Mermaid Diagram Guidelines

| Use Case | Diagram Type | When |
|----------|-------------|------|
| System/component structure | C4 (Context, Container, Component) | Building Block View, Module docs |
| Business process / API call chain | Sequence diagram | Flow docs, Runtime View |
| Decision logic / branching | Flowchart | Flow docs, Troubleshooting |
| Infrastructure / deployment | Deployment diagram | Deployment View |
| Data relationships | ER diagram | Entity docs, Data Model |

### Example: Sequence Diagram

```markdown
​```mermaid
sequenceDiagram
    participant Client
    participant API
    participant Auth
    participant DB
    Client->>API: POST /login
    API->>Auth: validateCredentials()
    Auth->>DB: findUser()
    DB-->>Auth: user record
    Auth-->>API: JWT token
    API-->>Client: 200 + token
​```
```

## Tone and Style

- **Language**: English (technical sections), 中文 OK for internal notes and comments
- **Tense**: Present tense for current behaviour, past tense for historical decisions
- **Audience**: System analysts, developers, maintenance team, new joiners
- **Conciseness**: Favour bullet points and tables over long paragraphs

## File Naming

See [[00 Vault Overview]] for naming conventions.

## Related Notes

- [[00 Vault Overview]]
- [[02 Tagging Rules]]
- [[03 Frontmatter Rules]]
- [[04 Review Workflow]]
