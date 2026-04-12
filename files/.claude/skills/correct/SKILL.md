---
name: correct
description: Log a design correction from Figma review, propagate it across all affected screens, and apply the fix via MCP. Use when the user identifies a visual issue on a constructed screen — wrong size, color, spacing, element, or layout. Triggers include "correct", "fix this on all screens", "this should be X not Y", or design review feedback.
user-invocable: true
argument-hint: "[screen] [description of issue]"
---

# Design Correction Skill

Log, propagate, and apply design corrections across Figma screens.

## MANDATORY PREPARATION

Read these files before proceeding:
1. `docs/Design/corrections-log.md` — existing corrections and their status
2. `docs/Design/figma-design-plan.md` — construction specs and screen inventory (to identify affected screens and node IDs)
3. `.claude/agents/figma-designer.md` — agent rules for applying fixes

---

## Step 1: Capture the Correction

Parse the user's input to extract:
- **Screen:** Which screen was the issue found on?
- **Element:** What specific element is wrong? (Use the layer/frame name from construction specs)
- **Current state:** What does it look like now?
- **Expected state:** What should it look like?
- **Rationale:** Why is the current state wrong? (Reference design document, .impeccable.md, or user preference)

If any of these are unclear from the user's input, ask ONE clarifying question before proceeding.

## Step 2: Identify Affected Screens

Search the construction specs in `docs/Design/figma-design-plan.md` for the element name. List every screen that contains this element.

Also check:
- The Consistency Rules section — does this element appear in a rule that applies to all screens?
- The Component Instance Reference — is this element part of a shared component?

Present the affected screens list to the user for confirmation:
> "This correction affects screens: 1, 2, 3, 4, 5 (all screens with [ElementName]). Confirm?"

## Step 3: Log the Correction

Assign the next sequential ID (C-001, C-002, ...) by reading existing entries in `docs/Design/corrections-log.md`.

Append the correction to the **Active Corrections** section using this format:

```markdown
### C-NNN: [Short description]

- **Found on:** Screen N (Name)
- **Date:** [today's date]
- **What:** [Observable issue — current state vs expected state]
- **Why:** [Root cause / design rationale for the fix]
- **Affects:** [Comma-separated list of all affected screens]
- **Applied to:**
  - [ ] Screen N (Name) — node `XX:YY`
  - [ ] Screen M (Name) — node `XX:YY`
  - [ ] ...
- **Spec update needed:** [Yes if construction specs in figma-design-plan.md contain the old value]
- **Rule candidate:** [Yes if affects 3+ screens and represents a reusable pattern]
```

Include the Figma node IDs from the Screen Inventory table in figma-design-plan.md — the agent needs these to apply fixes.

## Step 4: Apply the Correction

Ask the user: **"Apply this correction now to all affected screens, or log only?"**

### If applying now:

1. **Component health check (mandatory when fix involves components):**
   If the correction involves replacing elements with component instances OR modifying a shared component, the agent prompt MUST include this disambiguation step:
   > "Before applying the fix, inspect the source component(s) using `get_metadata`. Verify each component has:
   > - Expected children (background fills, text nodes, icons)
   > - Correct dimensions (width × height)
   > - Proper visual content (not an empty shell)
   >
   > If the component itself is broken or empty, fix the component FIRST, then instance it. Never swap in a broken component as a replacement."

2. Launch the `figma-designer` agent with a prompt that includes:
   - The correction ID and full description
   - The list of affected screen node IDs
   - The specific change to make (element name, old value, new value)
   - The file key from the design plan's Figma IDs table
   - Instruction to batch all fixes into minimal `use_figma` calls
   - The component health check instruction (if applicable)
   - Instruction to take a **zoomed screenshot of the specific element** after fixing
3. After the agent confirms success, check all "Applied to" boxes with today's date
4. If "Spec update needed: Yes", update the frame tree specs in `docs/Design/figma-design-plan.md`

### If logging only:

Leave all boxes unchecked. The correction will be picked up:
- During the next Refine step in the execution checklist
- When the agent reads the corrections log before constructing new screens (per Rule 8)

## Step 5: Rule Graduation Check

If the correction is marked **"Rule candidate: Yes"**:

1. Check graduation criteria:
   - Affects **3+ screens** (not a one-off fix)
   - Has been **fully applied** (all "Applied to" boxes checked)
   - Represents a **reusable pattern** (e.g., "corner radii must be on-scale") not just a value fix
2. If criteria are met, propose a one-line rule for the user to confirm
3. On confirmation:
   - Add a row to the **Correction-Graduated Rules** table in `docs/Design/figma-design-plan.md`
   - Move the correction from "Active Corrections" to "Completed Corrections" in the log

If criteria are NOT met, move to Completed without graduating.

## Output

After completing the flow, report:

| Field | Value |
|-------|-------|
| **Correction ID** | C-NNN |
| **Description** | [short description] |
| **Screens affected** | [count and list] |
| **Applied** | Now / Logged for later |
| **Specs updated** | Yes / No / N/A |
| **Rule graduated** | Yes (rule text) / No (reason) |

---

## IMPORTANT RULES

- **Never skip propagation.** Every correction MUST identify all affected screens, even if the user only mentioned one.
- **Never apply without logging.** Every fix must have a corrections-log.md entry BEFORE any MCP call.
- **Batch MCP calls.** When applying to multiple screens, fix all in one agent call, not one call per screen.
- **Preserve the log.** Never delete or modify completed corrections. The log is an audit trail.
- **One correction per invocation.** If the user describes multiple issues, handle them as separate corrections (C-001, C-002). Ask which to address first.
- **Include node IDs.** Always include Figma node IDs in the "Applied to" list so the agent can target the right frames without needing a read call.
