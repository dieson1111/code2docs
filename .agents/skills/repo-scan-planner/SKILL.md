---
name: repo-scan-planner
description: Scan a codebase and produce a documentation plan identifying all modules, flows, APIs, entities, and decisions that need documentation.
---

# Skill: repo-scan-planner

## Purpose

Scan an entire codebase (source code, configs, API specs, DB schemas, existing docs) and produce a structured **documentation plan** — a prioritised checklist of documents to create or update, following the vault's arc42 template structure.

## When to Use

- Starting documentation for a new system
- Onboarding a new codebase into the vault
- Periodic review to find documentation gaps
- After a major refactoring or feature addition

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| Repository path | Yes | Root directory of the codebase to scan |
| Vault path | Yes | Root of the Obsidian vault (`SystemDocsVault/`) |
| System name | Yes | Name to use in frontmatter `system` field |
| Existing doc plan | No | Previous `doc-plan.md` for incremental updates |

## Outputs

| Output | Format | Location |
|--------|--------|----------|
| `doc-plan.md` | Markdown checklist | Vault `07-Operations/` |
| Console summary | Text | stdout |

## Constraints

1. **Never generate documentation directly** — only produce the plan
2. **Never modify existing vault files** — only read them to check coverage
3. **Mark confidence level** for each planned document (high / medium / low)
4. **Flag files that could not be parsed** or understood
5. **Respect token limits** — process large codebases in chunks (one module at a time)

## Workflow Steps

### Step 1: Discover Project Structure
```
- Read the project root: package.json / pubspec.yaml / Dockerfile / compose files
- Identify the tech stack, entry points, and folder layout
- List all source directories (ignore node_modules, build, .git)
```

### Step 2: Identify Modules
```
- Scan src/modules/ (or equivalent) for module directories
- For each module, identify: controllers, services, models, routes, validators
- Record module name, file count, estimated complexity
```

### Step 3: Identify Data Models / Entities
```
- Find all Mongoose schemas / model files / migration files
- Record entity name, fields, relationships (references to other models)
- Note indexes and validators
```

### Step 4: Identify API Endpoints
```
- Parse route files and/or swagger.yaml / openapi spec
- Record: method, path, controller, auth requirement
- Group endpoints by module
```

### Step 5: Identify Business Flows
```
- Trace key user journeys through the code (e.g. login, payment, booking)
- Identify cross-module interactions
- Record: flow name, modules involved, entry point, key steps
```

### Step 6: Check Existing Documentation
```
- Read all existing notes in the vault
- Compare against discovered modules, entities, APIs, flows
- Identify: missing docs, outdated docs (code changed after doc updated), covered docs
```

### Step 7: Produce Documentation Plan
```
- Generate doc-plan.md with:
  - [ ] items for each document to create (with template type and target path)
  - [x] items for documents that already exist and are current
  - Priority: critical flows first, then modules, then supporting docs
  - Confidence level per item
  - Estimated effort (small / medium / large)
```

## Doc Plan Output Format

```markdown
---
title: Documentation Plan
type: operations
system: {{system-name}}
status: draft
created: {{date}}
updated: {{date}}
tags:
  - meta
  - doc-plan
---

# Documentation Plan — {{system-name}}

## Summary
- Modules found: {{n}}
- Entities found: {{n}}
- API groups found: {{n}}
- Flows identified: {{n}}
- Existing docs: {{n}}
- Docs to create: {{n}}
- Docs to update: {{n}}

## Documents to Create

### Critical (create first)
- [ ] `Flow - Booking Payment.md` → `03-Flows/` | confidence: high | effort: medium
- [ ] `Module - Auth.md` → `02-Modules/` | confidence: high | effort: small

### Standard
- [ ] `Entity - User.md` → `05-Data/` | confidence: high | effort: small
- [ ] `API - User Management.md` → `04-Interfaces/` | confidence: medium | effort: medium

### System-Level
- [ ] `00 System Index - Ashylist Backend.md` → `01-Systems/`
- [ ] `03 Context and Scope - Ashylist Backend.md` → `01-Systems/`

## Existing Docs (up to date)
- [x] `00 Vault Overview.md`

## Existing Docs (needs update)
- [ ] (update) `README.md` → last modified before recent code changes

## Unable to Analyse
- {{file or directory that could not be parsed, with reason}}
```

## Quality Checklist

- [ ] Every module directory has a corresponding planned doc
- [ ] Every Mongoose model has a corresponding entity doc planned
- [ ] Every route group has a corresponding API doc planned
- [ ] At least 3 critical flows identified with cross-module coverage
- [ ] Existing docs checked for staleness
- [ ] Confidence levels assigned to every item
- [ ] No documents are generated — only the plan

## Failure Modes

| Failure | Detection | Recovery |
|---------|----------|----------|
| Codebase too large for single scan | Token limit hit | Scan one module at a time, merge results |
| Non-standard folder structure | No modules/ dir found | Fall back to file-by-file analysis |
| No API spec found | No swagger/openapi file | Infer from route files |
| Existing docs not in vault | Docs in repo but not vault | Note in plan with recommended action |

## Handoff to Next Skill

Pass `doc-plan.md` to **obsidian-doc-writer**. The writer uses:
- The checklist items (unchecked = to create)
- The template type (Module, Flow, API, Entity, ADR)
- The target path
- The system name and confidence level

---

## Master Prompt

Use this prompt when invoking this skill on any agent platform:

```
You are a **senior system analyst** specialising in codebase documentation.

**Your task**: Scan the provided codebase and produce a documentation plan (`doc-plan.md`).

**Input**:
- Repository path: {{repo-path}}
- Vault path: {{vault-path}}
- System name: {{system-name}}

**Rules**:
1. NEVER generate documentation — only produce the plan
2. NEVER modify existing vault files — only read them
3. Scan systematically: project structure → modules → data models → API endpoints → business flows → existing docs
4. For each planned document, specify: document type (Module/Flow/API/Entity/ADR), target vault path, confidence level (high/medium/low), estimated effort (small/medium/large)
5. Prioritise: critical business flows first, then core modules, then supporting docs
6. Flag anything you cannot parse or understand as "Unable to analyse" with reason
7. Mark uncertain identifications with confidence: low and explain why
8. Check existing vault docs and mark them as up-to-date or needing update
9. Use the frontmatter schema from the vault's Frontmatter Rules
10. Output the plan in the exact format specified in the skill definition

**Quality bar**:
- Every module directory must have a planned document
- Every data model must have a planned entity document
- At least 3 critical cross-module flows must be identified
- Zero hallucinated modules or flows — if unsure, mark as confidence: low

**Handling missing information**:
- If a file cannot be read, log it under "Unable to Analyse"
- If a module's purpose is unclear, mark confidence: low and add note
- NEVER invent behaviour — only document what is verifiable from code

**Output**: A single `doc-plan.md` file saved to `{{vault-path}}/07-Operations/`
```
