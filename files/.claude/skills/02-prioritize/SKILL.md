---
name: 02-prioritize
description: "Step 2 of 8 — Backlog prioritization. Applies P0-P3 priority framework and locks MVP scope. Sub-commands: [continue] resume, [status] check progress, [update] force status refresh."
user-invocable: true
argument-hint: "[continue|status|update] [prioritization criteria or constraints]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, Agent]
---

# /02-prioritize — Backlog Prioritization

Prioritize the product backlog using the P0-P3 framework and lock MVP scope.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report** section
- **Contains `[update]`** → Jump to **Force Status Update** section
- **Contains `[continue]`** or **empty** → Jump to **Execute** section
- **Other text** → Treat as prioritization criteria/constraints and proceed to **Execute**

---

## Prerequisites

**Hard dependencies:**
1. Read `docs/workflow-status.md` — verify step 01 status == `completed`
2. Verify `docs/backlog/{{PROJECT_NAME}}-backlog.md` exists with ≥1 epic

**If prerequisites fail:**
- Report: "Step 02 cannot start. Missing: completed backlog from step 01."
- Suggest: "Run `/01-backlog` first to create the product backlog."
- Do NOT proceed.

**Soft dependencies:**
- Business objectives, timeline constraints (warn if not provided)

---

## Execute

### Phase 1: Read Inputs

1. Read `CLAUDE.md` — extract prioritization framework (P0-P3 definitions)
2. Read `docs/backlog/{{PROJECT_NAME}}-backlog.md` — full backlog
3. Read `.claude/knowledge/problem-solving-101-framework.md` — Tool 6: Criteria & Evaluation Table
4. Note any constraints from `$ARGUMENTS`

### Phase 2: Delegate to Product Manager Agent

Spawn the `product-manager` agent:

> Prioritize the product backlog for {{PROJECT_NAME}}.
>
> Backlog: [content of backlog file]
> Prioritization framework: P0 (Critical), P1 (High), P2 (Medium), P3 (Nice to have)
> Additional constraints: [from $ARGUMENTS if provided]
>
> Use Tool 6 (Criteria & Evaluation Table) from the problem-solving framework:
> - Criteria: User impact, Business value, Technical effort, Dependencies
> - Score each feature across criteria
> - Assign P0-P3 based on composite score
>
> Output requirements:
> - Every feature gets a P0/P1/P2/P3 priority
> - MVP scope explicitly listed (P0 + critical P1 items)
> - Phase roadmap: MVP → Phase 1 → Phase 2+
> - Dependency order within MVP
> - Update `docs/backlog/{{PROJECT_NAME}}-backlog.md` with priorities

### Pipeline Context

Before delegating to the agent, read and summarize prior step outputs:
- Step 01 output: read first 20 lines of `docs/backlog/{{PROJECT_NAME}}-backlog.md` for epic/feature overview
- Include this summary in the agent prompt as "Pipeline Context" so decisions carry forward

### Phase 3: Validate Output

1. Verify backlog file updated with priority assignments
2. Check: every feature has P0/P1/P2/P3 assigned
3. Check: MVP scope section exists and is explicitly bounded
4. If validation passes → proceed to Phase 4
5. If validation fails → proceed to **Error Recovery**

### Error Recovery

If validation fails:
1. Identify the specific failure (which check failed, what's missing)
2. Feed the error + partial output back to the agent:
   > "The prioritization did not meet requirements: [specific failure].
   > Current output: [summary].
   > Please fix: [specific instruction]."
3. Re-validate after the retry
4. If retry also fails, mark step as `blocked (reason)` in `docs/workflow-status.md`

### Phase 4: Human Review Gate

1. Update `docs/workflow-status.md` step 02 status to `needs-review`
2. Report priority distribution and MVP scope to the user
3. Prompt: "Review the prioritization. Run `/02-prioritize [continue]` to approve, or provide feedback."
4. Only `[continue]` after `needs-review` transitions to `completed`

### Phase 5: Report

```
Step 02 — Backlog Prioritization: [status]

Priority distribution:
  P0 (Critical): [count] features
  P1 (High):     [count] features
  P2 (Medium):   [count] features
  P3 (Low):      [count] features

MVP scope: [list of P0 features]

Next step: /03-prd
```

---

## Status Report

1. Read `docs/workflow-status.md` — extract step 02 section
2. If backlog has priorities, report distribution (P0/P1/P2/P3 counts)
3. Report MVP scope summary

---

## Force Status Update

1. Re-scan backlog file for priority assignments
2. Count features per priority tier
3. Check if MVP scope section exists
4. Update `docs/workflow-status.md` step 02
