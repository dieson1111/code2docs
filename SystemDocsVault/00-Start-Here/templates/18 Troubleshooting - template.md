---
title: "Troubleshooting - {{topic}}"
type: troubleshooting
system: "{{system-name}}"
status: draft
created: "{{date}}"
updated: "{{date}}"
module: "{{module-name}}"
tags:
  - "{{system-tag}}"
  - troubleshooting
  - "{{module-tag}}"
---

# Troubleshooting — {{topic}}

## Symptom

> What does the user or system observe when this issue occurs?

{{Describe the visible symptom.}}

## Root Cause

> What causes this issue?

{{Explain the underlying cause.}}

## Diagnosis Steps

```mermaid
flowchart TD
    A[Symptom observed] --> B{Check condition 1?}
    B -->|Yes| C[Likely cause A]
    B -->|No| D{Check condition 2?}
    D -->|Yes| E[Likely cause B]
    D -->|No| F[Escalate to team]
    C --> G[Apply fix A]
    E --> H[Apply fix B]
```

1. {{Step 1 — what to check}}
2. {{Step 2 — what to check}}
3. {{Step 3 — what to look for}}

## Resolution

### Quick Fix
{{Immediate resolution steps.}}

### Permanent Fix
{{Long-term solution.}}

## Prevention

- {{How to prevent this from recurring}}

## Facts

> [!NOTE] Fact
> {{Verified cause and resolution from experience.}}

## Assumptions

> [!WARNING] Assumption
> {{Inferred root cause.}}

## Open Questions

> [!CAUTION] Open Question
> {{Unclear aspects of this issue.}}

## Related Notes

- [[Module - {{module-name}}]]
- {{Link to related flows or entities}}
