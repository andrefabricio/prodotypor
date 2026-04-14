# {{PROJECT_NAME}} — Agent Instructions

> **This file must stay under 300 lines.** For detailed specs, link to `docs/`.

## Project Context

<!-- CUSTOMIZE: Describe your project — what it does, who it's for, and the current phase -->

- **Phase:** <!-- e.g., Phase 0 MVP (target date, team size) -->
- **Revenue model:** <!-- e.g., D2C subscription, freemium, enterprise -->
- **Development approach:** AI-assisted with Claude Code
- **Detailed scope:** <!-- CUSTOMIZE: link to your scope document if you have one -->

## Tech Stack (Source of Truth)

<!-- CUSTOMIZE: Replace with your project's technology stack -->

| Layer | Technology | Notes |
|-------|-----------|-------|
| Frontend | <!-- e.g., Pure Flutter (Dart 3.0+) --> | <!-- e.g., flutter_bloc, go_router, Hive --> |
| Backend | <!-- e.g., Supabase (PostgreSQL 15+) --> | <!-- e.g., PostgREST, Realtime, Edge Functions --> |
| Auth | <!-- e.g., Supabase Auth (GoTrue) --> | <!-- e.g., Email/password + OAuth --> |
| Storage | <!-- e.g., Supabase Storage --> | <!-- e.g., S3-compatible CDN --> |
| Payments | <!-- e.g., Stripe --> | <!-- e.g., Subscriptions, webhooks --> |
| Analytics | <!-- e.g., PostHog --> | <!-- e.g., Privacy-focused, feature flags --> |
| CI/CD | <!-- e.g., GitHub Actions --> | <!-- e.g., Automated testing + builds --> |

---

## Product Development Pipeline

This project uses an 8-step AI-assisted product development workflow. Each step has a dedicated slash command with sub-commands for execution control.

### Pipeline Steps

| Step | Command | Agent | Output |
|------|---------|-------|--------|
| 00 | `/00-setup` | (interactive wizard) | Configured project files — CLAUDE.md, .impeccable.md, MCP |
| 01 | `/01-backlog` | product-manager | `docs/backlog/` — feature backlog with user stories |
| 02 | `/02-prioritize` | product-manager | `docs/backlog/` — prioritized backlog with P0-P3 matrix |
| 03 | `/03-prd` | product-manager | `docs/prds/` — PRDs and user stories per feature |
| 04 | `/04-inspiration` | inspiration-researcher | `docs/Design/Inspirations/` — structured design audits |
| 05 | `/05-design-doc` | design-doc-author | `docs/Design/` — comprehensive design specification |
| 06 | `/06-design-plan` | design-planner | `docs/Design/figma-design-plan.md` — Figma construction plan |
| 07 | `/07-figma` | figma-designer | Figma canvas — constructed screens via MCP |

### Sub-commands

Every pipeline command supports these sub-commands via arguments:

| Sub-command | Behavior |
|-------------|----------|
| *(empty)* | Initiate the step or continue from last checkpoint |
| `[continue]` | Explicitly resume from last checkpoint |
| `[status]` | Report current progress without executing |
| `[update]` | Re-scan outputs and force status refresh |

Example: `/03-prd [status]` reports PRD creation progress without doing work.

### Dependency Graph

```
00 Setup → 01 Backlog → 02 Prioritize → 03 PRD ──┬──→ 05 Design Doc → 06 Design Plan → 07 Figma
                                                   │         ▲
                                    04 Inspiration ┘         │
                                   (parallel to 03)    .impeccable.md
```

Steps 03 and 04 can run in parallel. Step 05 requires both to be completed.

### Status Tracking

Pipeline status is tracked in `docs/workflow-status.md`. Each step updates this file on entry and completion. Steps enforce prerequisites — a step will not proceed unless its hard dependencies are met.

Status lifecycle: `not-started` → `in-progress` → `needs-review` → `completed` (or `blocked`)

After producing output, each step sets status to `needs-review` and waits for the user to approve via `[continue]` before marking `completed`. This ensures human review at every pipeline gate.

### Context Management

Later pipeline steps (05-07) aggregate context from multiple large files. To prevent context degradation:
- Pass **summaries** of prior step outputs to agents, not full files
- Include only **relevant sections** from large files (e.g., token tables from a 50KB design doc)
- For the design plan, pass only the current batch's specs + consistency rules

---

## Product Management Rules

### Role

When working on product documentation: create PRDs, user stories, feature analysis, and prioritization matrices. Do not generate code, file structures, or scaffolding.

### User Story Format

```
As a [persona], I want [functionality], so that [value].

Acceptance Criteria:
- Given [context] / When [action] / Then [outcome]
```

### Prioritization

| Level | Meaning |
|-------|---------|
| P0 | Critical — blocks core functionality |
| P1 | High — significant user impact |
| P2 | Medium — improves experience |
| P3 | Low — nice to have |

### Templates & Examples

- PRD: `templates/prd-template.md`
- User Story: `templates/user-story-template.md`
- Epic: `templates/epic-template.md`
- Design System Kit: `templates/design-system-template.md`
- Inspiration Research: `templates/inspiration-research-template.md`
- Design Document: `templates/design-document-template.md`
- Design Plan: `templates/design-plan-template.md`

---

## Architecture Principles

<!-- CUSTOMIZE: Adapt to your project's architecture philosophy -->

1. **Separation of Concerns** — UI, business logic, and data access in distinct layers.
2. **Design for replaceability** — every integration point behind an interface or adapter.
3. **Fail loudly in dev, fail gracefully in prod** — differentiated error handling by environment.
4. **State is the enemy** — minimise shared mutable state; prefer reactive patterns.
5. **Vertical slices over horizontal layers** — organise code by feature, not by type.
6. **Device targets are configuration, not constants** — screen dimensions are defined in `.devices.md` at project root. All agents, design plans, and workflows must read this file before any Figma construction. Never hardcode device dimensions in agent instructions or design docs.

---

## Diagrams

All architecture diagrams must use **Mermaid syntax**. Produce diagrams proactively when discussing system boundaries, data flows, or state machines.

| Situation | Diagram Type |
|-----------|-------------|
| System architecture, component map | `flowchart TD` |
| API/webhook event sequence | `sequenceDiagram` |
| State transitions | `stateDiagram-v2` |
| Database schema relationships | `erDiagram` |
| User journey | `journey` |

Syntax reference: `.claude/knowledge/mermaid-flowchart-syntax-reference.md`

---

## Code Review Checklist

```
SECURITY
  [ ] No secrets, tokens, or credentials in code or comments
  [ ] All user input validated and sanitised
  [ ] Auth checks enforced at data layer, not just UI

PERFORMANCE
  [ ] N+1 queries eliminated
  [ ] Index confirmed for every new query pattern
  [ ] No synchronous blocking in async contexts

RELIABILITY
  [ ] All async paths have error handling
  [ ] Idempotency on all mutation operations
  [ ] Retry with backoff on external calls

MAINTAINABILITY
  [ ] Single responsibility per function
  [ ] Magic numbers/strings extracted to constants
  [ ] Complex logic has comments explaining *why*
  [ ] Tests exist or test plan defined for new business logic
```

---

## Design Quality Skills (Impeccable)

Impeccable design skills are installed at `.claude/skills/` and provide AI-assisted design quality commands. Design context is defined in `.impeccable.md` — this file is the single source of truth for visual identity and must be consulted before any UI work.

### When to Use

| Workflow Stage | Skills | Trigger |
|----------------|--------|---------|
| **PRD → UI Spec** | `/critique` | Validate UX before code |
| **Building UI** | `/frontend-design` | Design principles and anti-patterns |
| **Component extraction** | `/extract` | Consolidate repeated UI patterns |
| **Typography & layout** | `/typeset`, `/arrange` | Type scales, spacing, layouts |
| **Color system** | `/colorize` | Color token system |
| **Design review** | `/correct` | Log, propagate, and apply corrections |
| **Pre-ship review** | `/audit` | Technical quality audit (P0-P3) |
| **Final polish** | `/polish` | Last pass before merge |
| **UX evaluation** | `/critique` | Nielsen's heuristics scoring |

### Rules

1. **`.impeccable.md` is pre-configured.** Skills will find design context there automatically — do not run `/teach-impeccable` unless updating the design direction.
2. **Adapt design output to your framework.** All design output must translate to your target framework's components.
3. **`/audit` and `/critique` are read-only.** They produce reports, not code changes.

---

## Google Drive MCP Rules

The GDrive MCP is configured for **read-only consultation** of source-of-truth documents. These rules are non-negotiable:

### Allowed Documents (Allowlist)

<!-- CUSTOMIZE: Replace with your project's allowed Google Doc IDs -->

| Document | Google Doc ID |
|----------|--------------|
| <!-- Document name --> | `{{GDOC_ID_design_doc}}` |
| <!-- Document name --> | `{{GDOC_ID_backlog}}` |

### Access Rules

1. **Read-only by default.** Never create, edit, update, or delete any Google Drive content without explicit user approval.
2. **No browsing.** Do not search, list, or access any documents outside the allowlist.
3. **Citation.** When referencing information from these docs, state which document the information came from.

---

## Reference Docs

<!-- CUSTOMIZE: Update paths to your project's documentation -->

| Document | Path |
|----------|------|
| Project scope | <!-- CUSTOMIZE: your scope document path --> |
| Pipeline status | `docs/workflow-status.md` |
| Design plan | `docs/Design/figma-design-plan.md` |
| Problem-solving framework | `.claude/knowledge/problem-solving-101-framework.md` |

### On-Demand Commands

| Trigger | Action |
|---------|--------|
| "pipeline status" | Read `docs/workflow-status.md`, report all 8 steps |
| "design status" / "figma status" | Read design plan, report Figma construction status |
