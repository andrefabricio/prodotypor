# Plan: Figma Design — {{PROJECT_NAME}} [Phase]

> Last updated: [date]
> PRD: `docs/prds/[prd-file].md`
> Design tokens: `docs/Design/{{PROJECT_NAME}}-design-document.md`

---

## Current Status

<!-- Update as foundation items are completed -->

- [ ] Figma file exists — `{{FIGMA_FILE_ID}}`
- [ ] Variable collections created (colors, spacing)
- [ ] Text styles created
- [ ] Base components built (see Component Inventory below)
- [ ] Design system kit reference pages created
- [ ] Screen construction — 0/N screens across M batches

---

## MCP Access

**Figma remote MCP server** — full read+write canvas access.

```json
"figma": {
  "type": "http",
  "url": "https://mcp.figma.com/mcp"
}
```

**Auth:** Browser-based OAuth, handled by the server.
**Reference:** `.claude/knowledge/figma-mcp-reference.md`

---

## Source Hierarchy

```
Design Document (authoritative tokens, component specs)
  └─ .impeccable.md (brand context, aesthetic direction)
       └─ Figma Variable Collections (derivative from design doc)
```

<!-- CUSTOMIZE: Update if your project has additional source layers (e.g., Google Doc, design system library) -->

---

## Figma IDs

> Populated as resources are created.

| Resource | ID | Notes |
|----------|-----|-------|
| File | `{{FIGMA_FILE_ID}}` | |
| Page: Batch 1 | — | |
| Page: Batch 2-N | — | |
| Page: Components | — | |
| Variable: Colors | — | |
| Variable: Spacing | — | |
| Text Styles | — | |

---

## Component Instance Reference

> Populated as components are built in Figma.

| Component | Node ID | Variants | Default Size | Usage |
|-----------|---------|----------|-------------|-------|
<!-- Example:
| Button/Primary | 10:5 | Primary, Secondary, Text | 358×48 | CTA, form submission |
| Avatar/36px | 46:11 | — | 36×36 | Feed cards, comments |
-->

---

## Screen Inventory

<!-- CUSTOMIZE: Derive from your PRD user stories. Every story maps to at least one screen. -->

### Batch 1 — [Flow Name] (Screens 1-N)

<!-- Example: an onboarding flow with 5 screens -->

| # | Screen | Type | PRD Source | Priority | Status |
|---|--------|------|-----------|----------|--------|
| 01 | Welcome / Splash | Full | — | P0 | Not started |
| 02 | Sign Up (Email) | Full | Story 1.1 | P0 | Not started |
| 03 | Log In | Full | Story 1.2 | P0 | Not started |
| 04 | Create [Group/Space] | Full | Story 2.1 | P0 | Not started |
| 05 | Invite Members | Full | Story 2.2 | P1 | Not started |

### Batch 2 — [Flow Name] (Screens N-M)

<!-- Example: a core content loop -->

| # | Screen | Type | PRD Source | Priority | Status |
|---|--------|------|-----------|----------|--------|
| 06 | Home Feed | Full | Story 3.1 | P0 | Not started |
| 07 | Content Detail | Full | Story 3.2 | P0 | Not started |
| 08 | Create Content | Full | Story 4.1 | P0 | Not started |
| 09 | Comments / Thread | Full | Story 3.3 | P1 | Not started |

### Batch 3 — [Flow Name] (Screens M-Z)

<!-- CUSTOMIZE: Add remaining screens — settings, profile, management flows -->

| # | Screen | Type | PRD Source | Priority | Status |
|---|--------|------|-----------|----------|--------|
| 10 | [Screen Name] | Full | Story X.Y | P1 | Not started |

---

## Consistency Rules

Rules that apply to ALL screens:

### Resolution & Frame

- **Frame size:** <!-- CUSTOMIZE: e.g., 390×844 (iPhone 14/15 @1x) -->
- **Auto layout** on all structural containers
- **Variables** for all colors, spacing, typography (never hardcode)
- **Semantic layer naming** matching target framework widgets

### Spacing Scale

| Gap Role | Token | Value | Usage |
|----------|-------|-------|-------|
| gap-xs | space-1 | 4px | Tight internal gaps |
| gap-sm | space-2 | 8px | Compact elements |
| gap-md | space-3 | 12px | Standard spacing |
| gap-lg | space-4 | 16px | Section spacing |
| gap-xl | space-6 | 24px | Major breaks |
| gap-xxl | space-8 | 32px | Screen-level |

<!-- CUSTOMIZE: Map your design document spacing tokens -->

### Line Height Rules

| Category | Line Height | Applies to |
|----------|------------|------------|
| Display / Headline | <!-- e.g., 1.10 --> | display-lg, display-md, headline-md |
| Body | <!-- e.g., 1.65 --> | body-lg, body-md, body-sm |
| Labels | <!-- e.g., 1.40 --> | label-lg, label-md |

### Correction-Graduated Rules

> Rules promoted from design corrections (populated during reviews).

| Rule | Source Correction | Screens Affected |
|------|------------------|-----------------|
<!-- Example:
| All avatar placements must use Avatar component instances with FIXED×FIXED sizing | C-002 | 9+ screens |
-->

---

## Batch Execution Checklists

### Batch 1 — [Flow Name]

- [ ] Write inline construction specs for screens 1-N
- [ ] Verify token sync (design doc version matches Figma variables)
- [ ] Construct screens via MCP (batch 2-4 per call)
- [ ] Human review against design document
- [ ] Log corrections via `/correct` skill
- [ ] Apply corrections to all affected screens
- [ ] Cross-screen consistency check
- [ ] Update "Last Verified" dates below

| Screen | Constructed | Reviewed | Last Verified |
|--------|------------|----------|---------------|
| Screen 01 | — | — | — |
| Screen 02 | — | — | — |
<!-- Continue for all screens in batch -->

### Batch 2 — [Flow Name]

<!-- Same structure -->

---

## Inline Construction Specs

<!-- Write frame tree specs for each screen before construction. Example format: -->

### Screen 01: [Name] — [width]×[height]

```
Screen 01: [Name] — 390×844
├─ StatusBar (component instance)
├─ AppBar (component: AppBar/Default)
│  ├─ Logo [display-md, fill: var(textPrimary)]
│  └─ [Actions area]
├─ ScrollContent (auto-layout: vertical, padding: var(space-4), gap: var(space-3))
│  ├─ [SectionHeading] (text style: headline-md, fill: var(textPrimary))
│  ├─ [ContentCard] (component: Card/[Variant])
│  │  ├─ [Card content...]
│  │  └─ [Card footer...]
│  └─ [Additional content...]
└─ BottomNav (component: BottomNav/Default, activeTab: [tab])
```

**States:** default, empty, loading, error
**Notes:** [Any construction-specific guidance]

<!-- CUSTOMIZE: Add specs for each screen before its batch is constructed -->

---

## Token Sync Log

| Collection | Token Count | Last Synced | Design Doc Version | Match |
|------------|-------------|-------------|-------------------|-------|
| Colors | — | — | — | — |
| Spacing | — | — | — | — |
| Typography | — | — | — | — |

> **Pre-construction check:** Before constructing any screen batch, verify that the "Last Synced" date is not older than the design document's last modification. If stale, update Figma variables first.
