---
name: 00-setup
description: "Step 0 of 8 — Project setup wizard. Walks you through configuring your project name, brand, tech stack, and MCP connections. Run this first before any other pipeline step. Sub-commands: [continue] approve setup, [status] check progress, [update] re-scan for remaining placeholders."
user-invocable: true
argument-hint: "[continue|status|update]"
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit]
---

# /00-setup — Project Setup Wizard

Interactive setup that configures your project step by step. Run this once before starting the pipeline.

## Sub-command Router

Parse `$ARGUMENTS`:

- **Contains `[status]`** → Jump to **Status Report**
- **Contains `[update]`** → Jump to **Force Status Update**
- **Contains `[continue]`** → If status is `needs-review`, mark `completed`. If `in-progress`, resume from last completed phase.
- **Empty** → Start from Phase 1 (or resume from last completed phase)

---

## Prerequisites

None — this is the pipeline entry point.

Check current state:
1. Read `docs/workflow-status.md` — check step 00 status:
   - If `completed`: report "Setup already completed. Your project is configured. Run `/01-backlog` to start the pipeline, or `/00-setup [update]` to re-check for remaining placeholders."
   - If `needs-review`: report "Setup produced output but awaits your approval. Review CLAUDE.md and .impeccable.md, then run `/00-setup [continue]` to approve."
   - If `in-progress`: resume from the last completed phase (check which phase checklists are checked)
   - If `not-started`: proceed to Phase 1

**Important:** Running `/00-setup` on an already-completed setup will NOT overwrite your configuration. It resumes from the last completed phase, which means it will jump to validation (Phase 6) and report current state. To change values, edit the files directly and run `/00-setup [update]`.

---

## Phase 1: Project Identity

Ask the user these questions **one at a time**, waiting for each answer before proceeding:

1. **"What's your project name?"**
   - This will be used everywhere — file names, headings, agent references
   - Example: "Bloom", "HavenApp", "Fieldwork"

2. **"Describe your project in 1-2 sentences — what does it do and who is it for?"**
   - Example: "A private caregiving support platform for families navigating end-of-life journeys."

3. **"What phase are you in?"**
   - Example: "MVP", "Phase 0 — prototype", "Early exploration"

4. **"Revenue model?"**
   - Example: "D2C subscription", "Freemium", "Enterprise SaaS", "None yet"

### Actions after Phase 1:

1. **Replace `{{PROJECT_NAME}}`** across all project files using the Edit tool on each file that contains the placeholder. Use Grep to find all files:
   ```
   Grep pattern="{{PROJECT_NAME}}" → list of files
   For each file: Edit old_string="{{PROJECT_NAME}}" new_string="<user's answer>" replace_all=true
   ```
   Do NOT use sed — use the Edit tool for reliable cross-platform replacement.

2. **Update `CLAUDE.md`** — fill the Project Context section:
   ```markdown
   ## Project Context
   
   [User's project description]
   
   - **Phase:** [User's phase]
   - **Revenue model:** [User's revenue model]
   - **Development approach:** AI-assisted with Claude Code
   ```

3. Update `docs/workflow-status.md` step 00 Phase 1 checklist items.

---

## Phase 2: Brand & Design

Ask the user:

1. **"Three words that define your brand personality"**
   - Example: "Warm, Minimal, Trustworthy" or "Bold, Playful, Fast"
   - Tip: Think about how your product should *feel*, not what it *does*

2. **"Who are your primary users?"**
   - Ask for: age range, context, what makes them unique
   - Example: "Caregivers aged 35-65 managing end-of-life care — emotionally overwhelmed, time-poor"

3. **"Describe your visual direction — what should the product feel like? Any apps you admire aesthetically, or styles you want to avoid?"**
   - Example: "Soft and organic, like CaringBridge meets Apple Photos. NOT clinical health apps or social media feeds."

4. **"What are your top 3-5 design principles? (or say 'skip' to fill in later)"**
   - Example: "1. Gentleness over engagement 2. Privacy is felt, not just enforced 3. Accessibility is non-negotiable"

### Actions after Phase 2:

1. **Replace `{{BRAND_PERSONALITY}}`** across all files with the user's three words.

2. **Update `.impeccable.md`** — fill all sections with the user's answers:
   - Users section (primary persona, extended, job to be done)
   - Brand Personality (three words, emotional goals, voice)
   - Aesthetic Direction (visual tone, references, anti-references)
   - Design Principles (the user's principles, or leave `<!-- CUSTOMIZE -->` if skipped)

3. Update `docs/workflow-status.md` step 00 Phase 2 checklist items.

---

## Phase 3: Tech Stack

Ask the user:

1. **"What's your frontend framework?"**
   - Examples: "Flutter (Dart 3.0+)", "React Native", "Next.js", "SwiftUI", "Not decided yet"

2. **"What's your backend?"**
   - Examples: "Supabase (PostgreSQL)", "Firebase", "Rails API", "Node.js + Prisma", "Not decided yet"

3. **"Any other key technologies? (auth, payments, analytics, CI/CD — or 'skip')"**
   - Example: "Stripe for payments, Sentry for errors, PostHog for analytics, GitHub Actions for CI"

### Actions after Phase 3:

1. **Update `CLAUDE.md`** — fill the Tech Stack table with the user's answers. For any "not decided yet" answers, leave the placeholder with a note.

2. **Update `.claude/agents/architect.md`** — fill the Stack Expertise section with the user's tech choices.

3. Update `docs/workflow-status.md` step 00 Phase 3 checklist items.

---

## Phase 4: MCP Connections (optional)

Ask the user:

1. **"Do you want to connect Figma for AI-driven screen construction? (yes / no / later)"**

   - **If yes:**
     - Verify `.mcp.json` has the figma entry (it should — pre-configured)
     - Test: run `mcp__figma__whoami`
     - If successful: report "Figma connected as [identity]"
     - If auth needed: guide user through browser OAuth ("Claude Code will open your browser — approve the connection")
     - Report Figma plan tier if detectable

   - **If no/later:** Note in workflow-status.md. Figma is only needed at step 07.

2. **"Do you want to connect Google Drive for document access? (yes / no / later)"**

   - **If yes:**
     - Ask: "What's your Google OAuth Client ID?"
     - Ask: "What's your Google OAuth Client Secret?"
     - Update `.mcp.json` with the credentials (replace `{{OAUTH_CLIENT_ID}}` and `{{OAUTH_CLIENT_SECRET}}`)
     - Ask: "What's your project's absolute path?" → replace `{{PROJECT_ROOT}}` in `.mcp.json`
     - Run auth: `npx -y @modelcontextprotocol/server-gdrive auth`
     - Ask: "Do you have Google Doc IDs to allowlist? (paste IDs with a short label for each, e.g., 'Design Doc: 1j_BUYUq...' — or 'skip')"
     - If provided:
       - Replace `{{GDOC_ID_design_doc}}` in CLAUDE.md with the first doc ID (or whichever the user labels as design)
       - Replace `{{GDOC_ID_backlog}}` in CLAUDE.md with the second doc ID (or whichever the user labels as backlog)
       - Update the allowlist table in CLAUDE.md with the document names and IDs
     - If skipped: leave the `{{GDOC_ID_*}}` placeholders in place — they won't cause errors, GDrive features will just be inactive

   - **If no/later:** Leave `{{OAUTH_*}}` placeholders. Note in workflow-status.md.

3. Update `docs/workflow-status.md` step 00 Phase 4 checklist items.

---

## Phase 5: External Skills (optional)

Ask the user:

**"Install Impeccable design quality skills? These add /critique, /audit, /polish, /colorize, and 17 more design commands. (yes / no / later)"**

- **If yes:**
  - Run: `npx skills install pbakaus/impeccable`
  - This installs 21 skills: adapt, animate, arrange, audit, bolder, clarify, colorize, critique, delight, distill, extract, frontend-design, harden, normalize, onboard, optimize, overdrive, polish, quieter, teach-impeccable, typeset
  - After installation, report which skills were successfully installed
  - Note: `/teach-impeccable` is only needed if updating the design direction later — skills read `.impeccable.md` automatically
- **If no/later:** Note in workflow-status.md. These are optional enhancements that can be installed anytime.

Update `docs/workflow-status.md` step 00 Phase 5 checklist items.

---

## Phase 6: Validation

Perform these checks automatically (no user input needed):

1. **Placeholder scan:**
   - `grep -r "{{" . --include="*.md" --include="*.json" -l` — count remaining `{{PLACEHOLDER}}` variables
   - Report: "X placeholder variables remain in Y files" (list them)
   - Distinguish between: unfilled required vars (problem) vs optional vars left for later (ok)

2. **Content checks:**
   - Read `CLAUDE.md` — verify Project Context section has non-placeholder content
   - Read `.impeccable.md` — verify Brand Personality has non-placeholder content
   - Read `.mcp.json` — check if it's valid JSON

3. **Report setup summary:**

```
Setup Summary
─────────────
Project: [name]
Description: [1-liner]
Brand: [three words]
Tech: [frontend] + [backend]
Figma: [connected / not configured / later]
GDrive: [connected / not configured / later]
Impeccable skills: [installed / not installed / later]

Remaining placeholders: [count] in [file count] files
  Required unfilled: [list if any]
  Optional (ok to skip): [list]

Status: needs-review
Run /00-setup [continue] to approve and start the pipeline.
```

4. Set status to `needs-review` in `docs/workflow-status.md`.

---

## Error Recovery

If any phase fails or the user provides unclear input:
1. Ask ONE clarifying question — don't guess
2. If a file write fails, report the specific error and suggest manual editing as fallback
3. Never leave a file in a half-written state — either complete the section or leave the placeholder

---

## Status Report

When `[status]` is invoked:

1. Read `docs/workflow-status.md` step 00 section
2. Report which phases are completed (1-6)
3. Count remaining `{{PLACEHOLDER}}` variables
4. Count remaining `<!-- CUSTOMIZE` markers (distinguishing critical vs optional)
5. Report: "Phase X of 6 completed. Y placeholders remaining."

---

## Force Status Update

When `[update]` is invoked:

1. Re-scan all project files for `{{` patterns
2. Re-scan for `<!-- CUSTOMIZE` markers
3. Re-check CLAUDE.md and .impeccable.md for non-placeholder content
4. Update `docs/workflow-status.md` step 00 with current state
5. Report updated counts
