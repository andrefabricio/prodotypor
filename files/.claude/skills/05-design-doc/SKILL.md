---
name: 05-design-doc
description: "Step 5 of 8 — Design document creation. Produces the authoritative visual identity and design system specification from PRDs and inspiration research. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [section to focus on]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /05-design-doc — Design Document Creation

Create the authoritative design specification from product requirements and inspiration research.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as section focus (e.g., "color system", "typography")

---

## Prerequisites

**Hard dependencies (requires BOTH):**
1. Read `docs/workflow-status.md` — verify step 03 (PRD) status == `completed`
2. Read `docs/workflow-status.md` — verify step 04 (Inspiration) status == `completed`
3. Verify `docs/prds/*.md` exists (≥1 PRD with user stories)
4. Verify `docs/Design/Inspirations/*.md` exists (≥2 reviews)

**If prerequisites fail:**
- If step 03 missing: "Step 05 cannot start. Missing: PRDs from step 03. Run `/03-prd` first."
- If step 04 missing: "Step 05 cannot start. Missing: inspiration research from step 04. Run `/04-inspiration` first."
- If BOTH missing: Report both and suggest running them (03 and 04 can run in parallel).
- Do NOT proceed until both are completed.

**Soft dependencies:**
- Technical constraints from CLAUDE.md (framework, platform specifics)

---

## Execute

### Phase 1: Gather All Inputs

1. Read `CLAUDE.md` — project context, tech stack, architecture constraints
2. Read `.impeccable.md` — brand personality, aesthetic direction, anti-references
3. Read all PRDs in `docs/prds/` — extract feature scope, user personas, user flows
4. Read all inspiration reviews in `docs/Design/Inspirations/` — extract patterns to adopt/avoid
5. Read `templates/design-document-template.md` — output structure
6. If continuing, read existing design doc to determine what sections are complete

### Phase 2: Delegate to Design Doc Author Agent

Spawn the `design-doc-author` agent:

> Create the design document for {{PROJECT_NAME}}.
>
> Brand direction: [from .impeccable.md]
> User personas: [from PRDs]
> Feature scope: [screens and flows from PRDs]
> Inspiration insights:
>   Adopt: [from inspiration reviews]
>   Avoid: [from inspiration reviews]
> Technical constraints: [from CLAUDE.md — framework, platform]
> Template: [content of `templates/design-document-template.md`]
> Focus area: [from $ARGUMENTS if provided]
>
> Requirements:
> - Complete all 7 sections of the template
> - Every color with semantic token name and hex value
> - Every spacing value on a consistent scale
> - Every component with all states specified
> - Contrast ratios computed for all text/background combinations
> - Dark mode mapping (even if deferred)
> - Save to `docs/Design/{{PROJECT_NAME}}-design-document.md`

### Pipeline Context

Before delegating, read and summarize prior step outputs:
- Step 03 output: read PRD user personas, feature scope, key user flows
- Step 04 output: read inspiration "Relevance to {{PROJECT_NAME}}" sections — what to adopt/avoid
- Include summaries (not full files) in the agent prompt as "Pipeline Context"

### Context Management

The design doc agent needs input from multiple large files. To prevent context rot:
- Pass **summaries** of PRDs (user stories list, not full prose)
- Pass only the **Relevance** and **Color/Typography** sections from inspiration reviews
- Include full `.impeccable.md` (small file, critical context)

### Phase 3: Validate Output

1. Verify design doc exists at `docs/Design/{{PROJECT_NAME}}-design-document.md`
2. Check required sections present: Color System, Typography, Spacing, Components
3. Check: color palette has ≥8 tokens with hex values
4. Check: type scale has ≥6 roles defined
5. Check: spacing scale has ≥6 values
6. If validation passes → proceed to Phase 4
7. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (e.g., "only 5 color tokens, need ≥8")
2. Feed the error + partial output back to the agent:
   > "The design document did not meet requirements: [specific failure].
   > Current output: [section summary with counts].
   > Please fix: [specific instruction]."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Quality Gate (optional)

<!-- CUSTOMIZE: Define domain-specific quality checks for design document -->

After structural validation, optionally assess quality:
- Do all text/background combinations meet WCAG AA contrast ratios?
- Does every component spec include all states (default, pressed, disabled, error)?
- Are token names semantic (role-based, not color-based)?
- Score: pass / needs-revision

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 05 status to `needs-review`
2. Report design doc summary (token counts, section completeness) to the user
3. Prompt: "Review the design document. Run `/05-design-doc [continue]` to approve, or provide feedback."
4. Only `[continue]` after `needs-review` transitions to `completed`

### Phase 5: Report

```
Step 05 — Design Document: [status]

Output: docs/Design/{{PROJECT_NAME}}-design-document.md
Sections: [count]/7
Color tokens: [count]
Type roles: [count]
Spacing values: [count]
Components specified: [count]

Next step: /06-design-plan
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 05 section
2. If design doc exists, report section completeness and token counts
3. Report which sections are missing or incomplete

---

## Force Status Update

1. Read design doc, check for required section headings
2. Count color tokens, type roles, spacing values, components
3. Update `docs/workflow-status.md` step 05
