# Figma MCP — Reference for {{PROJECT_NAME}}

> Last updated: April 2026
> Sources: [Figma MCP Docs](https://developers.figma.com/docs/figma-mcp-server/), [Guide](https://help.figma.com/hc/en-us/articles/32132100833559), [Prompting Guide](https://developers.figma.com/docs/figma-mcp-server/write-effective-prompts/)

## What is Figma MCP?

Figma's official MCP (Model Context Protocol) server enables AI coding agents to read and write to Figma files. Claude Code is an explicitly supported client. The remote server provides full read+write canvas access.

---

## Server Options

### Remote MCP Server (Recommended)

- **Endpoint:** `https://mcp.figma.com/mcp`
- **Transport:** HTTP
- **Auth:** Browser-based OAuth (handled by server, no API key needed)
- **Capabilities:** Full read + write (including write-to-canvas and code-to-canvas)
- **Plan requirements:** Available on all seats and plans
- **Status:** Beta (free during beta, future usage-based pricing for write operations)

### Desktop MCP Server (Not Recommended)

- Runs locally through Figma Desktop app
- Limited to Dev/Full seats on paid plans only
- **No write-to-canvas** — read-only for design extraction
- Use only if remote server is unavailable

---

## Configuration

### `.mcp.json` entry

```json
{
  "mcpServers": {
    "figma": {
      "type": "http",
      "url": "https://mcp.figma.com/mcp"
    }
  }
}
```

### CLI alternative

```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

No API key or environment variables needed. Authentication happens via browser OAuth on first connection.

---

## Capabilities

### Read Operations

| Capability | Description |
|------------|-------------|
| Read file structure | Get pages, frames, components from a Figma file |
| Read frames | Extract layout tree, properties, fills, strokes |
| Read variables | Access design variable collections (colors, spacing, etc.) |
| Read components | Get component definitions and instances |
| Read layout data | Auto layout properties, constraints, sizing |
| Read Code Connect | Access design-to-code mappings |

### Write Operations (Remote Server Only)

| Capability | Description |
|------------|-------------|
| Create frames | Add new frames with dimensions, fills, layout |
| Create components | Define reusable components with variants |
| Set auto layout | Apply flexbox-like layout to containers |
| Create variables | Define variable collections for tokens |
| Write text nodes | Add and modify text content |
| Set fills/strokes | Apply colors, gradients, effects |
| Code-to-canvas | Convert live web UI into editable Figma layers |

### Code Operations

| Capability | Description |
|------------|-------------|
| Frame-to-code | Generate code from selected Figma frames |
| Extract tokens | Pull variable values for code generation |

---

## Figma Concepts for Agents

### Hierarchy

```
File
  └── Page (organize by flow: "Onboarding", "Core Loop", etc.)
       └── Frame (a single screen, 390x844 for mobile)
            └── Auto Layout Container
                 ├── Text Node
                 ├── Rectangle / Shape
                 ├── Component Instance
                 └── Nested Auto Layout
```

### Auto Layout (Figma's Flexbox)

Auto layout is the primary layout mechanism. Map to Flutter:
- **Vertical auto layout** → `Column`
- **Horizontal auto layout** → `Row`
- **Gap** → `SizedBox` or spacing parameter
- **Padding** → `EdgeInsets`
- **Fill container** → `Expanded` / `Flexible`
- **Hug contents** → intrinsic sizing

### Design Variables (Figma's Token System)

Variables are named values that can be referenced across a file. They map directly to `.impeccable.md` tokens:
- **Color variables** → hex values for fills, text colors
- **Number variables** → spacing, corner radius, font sizes
- **String variables** → font family names

Organize into collections:
- "{{PROJECT_NAME}} Colors" — all color tokens
- "{{PROJECT_NAME}} Spacing" — spacing scale
- "{{PROJECT_NAME}} Typography" — font sizes, weights, line heights

### Components

Reusable elements with:
- **Main component** — the definition (like a class)
- **Instances** — copies that inherit from the main (like objects)
- **Variants** — different states (default, hover, disabled, active)

---

## Best Practices for Programmatic Design

### Always Use Variables

Never hardcode colors or spacing. Create variables first, reference them in all frames. This ensures:
- Single source of truth for tokens
- Easy global updates
- Clean token extraction for Flutter

### Name Layers Semantically

Match Flutter widget structure:
- `AppBar` not `Rectangle 1`
- `FeedCard` not `Frame 42`
- `CTAButton` not `Button copy`

### Use Auto Layout Everywhere

Every container should have auto layout. No absolute positioning except:
- Overlay sheets
- FABs (floating action buttons)
- Badge indicators

### Build Components for Repeated Patterns

Extract these into components early:
- `Button` (primary, secondary, text variants)
- `TextInput` (default, focused, error states)
- `FeedCard` (photo, story, text variants)
- `AppBar` (with back, with title, with actions)
- `BottomNav` (5 tabs with active/inactive states)
- `Avatar` (sizes: 26px, 32px, 36px, 42px)
- `ReactionButton` <!-- CUSTOMIZE: your project's reaction types -->

### One Page per Batch

Organize the Figma file by batches:
- Page "Onboarding" — screens 2-5
- Page "Core Loop" — screens 6-9
- Page "Completion" — screens 10-18
- Page "Management" — screens 19-33
- Page "Components" — shared component definitions

---

## Project-Specific Patterns

<!-- CUSTOMIZE: Replace the examples below with your project's design tokens from the design document. These patterns define reusable construction specs for common UI elements. Reference your token names and hex values from docs/Design/{{PROJECT_NAME}}-design-document.md -->

### Standard Frame

```
Frame: 390x844, vertical auto layout
  Fill: var(pageBg)    <!-- CUSTOMIZE: your page background token -->
  Padding: 0 (managed per section)
  Gap: 0 (managed explicitly)
```

### Standard Card

```
Frame: fill width, auto height, vertical auto layout
  Fill: var(cardBg)    <!-- CUSTOMIZE: your card background token -->
  Corner radius: 18px  <!-- CUSTOMIZE: your corner radius -->
  Padding: 16px
  Shadow: <!-- CUSTOMIZE: your shadow definition -->
```

### Standard CTA Button

```
Frame: fill width, 50-52px height, horizontal auto layout, center aligned
  Fill: var(primaryAction)       <!-- CUSTOMIZE: your action color token -->
  Corner radius: 16px
  Shadow: <!-- CUSTOMIZE: your button shadow -->
  Text: [body font] 600 15px, var(surfaceLight)  <!-- CUSTOMIZE: your font + text color -->
```

### Standard Text Input

```
Frame: fill width, auto height, vertical auto layout
  Label: [body font] 600 10px uppercase, var(textMuted)  <!-- CUSTOMIZE: your font + color tokens -->
  Gap: 8px
  Input frame: fill width, 50px height
    Fill: var(surfaceLight)
    Border: 1px solid var(divider)
    Corner radius: 12px
    Text: [body font] 400 16px, var(textPrimary)
    Placeholder: [body font] 400 16px, var(textMuted)
```

---

## Prompting Best Practices (from Figma docs)

When using Figma MCP through Claude Code:

1. **Be specific** — "Generate iOS-style code from the Login frame" not "make code"
2. **Reference your project** — "Use components from lib/core/theme/"
3. **Specify framework** — "Generate Flutter/Dart widgets using Material 3"
4. **Designate file paths** — "Add this to lib/features/auth/presentation/screens/"
5. **Request modifications** — "Update the existing FeedCard widget" rather than creating new files
6. **Specify layout** — "Use Column with CrossAxisAlignment.stretch"

---

## Limitations

- **Beta status** — tools may change, expect occasional bugs
- **No Figma AI via MCP** — First Draft (AI screen generation) is GUI-only, cannot be triggered via MCP
- **Write operations will be paid** — free during beta, usage-based pricing coming
- **Font availability** — Custom fonts may need manual upload to the Figma team library depending on plan tier <!-- CUSTOMIZE: your font names -->
- **Rate limits** — see "MCP Call Budget & Optimization" below

---

## MCP Call Budget & Optimization

Every `mcp__figma__*` tool invocation (except exemptions) counts as one call against your plan's quota. Each `use_figma` call can execute up to **50,000 characters of JavaScript** — pack maximum work per call.

### Rate Limits by Plan

| Plan | Limit | Per-Minute |
|------|-------|-----------|
| **Starter** | 6/month | 10/min |
| **Professional** | 200/day | 15/min |
| **Organization** | 200/day | 20/min |
| **Enterprise** | 600/day | 20/min |
| View/Collab seats (any plan) | 6/month | — |

### Exempt Tools (don't count against quota)

- `whoami`
- `generate_figma_design`
- `add_code_connect_map`

### Counted Tools

`use_figma`, `create_new_file`, `get_design_context`, `get_screenshot`, `get_metadata`, `get_variable_defs`, `search_design_system`, `get_figjam`, `get_code_connect_map`, `get_code_connect_suggestions`, `get_context_for_code_connect`, `send_code_connect_mappings`, `create_design_system_rules`, `generate_diagram`

### Core Optimization Rules

1. **Batch all setup into one `use_figma` call** — pages, variable collections, text styles, and base components in a single script. Never split setup across calls.
2. **Batch multiple screens per call** — construct 2-4 screens in one `use_figma` call (depends on complexity; stay under 50K chars).
3. **Pre-compose JavaScript offline** — write and review the full Plugin API script before calling. Don't iterate on syntax via live calls.
4. **Combine read+write** — if you need to read existing state and then modify, do both in one call via `figma.getNodeById()`.
5. **Use `get_design_context` sparingly** — it costs 1 call per node. Batch reads via `use_figma` with `figma.getNodeById()` instead.
6. **Cache session state** — store the planKey, file key, page IDs, and variable IDs in conversation context. Never re-fetch what you already know.
7. **Group iterations** — if 3 screens need the same fix (e.g., wrong spacing), fix all 3 in one call, not 3 separate calls.
8. **Return structured data** — always return IDs, names, and sizes from `use_figma` scripts to avoid needing a follow-up read call.
9. **Include error handling** — wrap Plugin API calls in try/catch. A failed call still burns a quota slot.

### Call Budget Template (Professional plan, 200/day)

| Phase | Calls | What |
|-------|-------|------|
| Setup | 1 | `create_new_file` |
| Foundation | 1 | Pages + variables + text styles + all base components |
| Batch 1A (screens 1-5) | 2-3 | ~2 screens per call |
| Batch 1A review | 1 | Read all 5 frames, generate compliance report |
| Batch 1A fixes | 1-2 | Batch all fixes per review round |
| Batch 1B (screens 6-9) | 2-3 | Complex screens (Feed, Post Detail) may need 1 call each |
| Batch 1B review + fixes | 2-3 | Read + fix cycle |
| Batch 2 (screens 10-18) | 4-5 | 2 screens per call |
| Batch 3 (screens 19-33) | 6-8 | 2-3 screens per call |
| Final review | 2-3 | Cross-screen consistency check + fixes |
| **Total estimate** | **~25-35 calls** | Well within 200/day budget |

### Anti-Patterns (never do these)

- **One call per variable** — create all variables in one script, not separate calls for colors, spacing, and text styles
- **One call per screen** when screens share the same structure — batch 2-4 similar screens
- **Reading what you just wrote** — store the return value from `use_figma`, don't call `get_design_context` on nodes you created in the same session
- **Using `get_screenshot` to verify** — verify via the structured return value of `use_figma` instead
- **Retrying on syntax errors** — each retry burns a call. Pre-validate JavaScript before sending.

---

## Figma Plugin API Patterns

Executable JavaScript patterns for `use_figma` calls. These are **mandatory** — always follow these patterns, never improvise alternatives.

### A. Screen Frame Setup

```javascript
const screen = figma.createFrame();
screen.name = "01 – Welcome";
screen.resize(390, 844);                   // 1. Set size BEFORE auto layout
screen.layoutMode = "VERTICAL";            // 2. Set auto layout direction
screen.counterAxisSizingMode = "FIXED";    // 3. Lock width at 390px
screen.primaryAxisSizingMode = "AUTO";     // 4. Height grows with content
screen.paddingLeft = 20;                   // 5. 20px horizontal margins
screen.paddingRight = 20;
screen.paddingTop = 0;
screen.paddingBottom = 0;
screen.itemSpacing = 0;                    // Manage spacing explicitly per section
screen.fills = [boundPaint("pageBg", "#XXXXXX")];  // CUSTOMIZE: your page background hex
```

**Order matters:** `resize()` → `layoutMode` → `counterAxisSizingMode` → `padding*` → children.

### B. Child Sizing Rules

**ALL direct children** of an auto layout frame MUST use `layoutSizingHorizontal = "FILL"`. This makes them expand to the parent's content area (390 - 20 - 20 = 350px), respecting padding.

```javascript
const section = figma.createFrame();
section.name = "FormSection";
section.layoutMode = "VERTICAL";
section.itemSpacing = 12;
section.fills = [];
screen.appendChild(section);               // 1. Append FIRST
section.layoutSizingHorizontal = "FILL";   // 2. Set FILL AFTER appending
section.layoutSizingVertical = "HUG";
```

**Rules:**
- NEVER hardcode child width (e.g., `resize(350, ...)`) — always use FILL
- Set `layoutSizingHorizontal = "FILL"` AFTER `appendChild()` — it throws if set before
- Use `layoutSizingVertical = "HUG"` for sections that grow with content

### C. Component Instance Pattern

```javascript
// Look up component by node ID (cached from setup)
const btnComponent = figma.getNodeById("12:2");  // Button/Primary
const btnInstance = btnComponent.createInstance();
parentFrame.appendChild(btnInstance);
btnInstance.layoutSizingHorizontal = "FILL";

// Override text on instance — find the text node inside
const textNode = btnInstance.findOne(n => n.type === "TEXT");
if (textNode) {
  await figma.loadFontAsync(textNode.fontName);
  textNode.characters = "Create Account";
}
```

**Always use instances** for Button, TextInput, Card, AppBar, BottomNav, Avatar. Only build inline if no matching component exists.

### D. Color Variable Binding

```javascript
function boundPaint(varName, fallbackHex) {
  const vars = figma.variables.getLocalVariables("COLOR");
  const v = vars.find(v => v.name === varName);
  const r = parseInt(fallbackHex.slice(1,3),16)/255;
  const g = parseInt(fallbackHex.slice(3,5),16)/255;
  const b = parseInt(fallbackHex.slice(5,7),16)/255;
  const paint = { type: "SOLID", color: {r,g,b} };
  if (v) return figma.variables.setBoundVariableForPaint(paint, "color", v);
  return paint;
}
```

Define this helper once at the top of every `use_figma` script. Always use `boundPaint()` instead of raw color objects.

### E. Text Node Pattern

```javascript
// CUSTOMIZE: Replace font family and color tokens with your design document values
await figma.loadFontAsync({ family: "YourFont", style: "SemiBold" });
const text = figma.createText();
text.fontName = { family: "YourFont", style: "SemiBold" };
text.fontSize = 15;
text.characters = "Button Label";
text.lineHeight = { value: 15 * 1.40, unit: "PIXELS" };  // size × line-height ratio
text.fills = [boundPaint("surfaceLight", "#XXXXXX")];  // CUSTOMIZE: your text color token
```

**Always load the font before creating text.** Line height is specified as pixels (fontSize * ratio), not as a multiplier.

### F. Centering Pattern

```javascript
// Center elements horizontally inside a vertical auto layout frame:
const logoWrapper = figma.createFrame();
logoWrapper.name = "LogoWrapper";
logoWrapper.layoutMode = "HORIZONTAL";
logoWrapper.primaryAxisAlignItems = "CENTER";   // Horizontal center
logoWrapper.counterAxisAlignItems = "CENTER";   // Vertical center
logoWrapper.fills = [];
parent.appendChild(logoWrapper);
logoWrapper.layoutSizingHorizontal = "FILL";
logoWrapper.layoutSizingVertical = "HUG";
// Add logo elements inside logoWrapper — they will be centered
```

Never center by calculating x offset. Always use a wrapper with `primaryAxisAlignItems = "CENTER"`.

### G. Spacer Pattern

```javascript
// Add explicit vertical spacing between sections
const spacer = figma.createFrame();
spacer.name = "Spacer-32";
spacer.resize(1, 32);  // Height = spacing value
spacer.fills = [];
parent.appendChild(spacer);
spacer.layoutSizingHorizontal = "FILL";
spacer.layoutSizingVertical = "FIXED";
```

Use spacers for gaps between major sections when `itemSpacing` on the parent doesn't match the varied spacing needs.

### H. Common Gotchas

| Gotcha | Symptom | Fix |
|--------|---------|-----|
| Child FIXED width > parent content area | **Left-clipping** | `layoutSizingHorizontal = "FILL"` on child |
| Setting FILL before `appendChild()` | Error thrown | Append child first, THEN set FILL |
| Calling `resize()` after `layoutMode` | Layout breaks | Resize BEFORE setting `layoutMode` |
| Hardcoding child width (e.g., 350px) | Clips if padding changes | Always use FILL |
| Not loading font before setting text | Silently fails | Always `await figma.loadFontAsync()` first |
| `clipsContent = true` (default) | Overflow hidden | Set `false` only for overlay screens |
| Using `layoutSizingHorizontal = "HUG"` on child | Child shrinks, doesn't fill | Use FILL to expand to parent width |
| Building components inline | No single source of truth | Use `getNodeById().createInstance()` |

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP not connecting | Run `/mcp` in Claude Code to check server status. Verify `.mcp.json` has `"type": "http"` |
| OAuth prompt not appearing | Try disconnecting and reconnecting the MCP server |
| Write operations failing | Ensure you're using the remote server, not desktop. Check file permissions in Figma |
| Variables not applying | Verify variable collection exists. Check variable names match exactly |
| Fonts not rendering | Upload your custom fonts to the Figma team library, or use fallbacks <!-- CUSTOMIZE: your font names --> |
