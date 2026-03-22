---
title: Tagging Rules
type: vault-guide
status: reviewed
created: 2026-03-22
updated: 2026-03-22
tags:
  - meta
  - vault-structure
  - tagging
---

# Tagging Rules

## Tag Categories

### System Tags
Identify which system a note belongs to.
- `ashylist-backend`
- `ashylist-flutter`

### Document Type Tags
Match the `type` frontmatter field.
- `arc42`, `module`, `flow`, `api`, `entity`, `adr`, `operations`, `troubleshooting`

### Module Tags
Name of the backend module (lowercase, hyphenated).
- `auth`, `users`, `payments`, `classes`, `enrolments`, `schedules`, `tokens`, `packages`, `plans`, `promotions`, `rentals`, `rooms`, `spots`, `studios`, `notifications`, `news`, `systems`, `security`

### Concern Tags
Cross-cutting technical concerns.
- `security`, `performance`, `data-model`, `integration`, `validation`, `caching`, `error-handling`, `logging`, `deployment`

### Status Tags (use sparingly)
Only when you need to filter by status beyond frontmatter.
- `needs-confirmation`, `has-open-questions`

## Rules

1. **Use existing tags** — check this list before creating new ones
2. **Lowercase, hyphenated** — `error-handling` not `ErrorHandling`
3. **No redundancy** — don't tag `module` if `type: module` is in frontmatter
4. **Minimum 2 tags** — every note should have at least a system tag + one other
5. **Maximum 6 tags** — keep tags focused and useful
6. **New tags** — add to this file first, then use; avoid one-off tags

## Examples

| Note | Tags |
|------|------|
| `Module - Auth.md` | `ashylist-backend`, `auth`, `security` |
| `Flow - Booking Payment.md` | `ashylist-backend`, `payments`, `classes` |
| `Entity - User.md` | `ashylist-backend`, `users`, `data-model` |
| `ADR - 001 Use MongoDB.md` | `ashylist-backend`, `data-model` |

## Related Notes

- [[00 Vault Overview]]
- [[03 Frontmatter Rules]]
- [[01 Documentation Style Guide]]
