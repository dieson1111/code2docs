---
name: obsidian-doc-writer
description: Generate individual Markdown documents following arc42 templates, with proper frontmatter, Mermaid diagrams, wiki-links, and explicit uncertainty marking.
---

# Skill: obsidian-doc-writer

## Purpose

Take a documentation plan (from `repo-scan-planner`) and generate individual Markdown documents following the vault's arc42 templates. Each document is a complete, Obsidian-compatible note with frontmatter, Mermaid diagrams, wiki-links, and clearly separated facts/assumptions/open questions.

## When to Use

- After `repo-scan-planner` has produced a `doc-plan.md`
- When creating a single new document for a known module/flow/API/entity
- When updating an existing document after code changes

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| Documentation plan | Yes | `doc-plan.md` from repo-scan-planner |
| Source code access | Yes | Ability to read relevant source files |
| Template directory | Yes | `SystemDocsVault/00-Start-Here/templates/` |
| Vault path | Yes | Root of the Obsidian vault |
| System name | Yes | Name to use in frontmatter |

## Outputs

| Output | Format | Location |
|--------|--------|----------|
| Individual `.md` files | Obsidian Markdown | Appropriate vault subfolder |
| Generation report | Markdown | `07-Operations/` |

## Constraints

1. **Always use the correct template** — select based on document type
2. **Never invent code behaviour** — only document what is verifiable from source
3. **Mark all uncertainty explicitly** — use the callout system (`Fact` / `Assumption` / `Open Question`)
4. **Include Mermaid diagrams** where the template calls for them
5. **Use wiki-links** (`[[Note Name]]`) for all cross-references
6. **Follow the frontmatter schema** defined in `Frontmatter Rules.md`
7. **Follow the tagging rules** defined in `Tagging Rules.md`
8. **One document at a time** — do not batch-generate without review points

## Workflow Steps

### Step 1: Read the Documentation Plan
```
- Parse doc-plan.md
- Identify the next unchecked item to generate
- Note the document type, target path, and related source files
```

### Step 2: Select and Read Template
```
- Load the appropriate template from 00-Start-Here/templates/
- Template mapping:
  - Module doc → 13 Module - template.md
  - Flow doc → 14 Flow - template.md
  - API doc → 15 API - template.md
  - Entity doc → 16 Entity - template.md
  - ADR → 17 ADR - template.md
  - Troubleshooting → 18 Troubleshooting - template.md
  - Arc42 chapter → 01-12 chapter templates
```

### Step 3: Read Source Code
```
- Read all relevant source files for the target document
- For a module: controller, service, model, routes, validators
- For a flow: trace the call chain across modules
- For an entity: model file, schema, indexes, relationships
- For an API: route file, controller, swagger spec
```

### Step 4: Fill Template
```
- Replace all {{placeholders}} with actual content from source code
- Write overview section first (1-3 sentences)
- Fill in tables with verified information
- Create Mermaid diagrams based on actual code structure:
  - C4 Component diagrams for modules
  - Sequence diagrams for flows and API calls
  - ER diagrams for entities
  - Flowcharts for decision logic
- Add facts, assumptions, and open questions
```

### Step 5: Add Metadata
```
- Fill frontmatter: title, type, system, status (draft), dates, tags
- Add Related Notes section with wiki-links to:
  - Parent system index
  - Related modules, flows, APIs, entities
  - Relevant ADRs
```

### Step 6: Save Document
```
- Save to the target vault subfolder
- File name must follow naming conventions
- Update doc-plan.md: mark item as [x] completed
```

## Diagram Selection Guide

| Document Type | Required Diagrams |
|--------------|-------------------|
| Module | C4 Component diagram (internal structure) |
| Flow | Sequence diagram (happy path) + Flowchart (error paths) |
| API | Sequence diagram (call chain) |
| Entity | ER diagram + State diagram (lifecycle) |
| Building Block View | C4 Container + Component diagrams |
| Runtime View | Sequence diagrams |
| Deployment View | Deployment/infrastructure diagram |
| Context and Scope | C4 Context diagram |
| Troubleshooting | Flowchart (diagnosis) |

## Quality Checklist

- [ ] Frontmatter is complete and valid YAML
- [ ] All `{{placeholders}}` have been replaced
- [ ] Mermaid diagrams render correctly (valid syntax)
- [ ] Facts are sourced from actual code (cite file paths)
- [ ] Assumptions are clearly labelled with `> [!WARNING]`
- [ ] Open questions use `> [!CAUTION]`
- [ ] At least 2 related notes are wiki-linked
- [ ] Tags follow `Tagging Rules.md`
- [ ] File name follows naming conventions
- [ ] No code is copy-pasted — logic is explained in natural language

## Failure Modes

| Failure | Detection | Recovery |
|---------|----------|----------|
| Source file too complex to summarise | Confidence drops below medium | Mark as open question, write what is understood |
| Template doesn't fit the content | Required sections don't apply | Add N/A with explanation, don't force-fit |
| Cross-module flow too complex | More than 5 modules involved | Break into sub-flows, link between them |
| Cannot determine relationships | Model references unclear | Mark as assumption, add to open questions |

## Handoff to Next Skill

After generating documents, hand off to **vault-curator** for:
- Consistency checking across all generated docs
- Cross-link validation
- Tag normalisation
- Naming convention enforcement

---

## Master Prompt

Use this prompt when invoking this skill on any agent platform:

```
You are a **senior technical writer** specialising in system documentation.

**Your task**: Generate a Markdown document for the Obsidian vault following the provided template.

**Input**:
- Document to generate: {{doc-type}} for {{subject-name}}
- Source files: {{list of relevant source file paths}}
- Template: {{template file path}}
- System name: {{system-name}}
- Target path: {{vault subfolder}}

**Rules**:
1. Use the provided template — fill every section, replace every {{placeholder}}
2. NEVER invent behaviour — only document what you can verify from source code
3. Mark knowledge quality explicitly:
   - `> [!NOTE] Fact` — verified from code
   - `> [!WARNING] Assumption` — inferred, not confirmed
   - `> [!CAUTION] Open Question` — unknown, needs investigation
4. Include Mermaid diagrams as specified by the template:
   - C4 for structure, sequence for flows, ER for data models, flowchart for logic
5. Use `[[wiki-links]]` for all cross-references to other vault notes
6. Follow the frontmatter schema exactly (title, type, system, status: draft, created, updated, tags)
7. Follow tag naming: lowercase, hyphenated, from the approved tag list
8. Explain logic and responsibility — do NOT copy-paste code
9. Keep the overview section to 1-3 sentences
10. Always include a Related Notes section at the bottom

**Quality bar**:
- Every section has content (use "N/A — {{reason}}" if not applicable)
- Every Mermaid diagram has valid syntax
- At least 1 fact, 1 assumption or open question per document
- File name matches the naming convention

**Handling missing information**:
- If behaviour is unclear: mark as `> [!CAUTION] Open Question` with what you DO know
- If a relationship is uncertain: mark as `> [!WARNING] Assumption`
- NEVER leave a placeholder unfilled — replace with actual content or explicit N/A
- NEVER hallucinate module names, endpoints, or entity fields

**Output**: A single `.md` file with proper frontmatter, saved to the specified vault subfolder with `status: draft`
```
