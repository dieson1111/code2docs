---
title: Frontmatter Rules
type: vault-guide
status: reviewed
created: 2026-03-22
updated: 2026-03-22
tags:
  - meta
  - vault-structure
  - frontmatter
---

# Frontmatter Rules

## Required Fields

Every note in this vault **must** include these frontmatter fields:

```yaml
---
title: <string>            # Human-readable title
type: <enum>               # See type values below
system: <string>           # System name, e.g. ashylist-backend
status: <enum>             # draft | in-review | reviewed | stale
created: <YYYY-MM-DD>      # Date first created
updated: <YYYY-MM-DD>      # Date last modified
tags: []                   # Array of tags, see Tagging Rules
---
```

## Type Values

| Value | Used For | Folder |
|-------|----------|--------|
| `system` | Arc42 chapter docs | `01-Systems/` |
| `module` | Module documentation | `02-Modules/` |
| `flow` | Business flow / runtime | `03-Flows/` |
| `api` | API / integration docs | `04-Interfaces/` |
| `entity` | Data model / schema | `05-Data/` |
| `adr` | Architecture Decision Record | `06-Decisions/` |
| `operations` | Ops, governance, runbooks | `07-Operations/` |
| `troubleshooting` | Troubleshooting guides | `07-Operations/` |
| `vault-guide` | Vault structure docs | `00-Start-Here/` |

## Optional Fields

```yaml
---
arc42-chapter: <number>    # 1-12, for arc42 chapter docs
adr-number: <number>       # Sequential ADR number
module: <string>           # Module name for module-specific docs
replaces: <string>         # Title of note this supersedes
---
```

## Rules

1. **Always valid YAML** — use quotes around values with special characters
2. **Update `updated` date** whenever content is modified
3. **Status must follow lifecycle** — see [[00 Vault Overview#Status Lifecycle]]
4. **Tags are arrays** — even if only one tag: `tags: [ashylist-backend]`
5. **No empty fields** — omit optional fields rather than leaving them blank

## Example

```yaml
---
title: Module - Auth
type: module
system: ashylist-backend
status: draft
created: 2026-03-22
updated: 2026-03-22
tags:
  - ashylist-backend
  - auth
  - security
module: auth
---
```

## Related Notes

- [[00 Vault Overview]]
- [[02 Tagging Rules]]
- [[01 Documentation Style Guide]]
