---
name: 01-backlog
description: "Step 1 of 8 — Product backlog creation. Creates a feature backlog with epic grouping and user story titles from project context. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [feature area or context]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /01-backlog — Product Backlog Creation

Create or continue building the product backlog with epics, features, and user story titles.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as context input (feature area, user need) and proceed to **Execute**

---

## Prerequisites

**Hard dependencies:**
1. Read `docs/workflow-status.md` — verify step 00 (Setup) status == `completed`
2. Read `CLAUDE.md` — verify project context has non-placeholder content

**If prerequisites fail:**
- If step 00 not completed: "Step 01 cannot start. Run `/00-setup` first to configure your project."
- Do NOT proceed.

**Validation:**
1. Read `.impeccable.md` — note brand personality (soft dependency, warn if missing)
2. Check if `docs/backlog/` directory exists (create if not)

---

## Execute

### Phase 1: Gather Context

1. Read `CLAUDE.md` for project context, tech stack, and product scope
2. Read `.impeccable.md` for brand personality and user personas
3. If `$ARGUMENTS` contains specific context, note it as the focus area
4. Check for existing backlog at `docs/backlog/{{PROJECT_NAME}}-backlog.md` — if present, this is a continuation

### Phase 2: Delegate to Product Manager Agent

Spawn the `product-manager` agent with this prompt structure:

> Create/update the product backlog for {{PROJECT_NAME}}.
> 
> Context: [project description from CLAUDE.md]
> User personas: [from .impeccable.md]
> Focus area: [from $ARGUMENTS if provided]
> Existing backlog: [content if continuing]
>
> Output requirements:
> - Group features into epics (use `templates/epic-template.md` format)
> - Write user story titles for each feature (As a [persona], I want...)
> - Include phase grouping (MVP, Phase 1, Phase 2+)
> - Note dependencies between features
> - Save to `docs/backlog/{{PROJECT_NAME}}-backlog.md`

### Phase 3: Validate Output

1. Verify `docs/backlog/{{PROJECT_NAME}}-backlog.md` exists and is non-empty
2. Check: has at least 1 epic grouping
3. Check: has user story titles (not just feature names)
4. If validation passes → proceed to Phase 4
5. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (which check failed, what's missing)
2. Feed the error + partial output back to the agent:
   > "The backlog did not meet requirements: [specific failure].
   > Current output: [summary of what exists].
   > Please fix: [specific instruction — e.g., 'add epic grouping' or 'convert feature names to user story titles']."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`
5. Report the failure to the user with actionable guidance

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 01 status to `needs-review`
2. Report output summary to the user
3. Prompt: "Review the backlog output. Run `/01-backlog [continue]` to approve and mark completed, or provide feedback for revisions."
4. Only when the user runs `[continue]` after `needs-review`, transition status to `completed`

### Phase 5: Report

```
Step 01 — Backlog Creation: [status]

Output: docs/backlog/{{PROJECT_NAME}}-backlog.md
Epics: [count]
Features: [count]
User stories: [count]

Next step: /02-prioritize
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 01 section
2. If backlog file exists, report: epic count, feature count, story count
3. Report current status and any blockers

---

## Force Status Update

1. Re-scan `docs/backlog/{{PROJECT_NAME}}-backlog.md`
2. Count epics, features, and stories
3. Update `docs/workflow-status.md` step 01 with current state
4. Report updated status
