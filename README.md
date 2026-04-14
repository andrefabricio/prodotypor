<p align="center">
  <img src="assets/prodotypor-logo-square.webp" alt="Prodotypor" width="200">
</p>

<h1 align="center">Prodotypor</h1>

<p align="center">
  An AI-assisted product development pipeline for Claude Code.<br>
  Go from a product idea to a clickable Figma prototype in 9 structured steps,<br>
  with specialized AI agents handling product management, design research, and visual construction.
</p>

---

## Why

Building a product from scratch requires a product manager, a designer, and weeks of documentation before a single pixel is placed. If you're a solo founder, a small team, or just exploring an idea — you don't have that.

Prodotypor gives you an opinionated workflow where AI agents do the heavy lifting: creating backlogs, writing PRDs, researching design patterns, specifying your design system, planning screens, and constructing them in Figma. You provide the idea, the brand direction, and the approvals.

## How It Works

Copy the template into your project, run `/00-setup` to answer a few questions about your product, then walk through the pipeline step by step.

```
 YOU                                    AI AGENTS
  |                                         |
  |  "I'm building a fitness app            |
  |   for busy parents..."                  |
  |                                         |
  | ───── /00-setup ──────────────────────> |  Configures project files
  | ───── /01-backlog ────────────────────> |  Creates feature backlog with epics
  | ───── /02-prioritize ─────────────────> |  Applies P0-P3 framework, locks MVP
  | ───── /03-prd ────────────────────────> |  Writes PRDs with user stories
  | ───── /04-inspiration ────────────────> |  Audits 2+ competitor app designs
  | ───── /05-design-doc ─────────────────> |  Specifies colors, typography, components
  | ───── /06-design-plan ────────────────> |  Plans every screen with construction specs
  | ───── /07-figma ──────────────────────> |  Builds screens in Figma via MCP
  | ───── /prototype ─────────────────────> |  Wires clickable prototype + flow diagram
  |                                         |
  |  <── You review & approve at every step |
```

Every step produces real deliverables — markdown docs, design specs, Figma frames — that feed into the next. You review and approve at every gate before moving on.

## Workflow Diagram

```
                                ┌──────────────────────────────────────────┐
                                │            PRODOTYPOR PIPELINE            │
                                └──────────────────────────────────────────┘

  ┌────────────┐     ┌────────────┐     ┌────────────┐     ┌────────────────┐
  │  00 SETUP  │────>│ 01 BACKLOG │────>│02 PRIORITIZE│───>│    03 PRD      │──┐
  │            │     │            │     │            │     │                │  │
  │  Configure │     │  Feature   │     │  P0-P3     │     │  User stories  │  │
  │  project   │     │  inventory │     │  MVP scope │     │  Acceptance    │  │
  │  files     │     │  Epics     │     │  Roadmap   │     │  criteria      │  │
  └────────────┘     └────────────┘     └────────────┘     └────────────────┘  │
                                                                               │
                                                            ┌──────────────┐   │
                                                            │04 INSPIRATION│   │
                                                            │              │   │
                                                            │ Competitor   │───┤
                                                            │ design audits│   │
                                                            │ (parallel)   │   │
                                                            └──────────────┘   │
                                                                               │
  ┌────────────┐     ┌────────────┐     ┌────────────────┐                     │
  │  07 FIGMA  │<────│06 DESIGN   │<────│ 05 DESIGN DOC  │<────────────────────┘
  │            │     │   PLAN     │     │                │
  │  Construct │     │  Screen    │     │  Colors, type  │
  │  screens   │     │  inventory │     │  spacing,      │
  │  via MCP   │     │  Frame tree│     │  components    │
  │            │     │  specs     │     │                │
  └────────────┘     └────────────┘     └────────────────┘

  ─────────────────────────────────────────────────────────────────────────
  Human review gate at every step: needs-review → [continue] → completed
```

### Step Details

| Step | Command | What It Does | Output |
|------|---------|-------------|--------|
| 00 | `/00-setup` | Interactive wizard: project name, brand, tech stack, MCP config | Configured project files |
| 01 | `/01-backlog` | AI product manager creates feature backlog with epics | `docs/backlog/` |
| 02 | `/02-prioritize` | Applies P0-P3 priority framework, locks MVP scope | Prioritized backlog |
| 03 | `/03-prd` | Writes PRDs with user stories, acceptance criteria, metrics | `docs/prds/*.md` |
| 04 | `/04-inspiration` | Researches 2+ competitor apps with full design audits | `docs/Design/Inspirations/*.md` |
| 05 | `/05-design-doc` | Synthesizes research into a design specification | `docs/Design/*-design-document.md` |
| 06 | `/06-design-plan` | Creates Figma construction plan with screen-by-screen specs | `docs/Design/figma-design-plan.md` |
| 07 | `/07-figma` | Constructs screens in Figma using the plan specs via MCP | Figma canvas |
| -- | `/prototype` | Generates prototype wiring guide + Mermaid flow diagram, wires interactions | `docs/Design/prototype-wiring-guide.md` |

Steps 03 and 04 can run in parallel. All other steps are sequential — each checks its prerequisites before proceeding.

## What You Need

**Required:**
- Claude Code installed
- A product idea (who it's for, what it does)
- Three words for your brand personality (e.g., "Warm, Minimal, Trustworthy")

**Optional (for later steps):**
- Figma account with Professional plan (step 07 — screen construction)
- Google Cloud project with OAuth credentials (for Google Drive MCP)
- Node.js (for Google Drive MCP server)

Steps 00-06 work with nothing but Claude Code.

## Getting Started

### 1. Copy the template

```bash
cp -r /path/to/prodotypor/files/ .
```

### 2. Run the setup wizard

```
/00-setup
```

It asks about your project, brand, and tech stack one question at a time, then writes your answers into the right files. Approve with `/00-setup [continue]`.

### 3. Start building

```
/01-backlog
```

Each step produces output, asks for your review, and waits for `/NN-name [continue]` before marking itself complete.

### 4. Check progress

```
/03-prd [status]      # Status of one step
pipeline status       # Status of all 8 steps
```

### Manual setup (alternative)

If you prefer editing files by hand:
1. Replace `{{PROJECT_NAME}}` across all `.md` and `.json` files
2. Fill in `CLAUDE.md` (Project Context, Tech Stack)
3. Fill in `.impeccable.md` (Users, Brand, Aesthetic Direction, Design Principles)
4. Search `<!-- CUSTOMIZE` for remaining placeholder sections

## How Steps Work

Every command follows the same pattern:

1. **Check prerequisites** — is the previous step done?
2. **Delegate to an AI agent** — specialized for this step's task
3. **Validate output** — structural checks on deliverables
4. **Ask for your review** — status becomes `needs-review`
5. **You approve or revise** — `/NN-name [continue]` to proceed

### Sub-commands

| Command | Effect |
|---------|--------|
| `/03-prd` | Start or resume |
| `/03-prd [continue]` | Approve output, mark completed |
| `/03-prd [status]` | Check progress without executing |
| `/03-prd [update]` | Re-scan output files, refresh status |

## What's in the Box

| Component | Count | Details |
|-----------|-------|---------|
| Agents | 7 | architect, product-manager, copy-reviewer, figma-designer, inspiration-researcher, design-doc-author, design-planner |
| Pipeline Skills | 8 | `/00-setup` through `/07-figma` |
| Post-Pipeline Skills | 1 | `/prototype` (prototype wiring + flow diagrams) |
| Utility Skills | 1 | `/correct` (design corrections) |
| Knowledge Files | 3 | Figma MCP reference, Mermaid syntax, Problem-solving framework |
| Templates | 7 | PRD, user story, epic, design system, inspiration research, design document, design plan |
| MCP Servers | 2 | Figma (screen construction), Google Drive (doc access) |

## Optional Integrations

### Figma MCP (step 07)

Pre-configured in `.mcp.json`. Authenticates via browser OAuth on first use. Requires Professional plan ($15/mo) for 200 MCP calls/day.

### Google Drive MCP

For AI agents to read Google Docs as source-of-truth references. Requires a Google Cloud project with OAuth credentials. See `/00-setup` Phase 4 or configure `.mcp.json` manually.

### Impeccable Design Skills

21 skills from [pbakaus/impeccable](https://github.com/pbakaus/impeccable) that add `/critique`, `/audit`, `/polish`, `/colorize`, and more:

```bash
npx skills install pbakaus/impeccable
```

Or let `/00-setup` Phase 5 handle it.

## Troubleshooting

**"Step 01 cannot start. Run /00-setup first."**
Run `/00-setup` before starting the pipeline.

**Some `{{PLACEHOLDER}}` values remain after setup**
Run `/00-setup [update]` to scan. Optional placeholders (GDrive doc IDs, Figma file ID) are safe to leave until you need those services.

**Can I run steps out of order?**
Steps 03 and 04 can run in parallel. Everything else is sequential. Each step tells you what to run first if you skip ahead.

**Figma MCP won't connect**
Run `mcp__figma__whoami` to test. If it fails, try `/mcp` in Claude Code to manage servers. Ensure browser pop-ups aren't blocked for OAuth.

**I don't have Figma / Node.js / Google Drive**
Steps 00-06 need only Claude Code. Figma is step 07 only. Node.js and Google Drive are optional.

**A step is stuck in "needs-review"**
It's waiting for your approval. Review the output, then run `/NN-name [continue]`.

**I want to re-run setup**
Run `/00-setup` again — it resumes from the last completed phase, won't overwrite existing config. Edit files directly to change values, then `/00-setup [update]` to verify.

---

## What This Is Not

**Not a no-code tool.** You need Claude Code and terminal comfort. This is for builders who think in commands, not drag-and-drop.

**Not a replacement for a real designer.** The AI agents produce structured design specifications and Figma frames, but they don't have taste. You provide the creative direction; they execute the structure. Human review at every gate is not optional — it's the design.

**Not a production-ready app generator.** The pipeline produces product documentation and UI designs. It doesn't write application code, set up databases, or deploy anything. The architect agent can help with technical planning, but code generation is outside this pipeline's scope.

**Not framework-agnostic yet.** The current design pipeline (steps 05-07) is built around mobile app patterns — device dimensions from `.devices.md`, component-based UI, Figma as the design tool. Web-first teams (React, Tailwind, design-in-code) would need to adapt the design agents and templates.

**Not tested at scale.** Steps 01-03 and 07 are derived from a real production project. Steps 04-06 (inspiration research, design document, design plan) were created for this template and have limited real-world validation. Expect to iterate on agent output quality.

## Known Limitations

- **Claude Code only** — doesn't work with Cursor, GitHub Copilot, or other AI coding tools
- **Figma MCP is in beta** — write operations may change; pricing will shift from free to usage-based
- **No eval framework** — pipeline quality depends on model performance; no automated quality scoring yet
- **Single-project scope** — no multi-project dashboards, team collaboration, or shared template registries
- **No CI/CD integration** — pipeline status lives in a markdown file, not in your build system

## Backlog

Improvements under consideration for future versions:

### Platform & Framework
- [ ] Web-first variant (React/Next.js, design-in-code instead of Figma)
- [ ] Desktop app variant (Electron/Tauri frame sizes and patterns)
- [ ] Framework-agnostic design agents (decouple from mobile-specific assumptions)

### Pipeline Depth
- [ ] Step 08: Flutter/React Native code generation from Figma frames
- [ ] Step 09: Supabase/Firebase schema generation from PRD data models
- [ ] Automated design-to-code token extraction (Figma variables to theme files)
- [ ] Run complete example project end-to-end and publish outputs as reference

### Quality & Validation
- [ ] Eval framework for agent output quality (LLM-as-judge scoring)
- [ ] Golden-output test suite (expected output for a reference project)
- [ ] Automated WCAG contrast ratio checks in design doc validation
- [ ] CI integration for pipeline status tracking

### Collaboration
- [ ] Multi-user support (different team members own different steps)
- [ ] Integration with Linear/Jira (sync backlog and PRD items)
- [ ] Integration with Notion (publish pipeline outputs as pages)
- [ ] Shared template registry (publish/discover prodotypor variants)

### Developer Experience
- [ ] One-command install (`npx create-prodotypor my-project`)
- [ ] Interactive pipeline dashboard (web UI for status visualization)
- [ ] Step-level undo (revert a step's output and re-run)
- [ ] Partial pipeline runs (install only PM steps 01-03 or only design steps 05-07)

## Changelog

### v1.1.0 (2026-04-15)

Propagated improvements from production use (Candle-Product, April 10-14):

**New components:**
- `/prototype` skill — generates prototype wiring guides with interaction mapping, Mermaid flow diagrams, and dead-end detection
- `.devices.md` — device configuration template (single source of truth for screen dimensions, replaces hardcoded 390x844)

**Safety rules (figma-designer agent, +130 lines):**
- Rule 9: Orphan Prevention — `createdNodes[]` tracking + auto-cleanup in catch block
- Rule 10: Post-Construction Page Audit — auto-detects and removes orphaned nodes
- Rule 11: Component Health Gate — `safeInstance()` prevents instancing empty-shell components
- Rule 12: Overlay Screen Construction — `CLONE` keyword ensures overlay backgrounds are populated
- Scope-Check Gate — requires demo path declaration before any construction

**Knowledge patterns (figma-mcp-reference, +120 lines):**
- Patterns I-L: copy-pasteable JavaScript for orphan prevention, page audit, component health, overlay cloning
- Fixed multi-page anti-pattern: replaced "One Page per Batch" (which breaks prototype interactions) with single-page architecture guidance

**Architecture:**
- Principle #6: Device targets are configuration, not constants (`.devices.md`)
- All hardcoded 390x844 references replaced with `.devices.md` lookups

### v1.0.0 (2026-04-12)

Initial release. 8-step pipeline, 7 agents, 9 skills, 3 knowledge files, 7 templates.

---

Built with [Claude Code](https://claude.ai/code) and [templadotor](https://github.com/templadotor).
