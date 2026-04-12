---
name: 06-design-plan
description: "Step 6 of 8 — Design plan creation. Produces a Figma construction plan with screen inventory, batch organization, frame tree specs, and token sync tracking. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [batch or screen to plan]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /06-design-plan — Design Plan Creation

Create the Figma construction plan that the figma-designer agent will execute.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as specific batch/screen to plan

---

## Prerequisites

**Hard dependencies:**
1. Read `docs/workflow-status.md` — verify step 05 (Design Doc) status == `completed`
2. Read `docs/workflow-status.md` — verify step 03 (PRD) status == `completed`
3. Verify `docs/Design/{{PROJECT_NAME}}-design-document.md` exists with color + typography sections
4. Verify `docs/prds/*.md` exists with user stories

**If prerequisites fail:**
- If step 05 missing: "Step 06 cannot start. Missing: design document. Run `/05-design-doc` first."
- If step 03 missing: "Step 06 cannot start. Missing: PRDs with user stories. Run `/03-prd` first."
- Do NOT proceed.

**Soft dependencies:**
- Figma file already created (nice but not required — plan can be written before file exists)
- `.claude/knowledge/figma-mcp-reference.md` (MCP API patterns for call budgeting)

---

## Execute

### Phase 1: Gather All Inputs

1. Read `CLAUDE.md` — project context
2. Read `.impeccable.md` — brand personality, design principles
3. Read `docs/Design/{{PROJECT_NAME}}-design-document.md` — all tokens, component specs
4. Read all PRDs in `docs/prds/` — extract user stories for screen mapping
5. Read `.claude/knowledge/figma-mcp-reference.md` — API patterns, call budget
6. Read `templates/design-plan-template.md` — output structure
7. If continuing, read existing plan to determine what sections are complete

### Phase 2: Delegate to Design Planner Agent

Spawn the `design-planner` agent:

> Create the Figma construction plan for {{PROJECT_NAME}}.
>
> Design document: [content — tokens, components, principles]
> PRD user stories: [list with story IDs]
> MCP reference: [call budget, API patterns]
> Template: [content of `templates/design-plan-template.md`]
> Focus: [from $ARGUMENTS if specific batch/screen]
>
> Requirements:
> - Map every PRD user story to at least one screen
> - Organize screens into batches by user flow (4-7 per batch)
> - Write consistency rules referencing design doc token names
> - Write inline frame tree specs for at least batch 1
> - Include token sync log structure
> - Include component instance reference table (empty, populated during construction)
> - Include execution checklists per batch
> - Save to `docs/Design/figma-design-plan.md`

### Pipeline Context

Before delegating, read and summarize prior step outputs:
- Step 03 output: read PRD user story IDs and titles for screen mapping
- Step 05 output: read design doc token names and component names (not full specs)
- Include summaries in the agent prompt as "Pipeline Context"

### Context Management

The design plan agent reads the full design document (potentially 50KB+). To prevent context rot:
- Pass only the **token tables** and **component names** from the design doc, not full prose
- Pass PRD user story IDs and titles, not full Given/When/Then criteria
- Include figma-mcp-reference.md's **Call Budget** section, not the full file

### Phase 3: Validate Output

1. Verify `docs/Design/figma-design-plan.md` exists
2. Check: screen inventory has all PRD stories mapped
3. Check: at least 1 batch with frame tree specs
4. Check: consistency rules section exists
5. Check: token sync log section exists
6. If validation passes → proceed to Phase 4
7. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (e.g., "3 PRD stories have no mapped screen")
2. Feed the error + partial output back to the agent:
   > "The design plan did not meet requirements: [specific failure].
   > Current output: [screen count, batch count, missing mappings].
   > Please fix: [specific instruction]."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 06 status to `needs-review`
2. Report plan summary (screen count, batch count, spec coverage) to the user
3. Prompt: "Review the design plan. Run `/06-design-plan [continue]` to approve, or provide feedback."
4. Only `[continue]` after `needs-review` transitions to `completed`

### Phase 5: Report

```
Step 06 — Design Plan: [status]

Output: docs/Design/figma-design-plan.md
Total screens: [count] across [batch count] batches
  Batch 1: [screen count] screens ([flow name]) — specs: [written/pending]
  Batch 2: [screen count] screens ([flow name]) — specs: pending
  ...
Components referenced: [count]
Consistency rules: [count]

Next step: /07-figma
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 06 section
2. If plan exists, report: screen count, batch count, spec completion
3. Report which batches have frame tree specs vs pending

---

## Force Status Update

1. Read `docs/Design/figma-design-plan.md`
2. Count screens, batches, specs written
3. Check token sync log freshness
4. Update `docs/workflow-status.md` step 06
