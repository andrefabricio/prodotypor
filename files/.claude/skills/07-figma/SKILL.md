---
name: 07-figma
description: "Step 7 of 8 — Figma creation. Constructs screens in Figma via MCP following the design plan. Handles file setup, variable creation, component building, and screen construction. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [batch number or screen name]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /07-figma — Figma Creation

Construct screens in Figma via MCP following the design plan.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as batch number or screen name to construct

---

## Prerequisites

**Hard dependencies:**
1. Read `docs/workflow-status.md` — verify step 06 (Design Plan) status == `completed`
2. Verify `docs/Design/figma-design-plan.md` exists with screen inventory + frame tree specs

**If prerequisites fail:**
- Report: "Step 07 cannot start. Missing: design plan with construction specs. Run `/06-design-plan` first."
- Do NOT proceed.

**Pre-flight checks (beyond dependency clearance):**
1. **Token sync check** — Read design plan's Token Sync Log. Compare design doc modification date vs "Last Synced" date. If design doc is newer, warn: "Design tokens have changed since last Figma sync. Update Figma variables before constructing."
2. **Corrections log check** — Read `docs/Design/corrections-log.md` (if exists). Note active corrections and graduated rules to apply during construction.
3. **Component health check** — If continuing construction, verify source components in Figma are populated before instancing (lesson from correction C-002 pattern).
4. **MCP connectivity** — Test Figma MCP connection before starting a batch.

---

## Execute

### Phase 1: Determine Construction Scope

1. Read `docs/Design/figma-design-plan.md` — full plan
2. Read design plan execution checklists — what's already done?
3. If `$ARGUMENTS` specifies a batch or screen, scope to that
4. If continuing, pick up from the next unchecked batch
5. Read `docs/Design/{{PROJECT_NAME}}-design-document.md` — tokens and component specs
6. Read `.impeccable.md` — brand principles for design rationale

### Phase 2: Foundation Setup (first run only)

If Figma file doesn't exist yet (Figma IDs table is empty):

1. Delegate to `figma-designer` agent to:
   - Create Figma file
   - Set up variable collections (colors, spacing from design doc)
   - Create text styles (from design doc type scale)
   - Build base components (from design doc component specs)
   - Create pages for each batch
2. Update design plan with populated Figma IDs
3. Update Token Sync Log

### Phase 3: Screen Construction

For each batch/screen in scope, delegate to `figma-designer` agent:

> Construct [batch/screen] for {{PROJECT_NAME}} in Figma.
>
> Design plan: [inline frame tree specs for this batch]
> Design tokens: [from design document]
> Consistency rules: [from design plan]
> Active corrections: [from corrections log]
> Graduated rules: [from design plan]
> Component reference: [node IDs from design plan]
>
> Construction rules:
> - Batch 2-4 screens per MCP call (50KB JS capacity)
> - Use FILL sizing on children of auto-layout frames
> - Always use component instances (look up by node ID first)
> - Set padding on screen frame, not children
> - Return structured data (node IDs, names, dimensions) for each constructed element
> - Update design plan checklist on completion

### Context Management

The Figma agent reads many large files. To prevent context rot:
- Pass only the **current batch's frame tree specs** from the design plan, not the full 42KB+ file
- Pass only the **consistency rules** and **token sync log** sections
- Pass the **component instance reference table** (compact)
- Summarize the design document's token tables — don't include full component prose
- Include the corrections log in full (typically small)

### Phase 4: Post-Construction

1. Update design plan execution checklist (mark constructed screens)
2. Update design plan Figma IDs table with new frame references
3. If construction produced errors, apply **Error Recovery** below

### Error Recovery

If construction fails or produces incorrect output:
1. Identify the specific failure (MCP error, wrong component, layout broken)
2. Feed the error + current frame state back to the agent:
   > "Construction failed/incorrect: [specific error or screenshot evidence].
   > Current state: [node IDs and observed properties].
   > Please fix: [specific instruction]."
3. The agent retries with the error context (batch fixes into one MCP call)
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Phase 5: Human Review Gate

1. Update `docs/workflow-status.md` step 07 status to `needs-review`
2. Prompt: "Screens constructed. Please review in Figma and report any issues with `/correct`. Run `/07-figma [continue]` to approve this batch and proceed to the next."
3. Only `[continue]` after `needs-review` transitions to `completed` (or starts next batch)

### Phase 6: Report

```
Step 07 — Figma Creation: [status]

Foundation: [complete/incomplete]
Screens constructed: [count]/[total]
  Batch 1: [count]/[total] — [status]
  Batch 2: [count]/[total] — [status]
  ...

Pre-flight:
  Token sync: [OK / STALE — update needed]
  Active corrections: [count]
  MCP status: [connected / not tested]

Next actions:
  - Review constructed screens in Figma
  - Run /correct for any visual issues
  - Run /07-figma [continue] for next batch
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 07 section
2. Read design plan execution checklists — count completed vs total
3. Check token sync freshness
4. Check corrections log for active items
5. Report per-batch progress

---

## Force Status Update

1. Read design plan — count screens with "Last Verified" dates
2. Read corrections log — count active vs completed corrections
3. Check token sync log
4. Update `docs/workflow-status.md` step 07
