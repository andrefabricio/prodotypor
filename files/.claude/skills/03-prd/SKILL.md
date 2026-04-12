---
name: 03-prd
description: "Step 3 of 8 — PRD creation. Produces Product Requirements Documents with user stories, acceptance criteria, and success metrics. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [feature name or epic to write PRD for]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /03-prd — PRD Creation

Create Product Requirements Documents from the prioritized backlog.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as specific feature/epic name to write PRD for

---

## Prerequisites

**Hard dependencies:**
1. Read `docs/workflow-status.md` — verify step 02 status == `completed`
2. Verify `docs/backlog/{{PROJECT_NAME}}-backlog.md` has MVP scope locked (P0-P3 assigned)

**If prerequisites fail:**
- Report: "Step 03 cannot start. Missing: prioritized backlog with MVP scope."
- Suggest: "Run `/02-prioritize` first."
- Do NOT proceed.

**Soft dependencies:**
- `.impeccable.md` brand direction (improves story quality but not blocking)

---

## Execute

### Phase 1: Determine Scope

1. Read prioritized backlog — identify MVP features (P0 + critical P1)
2. If `$ARGUMENTS` names a specific feature/epic, scope to that
3. If continuing, read existing PRDs in `docs/prds/` to determine what's already written
4. List features that still need PRDs

### Phase 2: Delegate to Product Manager Agent

For each feature/epic needing a PRD, spawn the `product-manager` agent:

> Write a PRD for [feature name] in {{PROJECT_NAME}}.
>
> Backlog entry: [relevant section from backlog]
> Template: [content of `templates/prd-template.md`]
> User story template: [content of `templates/user-story-template.md`]
> Brand context: [from `.impeccable.md` if available]
>
> Output requirements:
> - Problem statement with user pain points
> - ≥5 user stories in Given/When/Then format
> - Acceptance criteria for each story
> - Success metrics (adoption, engagement, retention, quality)
> - Scope boundaries ("what we're NOT building")
> - Dependencies and risks
> - Save to `docs/prds/[feature-slug].md`

### Pipeline Context

Before delegating, read and summarize prior step outputs:
- Step 01-02 output: read MVP scope and priority assignments from backlog
- Include this summary in the agent prompt as "Pipeline Context"

### Phase 3: Validate Output

1. Verify at least 1 PRD file exists in `docs/prds/`
2. Check each PRD has: problem statement, ≥5 user stories, acceptance criteria, success metrics
3. If validation passes → proceed to Phase 4
4. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (e.g., "only 3 user stories, need ≥5")
2. Feed the error + partial output back to the agent:
   > "The PRD did not meet requirements: [specific failure].
   > Current output: [summary].
   > Please fix: [specific instruction]."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Quality Gate (optional)

<!-- CUSTOMIZE: Define domain-specific quality checks for PRD output -->

After structural validation, optionally assess quality:
- Read the PRD and check: Are user stories specific enough to derive screens? Do acceptance criteria have measurable outcomes?
- Score: pass / needs-revision
- If needs-revision, feed specific feedback to the agent (see Error Recovery)

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 03 status to `needs-review`
2. Report PRD summary to the user
3. Prompt: "Review the PRD(s). Run `/03-prd [continue]` to approve, or provide feedback."
4. Only `[continue]` after `needs-review` transitions to `completed`

### Phase 5: Report

```
Step 03 — PRD Creation: [status]

PRDs written: [count]
  - [feature-name]: [story count] user stories
  - [feature-name]: [story count] user stories

Total user stories: [count]

Next steps:
  /04-inspiration — UI inspiration research (can run in parallel)
  /05-design-doc — after inspiration research completes
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 03 section
2. List PRD files in `docs/prds/` with story counts
3. Report completion percentage (PRDs written vs MVP features)

---

## Force Status Update

1. Glob `docs/prds/*.md` — count PRDs
2. Read each, count user stories
3. Compare against MVP feature list from backlog
4. Update `docs/workflow-status.md` step 03
