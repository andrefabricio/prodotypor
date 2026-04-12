# Product Development Pipeline — Status

> Auto-maintained by pipeline skills. Do not edit manually.
> Last updated: —

## Pipeline Overview

| Step | Name | Status | Started | Completed | Output |
|------|------|--------|---------|-----------|--------|
| 00 | Project Setup | not-started | — | — | — |
| 01 | Backlog Creation | not-started | — | — | — |

> **Status values:** `not-started` → `in-progress` → `needs-review` → `completed` | `blocked (reason)`
> After producing output, each step transitions to `needs-review`. Run `/NN-name [continue]` to approve and transition to `completed`.
| 02 | Backlog Prioritization | not-started | — | — | — |
| 03 | PRD Creation | not-started | — | — | — |
| 04 | UI Inspiration Research | not-started | — | — | — |
| 05 | Design Document | not-started | — | — | — |
| 06 | Design Plan | not-started | — | — | — |
| 07 | Figma Creation | not-started | — | — | — |

## Step Details

### 00 — Project Setup
- **Status:** not-started
- **Output files:** CLAUDE.md, .impeccable.md, .mcp.json (configured)
- **Checklist:**
  - [ ] Phase 1: Project identity (name, description, phase, revenue model)
  - [ ] Phase 2: Brand & design (personality, users, visual direction, principles)
  - [ ] Phase 3: Tech stack (frontend, backend, other technologies)
  - [ ] Phase 4: MCP connections (Figma, GDrive — optional)
  - [ ] Phase 5: External skills (Impeccable — optional)
  - [ ] Phase 6: Validation (placeholder scan, content checks)
- **Notes:** Run `/00-setup` to start. This is the first step in the pipeline.

### 01 — Backlog Creation
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Read project context (CLAUDE.md, .impeccable.md)
  - [ ] Identify features and group into epics
  - [ ] Write user story titles for each feature
  - [ ] Include phase grouping (MVP, Phase 1, Phase 2+)
  - [ ] Review with user
- **Notes:** (none)

### 02 — Backlog Prioritization
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Apply P0-P3 priority framework to all features
  - [ ] Create prioritization matrix (user impact × business value × effort)
  - [ ] Lock MVP scope (P0 + critical P1)
  - [ ] Sketch phase roadmap
  - [ ] Review with user
- **Notes:** (none)

### 03 — PRD Creation
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Write problem statement and user pain points
  - [ ] Define success metrics (adoption, engagement, retention, quality)
  - [ ] Write user stories with Given/When/Then acceptance criteria
  - [ ] Define scope boundaries ("what we're NOT building")
  - [ ] Document dependencies and risks
  - [ ] Review with user
- **Notes:** (none)

### 04 — UI Inspiration Research
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Identify 2-3 reference apps (similar context, features, or design quality)
  - [ ] Complete design audit for app 1 (all 6 sections)
  - [ ] Complete design audit for app 2 (all 6 sections)
  - [ ] Cross-reference findings against project brand direction
  - [ ] Review with user
- **Notes:** Can run in parallel with step 03

### 05 — Design Document
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Define design philosophy and governing principles
  - [ ] Specify color system (tokens, hex values, usage rules, contrast ratios)
  - [ ] Specify typography (type scale, font families, line heights)
  - [ ] Define spacing scale
  - [ ] Spec all components (buttons, inputs, cards, nav, avatars — all states)
  - [ ] Plan dark mode mapping
  - [ ] Document accessibility requirements
  - [ ] Review with user
- **Notes:** Requires both step 03 and step 04 completed

### 06 — Design Plan
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Map all PRD user stories to screens
  - [ ] Organize screens into batches by user flow
  - [ ] Write consistency rules referencing design document tokens
  - [ ] Write inline frame tree specs for batch 1
  - [ ] Create token sync log
  - [ ] Create component instance reference table
  - [ ] Create execution checklists per batch
  - [ ] Review with user
- **Notes:** (none)

### 07 — Figma Creation
- **Status:** not-started
- **Output files:** (none)
- **Checklist:**
  - [ ] Create Figma file and pages
  - [ ] Set up variable collections (colors, spacing)
  - [ ] Create text styles
  - [ ] Build base components
  - [ ] Build design system kit (human review gate)
  - [ ] Construct batch 1 screens
  - [ ] Human review + corrections
  - [ ] Construct remaining batches
  - [ ] Final consistency check
  - [ ] Export design tokens for code handoff
- **Notes:** Token sync check required before each batch
