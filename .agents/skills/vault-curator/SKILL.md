---
name: vault-curator
description: Validate, organise, and maintain Obsidian vault consistency — checking naming, tags, links, frontmatter, and flagging stale or missing documentation.
---

# Skill: vault-curator

## Purpose

Validate, organise, and maintain the Obsidian vault's consistency. This skill checks naming conventions, tags, frontmatter, cross-links, and flags stale or missing documentation. It **never deletes** content — only flags, renames, adds metadata, or suggests actions.

## When to Use

- After `obsidian-doc-writer` has generated a batch of documents
- Periodically (e.g. monthly) to maintain vault health
- After code changes that may have made docs stale
- When onboarding a new team member (to ensure vault is clean)

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| Vault path | Yes | Root of the Obsidian vault (`SystemDocsVault/`) |
| Vault rules | Yes | `00-Start-Here/` guide files (auto-discovered) |
| Source repo path | No | For staleness checks against code |

## Outputs

| Output | Format | Location |
|--------|--------|----------|
| `vault-health-report.md` | Markdown | `07-Operations/` |
| Auto-fixed files | In-place edits | Various vault folders |
| Suggested actions | Within report | — |

## Constraints

1. **Never delete any file** — only rename, move, or flag
2. **Never modify document content** — only fix metadata (frontmatter, tags, links)
3. **Log every change** in the health report
4. **Preserve existing wiki-links** — if renaming, update references
5. **Respect status lifecycle** — don't change reviewed → draft without justification

## Workflow Steps

### Step 1: Load Vault Rules
```
- Read: 00 Vault Overview.md, 01 Documentation Style Guide.md, 02 Tagging Rules.md, 03 Frontmatter Rules.md
- Build validation rules from these files
```

### Step 2: Scan All Notes
```
- List every .md file in the vault (excluding 00-Start-Here/templates/)
- Parse frontmatter for each file
- Extract: title, type, system, status, tags, created, updated dates
- Extract: all [[wiki-links]] and section headers
```

### Step 3: Validate Naming Conventions
```
For each file, check:
- [ ] File name matches the pattern for its type (Module - X.md, Flow - X.md, etc.)
- [ ] File is in the correct folder for its type
- [ ] No spaces around hyphens in names
- [ ] ADR numbers are sequential with no gaps
```

### Step 4: Validate Frontmatter
```
For each file, check:
- [ ] All required fields present (title, type, system, status, created, updated, tags)
- [ ] type value is from the allowed enum
- [ ] status value is from the allowed enum
- [ ] Dates are valid YYYY-MM-DD format
- [ ] tags is an array (not string)
```

### Step 5: Validate Tags
```
For each file, check:
- [ ] Tags use lowercase-hyphenated format
- [ ] Tags exist in the approved tag list (02 Tagging Rules.md)
- [ ] At least 2 tags per note
- [ ] No more than 6 tags per note
- [ ] No redundant tags (e.g. type tag matching frontmatter type)
```

### Step 6: Validate Cross-Links
```
- Build a link graph: note → [linked notes]
- Check for:
  - [ ] Broken links (target note doesn't exist)
  - [ ] Orphan notes (no incoming links)
  - [ ] Missing Related Notes section
  - [ ] Module notes not linked from Building Block View
  - [ ] Flow notes not linked from Runtime View
```

### Step 7: Check for Staleness
```
If source repo path is provided:
- Compare file `updated` date vs source code modification date
- Flag notes where source code changed after doc was last updated
- Set suggestion: change status to stale
```

### Step 8: Check for Duplicates
```
- Look for notes with similar titles or covering the same module/flow
- Flag potential duplicates for human review
```

### Step 9: Generate Health Report
```
Output vault-health-report.md with:
- Summary statistics (total notes, by status, by type)
- Issues found (grouped by severity: error / warning / info)
- Auto-fixes applied (with before/after)
- Suggested manual actions
- Orphan notes list
- Stale notes list
```

### Step 10: Auto-Fix (Safe Fixes Only)
```
Safe to auto-fix:
- Add missing updated date (set to today)
- Normalise tag casing (to lowercase-hyphenated)
- Add missing Related Notes section (empty)
- Fix frontmatter formatting (quotes around special chars)

NOT safe to auto-fix (suggest only):
- Rename files
- Move files between folders
- Change status
- Add/remove tags that change meaning
- Modify content
```

## Health Report Format

```markdown
---
title: Vault Health Report
type: operations
system: all
status: draft
created: {{date}}
updated: {{date}}
tags:
  - meta
  - vault-health
---

# Vault Health Report — {{date}}

## Summary

| Metric | Count |
|--------|-------|
| Total notes | {{n}} |
| Draft | {{n}} |
| In-review | {{n}} |
| Reviewed | {{n}} |
| Stale | {{n}} |

## Issues

### 🔴 Errors (must fix)
- {{file}}: Missing required frontmatter field `type`
- {{file}}: File in wrong folder (expected 02-Modules/, found 03-Flows/)

### 🟡 Warnings (should fix)
- {{file}}: No incoming links (orphan note)
- {{file}}: Only 1 tag (minimum 2 required)

### 🔵 Info
- {{file}}: Source code changed after doc updated — consider marking stale

## Auto-Fixes Applied
- {{file}}: Normalised tag `ErrorHandling` → `error-handling`
- {{file}}: Added empty Related Notes section

## Suggested Actions
- [ ] Rename `{{file}}` → `{{correct name}}`
- [ ] Move `{{file}}` from `{{current}}` to `{{target}}`
- [ ] Review `{{file}}` — marked as stale

## Orphan Notes
- {{list of notes with no incoming links}}
```

## Quality Checklist

- [ ] Every note in the vault has been scanned
- [ ] All naming convention violations are reported
- [ ] All broken wiki-links are identified
- [ ] All orphan notes are listed
- [ ] Auto-fixes are logged with before/after
- [ ] No content has been deleted
- [ ] No content sections have been modified
- [ ] Health report is complete and saved

## Failure Modes

| Failure | Detection | Recovery |
|---------|----------|----------|
| Cannot parse frontmatter | YAML parse error | Log file as error, skip validation |
| Too many files to scan | Token limit | Process folder by folder, merge reports |
| Circular links detected | Link graph cycle | Report but don't treat as error |
| Cannot access source repo | Path not found | Skip staleness checks, note in report |

## Handoff Rules

This skill is the **end of the pipeline**. After curation:
1. **Human review** the health report
2. Address errors and warnings
3. Optionally re-run `repo-scan-planner` for gap analysis
4. Cycle back to `obsidian-doc-writer` for any new/updated docs

---

## Master Prompt

Use this prompt when invoking this skill on any agent platform:

```
You are a **documentation quality engineer** specialising in Obsidian vault maintenance.

**Your task**: Validate and curate the Obsidian vault for consistency, completeness, and correctness.

**Input**:
- Vault path: {{vault-path}}
- Source repo path: {{repo-path}} (optional, for staleness checks)

**Rules**:
1. NEVER delete any file — only flag, rename, or add metadata
2. NEVER modify document content sections — only fix frontmatter, tags, and structural elements
3. Load vault rules from `00-Start-Here/` before validating
4. Check every note for: naming convention, correct folder, frontmatter completeness, tag compliance, cross-links
5. Build a link graph and identify: broken links, orphan notes, missing Related Notes sections
6. If source repo is provided: compare `updated` dates to identify stale docs
7. Auto-fix ONLY safe changes: tag casing, missing empty sections, date formatting
8. Log EVERY change in the health report
9. Group issues by severity: Error (must fix) / Warning (should fix) / Info (nice to fix)
10. Output a complete `vault-health-report.md` to `07-Operations/`

**Quality bar**:
- 100% of notes scanned
- All broken links identified
- All naming violations reported
- Auto-fixes logged with before/after
- Health report summary statistics are accurate

**Handling issues**:
- If YAML cannot be parsed: log as error, skip note, continue
- If file is in wrong folder: suggest move (don't auto-move)
- If status change is needed: suggest (don't auto-change)
- If content is wrong: this is NOT your job — flag for obsidian-doc-writer

**Output**: `vault-health-report.md` saved to `{{vault-path}}/07-Operations/`
```
