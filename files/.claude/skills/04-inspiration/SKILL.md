---
name: 04-inspiration
description: "Step 4 of 8 — UI inspirational research. Performs structured design audits of reference applications to inform visual direction. Can run in PARALLEL with step 03 (PRD creation). Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [app name to research]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent, WebSearch, WebFetch]
---

# /04-inspiration — UI Inspirational Research

Research and document UI/UX design patterns from reference applications.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as app name to research

---

## Prerequisites

**Hard dependencies (flexible — this step can start early):**
1. At least ONE of these must be true:
   - Step 03 (PRD) status == `completed` — full feature context available
   - `.impeccable.md` exists — brand direction available for research lens
2. If NEITHER exists, report: "Step 04 needs at least a brand direction (.impeccable.md) or completed PRDs to focus the research."

**Parallel execution note:** This step can run in PARALLEL with step 03. It does not require PRD output — inspiration research is independent competitive analysis. However, PRD context improves research relevance.

**Soft dependencies:**
- PRDs (improve relevance of research — know what features need design inspiration)
- Existing inspiration docs (avoid duplicate research)

---

## Execute

### Phase 1: Determine Research Scope

1. Read `.impeccable.md` — extract aesthetic direction, anti-references, brand personality
2. Read PRDs if available — note feature types that need design inspiration
3. Check existing research in `docs/Design/Inspirations/` — avoid duplicates
4. If `$ARGUMENTS` names a specific app, target that app
5. If no app specified, identify 2-3 candidates based on:
   - Similar emotional context or user sensitivity to {{PROJECT_NAME}}
   - Similar feature patterns (feeds, cards, timelines, circles, etc.)
   - Recognized design quality
6. Present candidates to the user for approval (if discovering independently)

### Phase 2: Delegate to Inspiration Researcher Agent

For each app to audit, spawn the `inspiration-researcher` agent:

> Perform a comprehensive design audit of [app name].
>
> Project context: {{PROJECT_NAME}} — [summary from CLAUDE.md]
> Brand direction: [from .impeccable.md]
> Feature scope: [from PRDs if available]
> Template: [content of `templates/inspiration-research-template.md`]
>
> Requirements:
> - Professional-grade design audit (≥10KB)
> - All 6 sections from the template
> - Specific measurements (hex values, pixel sizes, font names)
> - Explicit relevance assessment for {{PROJECT_NAME}}
> - Save to `docs/Design/Inspirations/[app-name]-review.md`

### Pipeline Context

Before delegating, read and summarize prior step outputs (if available):
- Step 03 output (if completed): read PRD feature list for targeted research focus
- Step .impeccable.md: brand direction for research lens
- Include this summary in the agent prompt as "Pipeline Context"

### Phase 3: Validate Output

1. Verify at least 2 inspiration review files exist in `docs/Design/Inspirations/`
2. Check each review has all 6 sections from the template
3. Check each review is ≥10KB (sufficient depth)
4. If validation passes → proceed to Phase 4
5. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (e.g., "review is only 5KB, need ≥10KB" or "missing Typography section")
2. Feed the error + partial output back to the agent:
   > "The inspiration review did not meet requirements: [specific failure].
   > Current output: [summary of what sections exist and their sizes].
   > Please expand: [specific sections that need more depth]."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Quality Gate (optional)

<!-- CUSTOMIZE: Define domain-specific quality checks for inspiration research -->

After structural validation, optionally assess quality:
- Are color palettes documented with actual hex values (not vague descriptions)?
- Are typography specs specific enough to inform a design document?
- Score: pass / needs-revision

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 04 status to `needs-review`
2. Report research summary to the user
3. Prompt: "Review the inspiration audits. Run `/04-inspiration [continue]` to approve, or provide feedback."
4. Only `[continue]` after `needs-review` transitions to `completed`

### Phase 5: Report

```
Step 04 — UI Inspiration Research: [status]

Reviews completed: [count]
  - [app-name]: [file size], [section count] sections
  - [app-name]: [file size], [section count] sections

Key patterns identified:
  Adopt: [brief list]
  Avoid: [brief list]

Next step: /05-design-doc (requires both step 03 + step 04 completed)
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 04 section
2. List review files in `docs/Design/Inspirations/` with sizes
3. Report completion (need ≥2 reviews)

---

## Force Status Update

1. Glob `docs/Design/Inspirations/*.md` — count reviews
2. Check file sizes (≥10KB threshold)
3. Update `docs/workflow-status.md` step 04
