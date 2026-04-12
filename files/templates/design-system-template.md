# Design System Kit Template

> Template for building a Figma design system kit — visual reference pages that document tokens, components, and patterns before screen construction begins.
>
> **This solves for:** Visual aid and screen construction granularity.
> **Not to be confused with:** The design plan (`figma-design-plan.md`), which solves for orchestration and execution planning.

---

## How to Use This Template

1. **Fill in** the project-specific values in each section below
2. **Construct** each page in Figma using the MCP, following the layout guides
3. **Review** the completed kit with the team before any screen construction
4. **Reference** the kit pages during screen construction to ensure consistency

### Where This Fits in the Workflow

```
Setup (variables + base components)
  → Design System Kit (this template — 12 reference pages)
    → Human Review Gate (validate tokens, states, contrast)
      → Component Expansion (new components identified in kit)
        → Screen Construction (screens reference kit pages)
```

The kit is built **once** after variables and base components exist, and becomes a **living reference** for all subsequent screen work.

---

## Page 1: Color System

**Purpose:** Visual proof that all color tokens render correctly, semantic roles are clear, and contrast ratios pass accessibility requirements.

### Sections to Include

#### 1.1 Palette Swatches

Group colors by function. Each swatch shows: token name, hex value, filled rectangle.

| Group | Tokens |
|-------|--------|
| **Backgrounds** | _list background tokens (e.g., pageBg, cardBg, surface variants)_ |
| **Text** | _list text color tokens (e.g., primary text, secondary text, muted)_ |
| **Accents** | _list accent/action tokens (e.g., primary action, links, soft variants)_ |
| **Semantic** | _list status tokens (e.g., error, success, warning, info)_ |
| **Borders** | _list border/divider tokens_ |

#### 1.2 Theme Role Mapping

Side-by-side grid showing how each token maps to M3 ColorScheme roles, with light and dark values.

```
| M3 Role             | Token     | Light    | Dark     |
|---------------------|-----------|----------|----------|
| primary             | _token_   | _hex_    | _hex_    |
| onPrimary           | _token_   | _hex_    | _hex_    |
| primaryContainer    | _token_   | _hex_    | _hex_    |
| secondary           | _token_   | _hex_    | _hex_    |
| tertiary            | _token_   | _hex_    | _hex_    |
| error               | _token_   | _hex_    | _hex_    |
| surface             | _token_   | _hex_    | _hex_    |
| surfaceContainer    | _token_   | _hex_    | _hex_    |
| onSurface           | _token_   | _hex_    | _hex_    |
| onSurfaceVariant    | _token_   | _hex_    | _hex_    |
| outline             | _token_   | _hex_    | _hex_    |
| outlineVariant      | _token_   | _hex_    | _hex_    |
```

#### 1.3 Contrast Validation Grid

Show every text-on-background combination used in the product, with contrast ratio and WCAG pass/fail.

```
| Text Token | Background Token | Ratio  | AA Normal | AA Large |
|------------|-----------------|--------|-----------|----------|
| _text_     | _bg_            | _N:1_  | _pass/fail_ | _pass/fail_ |
```

---

## Page 2: Typography

**Purpose:** Every type style rendered at actual size with metadata, proving fonts load correctly in Figma and establishing visual hierarchy.

### Sections to Include

#### 2.1 Display / Editorial Styles

Each style shows: sample text at actual size, then metadata line below.

```
Format per style:
  [Sample text in the actual font/size/weight]
  Token · Font Family · Size/LineHeight · Weight · LetterSpacing
```

| Token | Font | Size | Weight | Line Height | Letter Spacing | Sample |
|-------|------|------|--------|-------------|----------------|--------|
| _display-lg_ | _font_ | _size_ | _weight_ | _lh_ | _ls_ | _sample text_ |
| _display-md_ | | | | | | |
| _headline-*_ | | | | | | |

#### 2.2 Interface / UI Styles

Same format as above, for body, label, and metadata styles.

| Token | Font | Size | Weight | Line Height | Letter Spacing | Sample |
|-------|------|------|--------|-------------|----------------|--------|
| _body-lg_ | | | | | | |
| _body-md_ | | | | | | |
| _label-lg_ | | | | | | |
| _label-md_ | | | | | | |
| _body-sm_ | | | | | | |

#### 2.3 Special Contexts (if applicable)

Side-by-side comparison of any context-specific type treatments (e.g., compose vs. reading mode, code blocks, quote styles).

```
| Context | Font | Token | Visual Treatment |
|---------|------|-------|------------------|
| _context_ | _font_ | _token_ | _description_ |
```

---

## Page 3: Spacing & Shape

**Purpose:** Visual ruler for spacing scale, corner radius specimens, and elevation/shadow comparison.

### Sections to Include

#### 3.1 Spacing Scale

Visual ruler showing each spacing value as a colored bar with label.

| Token | Value | Usage |
|-------|-------|-------|
| _space-1_ | _4px_ | _usage_ |
| _space-2_ | _8px_ | _usage_ |
| ... | | |

#### 3.2 Corner Radius Scale

Identical rectangles rendered at each radius value, labeled.

| Token | Value | Usage |
|-------|-------|-------|
| _radius-xs_ | _6px_ | _badges, chips_ |
| _radius-sm_ | _12px_ | _photo blocks_ |
| ... | | |

#### 3.3 Elevation / Shadow Specimens

Cards rendered at each shadow level, side-by-side on the page background.

| Token | Shadow Value | Usage |
|-------|-------------|-------|
| _shadow-none_ | none | _flat surfaces_ |
| _shadow-card_ | _value_ | _post cards_ |
| ... | | |

---

## Pages 4-12: Component Reference Pages

Each component page follows the same structure. Adapt this template per component.

### Component Page Template

```
┌─────────────────────────────────────────────────┐
│  [Component Name]                                │
│  See design guideline: [v2.md section ref]       │
│  [One-line description of component purpose]     │
│                                                   │
│  ┌─── State Matrix ──────────────────────────┐   │
│  │                                            │   │
│  │      Default    Focused   Pressed  Disabled│   │
│  │ Var1 [render]  [render]  [render] [render] │   │
│  │ Var2 [render]  [render]  [render] [render] │   │
│  │ ...                                        │   │
│  └────────────────────────────────────────────┘   │
│                                                   │
│  ┌─── Building Blocks ───────────────────────┐   │
│  │ [Exploded anatomy showing labeled parts]   │   │
│  │  Container: _token_ · _radius_ · _shadow_ │   │
│  │  Label: _token_ · _font_ · _size_         │   │
│  │  Icon slot: _size_ · _color token_         │   │
│  │  Touch target: _minimum size_              │   │
│  └────────────────────────────────────────────┘   │
│                                                   │
│  ┌─── Token Bindings ────────────────────────┐   │
│  │ | Part      | Property   | Token    |      │   │
│  │ |-----------|-----------|----------|      │   │
│  │ | Container | background | _token_  |      │   │
│  │ | Container | radius     | _token_  |      │   │
│  │ | Label     | color      | _token_  |      │   │
│  │ | Label     | typography | _token_  |      │   │
│  └────────────────────────────────────────────┘   │
│                                                   │
│  ┌─── Accessibility ─────────────────────────┐   │
│  │ Min touch target: _52px_                   │   │
│  │ Contrast ratio: _value_ (_pass/fail_)      │   │
│  │ Focus indicator: _description_             │   │
│  │ Reduced motion: _alt behavior_             │   │
│  └────────────────────────────────────────────┘   │
│                                                   │
│  ┌─── Flutter Mapping ───────────────────────┐   │
│  │ Widget: _Flutter widget name_              │   │
│  │ ThemeData: _theme property_                │   │
│  │ Key props: _list_                          │   │
│  └────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

### Recommended Component Pages

| Page | Components | State Matrix |
|------|-----------|-------------|
| **4. Buttons** | Primary, Secondary, Text Link | Default × Pressed × Disabled, with/without icon |
| **5. Text Inputs** | Default, Password | Default × Focused × Filled × Error |
| **6. Cards** | Photo Card, Story Card, Text Card | Building blocks: header, footer, photo block, story block |
| **7. Avatars** | Size variants (26-42px) | 6 color backgrounds, stacking example |
| **8. Navigation** | Bottom Nav, Top Bar (AppBar) | All active tab states, notification badge states, bar variants |
| **9. Reactions** | <!-- CUSTOMIZE: your project's reaction types --> | Active × Inactive, count variants (0, 1, 5, 99) |
| **10. Overlays & Sheets** | Bottom Sheet, Dialog, Toast | Sheet content states, dialog variants, toast types |
| **11. Badges & Chips** | Privacy Badge, Invite Chip, Indicators | Remove state, count states |
| **12. Empty States & Loading** | Empty feed, Empty search, Skeleton, Offline | Illustration + copy variants |

---

## Component Inventory Checklist

Use this table to track which components exist and which need to be built.

```
| # | Component | Figma Node ID | Variants | States | Kit Page | Status |
|---|-----------|---------------|----------|--------|----------|--------|
| 1 | _name_ | _id or TBD_ | _list_ | _list_ | _page #_ | _done/pending_ |
| 2 | | | | | | |
| ... | | | | | | |
```

---

## Construction Notes for the Figma Designer Agent

### Page Layout Convention

- Each reference page is a **frame on the Components page** of the Figma file
- Page frame width: **1440px** (wide enough for state matrices)
- Vertical auto layout with **48px** top padding, **32px** between sections
- Section titles: project display font, followed by a 1px divider
- Background: project page background token

### MCP Call Budget for Kit

| Phase | Estimated Calls | What |
|-------|----------------|------|
| Token pages (Color, Type, Spacing) | 2-3 | Swatches, specimens, rulers |
| Component state matrices (Buttons, Inputs, Cards) | 3-4 | Full variant × state grids |
| Pattern pages (Nav, Overlays, Badges, Empty) | 2-3 | Examples + building blocks |
| **Total kit construction** | **7-10 calls** | |

### Naming Convention

| Element | Naming Pattern | Example |
|---------|---------------|---------|
| Page frame | `Kit / [Topic]` | `Kit / Color System` |
| Section frame | `Section / [Name]` | `Section / Palette Swatches` |
| Component specimen | `[Component] / [Variant] / [State]` | `Button / Primary / Pressed` |
| Swatch | `Swatch / [Token]` | `Swatch / amber` |
| Type specimen | `Type / [Token]` | `Type / display-lg` |

---

## Review Checklist

Before approving the kit and proceeding to screen construction:

- [ ] All color tokens render correctly in Figma swatches
- [ ] All text styles render at correct size/weight/font in specimens
- [ ] Every component has all documented states visible
- [ ] Contrast validation grid shows all combinations pass WCAG AA
- [ ] Corner radius specimens match documented values
- [ ] Shadow specimens render warm-tinted (not neutral grey)
- [ ] Building block anatomies clearly label all parts
- [ ] Component state matrices are complete (no empty cells)
- [ ] Dark mode preview (if applicable) looks correct
- [ ] Kit page naming follows convention
