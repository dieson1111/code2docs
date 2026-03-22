---
title: "09 Architectural Decisions - {{system-name}}"
type: system
system: "{{system-name}}"
arc42-chapter: 9
status: draft
created: "{{date}}"
updated: "{{date}}"
tags:
  - "{{system-tag}}"
  - arc42
---

# 09 Architectural Decisions — {{system-name}}

## ADR Index

| # | Title | Status | Date | Link |
|---|-------|--------|------|------|
| 001 | {{title}} | {{accepted/superseded/deprecated}} | {{date}} | [[ADR - 001 {{title}}]] |
| 002 | {{title}} | {{status}} | {{date}} | [[ADR - 002 {{title}}]] |

## Decision Categories

### Technology Choices
- {{e.g. Why MongoDB over PostgreSQL}}
- {{e.g. Why Express over Fastify}}

### Architecture Patterns
- {{e.g. Module-based folder structure}}
- {{e.g. Middleware chain for auth}}

### Integration Decisions
- {{e.g. Payment gateway selection}}
- {{e.g. Email service choice}}

## How to Add a New ADR

1. Copy the [[17 ADR - template]] from `00-Start-Here/templates/`
2. Save to `06-Decisions/` as `ADR - <NNN> <Title>.md`
3. Add an entry to this index
4. Set `status: draft` and request review

## Facts

> [!NOTE] Fact
> {{Verified decisions from code, config, or team.}}

## Assumptions

> [!WARNING] Assumption
> {{Inferred decision rationale.}}

## Open Questions

> [!CAUTION] Open Question
> {{Decisions with unclear rationale.}}

## Related Notes

- [[08 Crosscutting Concepts - {{system-name}}]]
- [[10 Quality Requirements - {{system-name}}]]
- {{Link to each ADR note}}
