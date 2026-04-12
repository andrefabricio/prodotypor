# {{PROJECT_NAME}} — Visual Identity & Design Language

## Metadata

| Field | Value |
|-------|-------|
| **Document Type** | Design Brief |
| **Product Phase** | <!-- CUSTOMIZE: Your current phase (e.g., Phase 0 — MVP) --> |
| **Status** | Cornerstone Reference |
| **Version** | 1.0 |

> This document is the authoritative design backbone for {{PROJECT_NAME}}. It defines every visual, typographic, component, and interaction decision. Engineering, product, and design teams should treat it as the single source of truth.

---

# Design Philosophy

<!-- CUSTOMIZE: Replace with your project's governing design convictions. Ground each principle in user research, brand values, or inspiration findings. -->

| # | Principle | Description |
|---|-----------|-------------|
| 01 | **[Principle Name]** | [Actionable description a designer can use to resolve ambiguity] |
| 02 | **[Principle Name]** | [Description] |
| 03 | **[Principle Name]** | [Description] |
| 04 | **[Principle Name]** | [Description] |
| 05 | **[Principle Name]** | [Description] |
| 06 | **[Principle Name]** | [Description] |

---

# 1. Color System

<!-- CUSTOMIZE: Define your project's color palette. Every color must have a semantic token name. -->

## 1.1 Full Palette Reference

| Token | Hex | Role | Usage Context |
|-------|-----|------|---------------|
| **pageBg** | | Page Background | App-level background behind all screens |
| **cardBg** | | Card Background | Post cards, list items, elevated surfaces |
| **surfaceLight** | | Sheet / Modal | Bottom sheets, detail views |
| **textPrimary** | | Primary Text | Headings, author names, active labels |
| **textBody** | | Body Text | Post content, comments, descriptions |
| **textMuted** | | Secondary / Labels | Timestamps, metadata, placeholder text |
| **divider** | | Dividers | Horizontal rules, card borders |
| **primaryAction** | | Primary Action (Fills) | CTA buttons, FAB, active states |
| **primaryActionText** | | Primary Action (Text) | Active labels, link text, accent text |
| **success** | | Success Status | Positive indicators |
| **error** | | Error Status | Error states, destructive actions |
<!-- Add project-specific tokens as needed -->

## 1.2 Color Usage Rules

<!-- CUSTOMIZE: Define explicit rules preventing misuse -->

- [Action color split rule — when to use fill vs text variant]
- [Status color reservation rule]
- [Pure black/white prohibition or permission]
- [Text-on-fill contrast requirements]
- [Gradient policy]

## 1.3 Dark Mode Mapping

<!-- Include even if deferred — plan the token mapping now -->

| Light Token | Light Hex | Dark Hex | Notes |
|-------------|-----------|----------|-------|
| pageBg | | | |
| cardBg | | | |
| textPrimary | | | |
<!-- Map all tokens -->

---

# 2. Typography

## 2.1 Font Families

| Role | Typeface | Source | Fallback | Justification |
|------|----------|--------|----------|---------------|
| Display / Headlines | <!-- CUSTOMIZE --> | | | |
| Body / UI | <!-- CUSTOMIZE --> | | | |

## 2.2 Type Scale

| Role | Font | Weight | Size (px) | Line Height | Letter Spacing | Usage |
|------|------|--------|-----------|-------------|----------------|-------|
| display-lg | | | | | | Hero text, splash screens |
| display-md | | | | | | Section headings |
| headline-md | | | | | | Card titles, form headings |
| body-lg | | | | | | Primary content |
| body-md | | | | | | Standard body text |
| body-sm | | | | | | Captions, fine print |
| label-lg | | | | | | Button text, nav labels |
| label-md | | | | | | Tags, badges, timestamps |

## 2.3 Typography Rules

- **Line height by context:**
  - Display / Headline: [value] (tight)
  - Body: [value] (comfortable reading)
  - Labels: [value] (compact)
- **Maximum line length:** [characters] for body text
- **Truncation:** [ellipsis rules, "Read more" patterns]

---

# 3. Spacing & Layout

## 3.1 Spacing Scale

| Token | Value (px) | Usage |
|-------|-----------|-------|
| space-1 | 4 | Tight internal padding, icon gaps |
| space-2 | 8 | Compact spacing, inline elements |
| space-3 | 12 | Standard internal padding |
| space-4 | 16 | Card padding, section gaps |
| space-6 | 24 | Major section separators |
| space-8 | 32 | Screen-level margins |
| space-10 | 40 | Hero spacing |
| space-12 | 48 | Maximum spacing |

## 3.2 Layout Rules

- **Screen frame:** <!-- CUSTOMIZE: e.g., 390×844 (iPhone 14/15 @1x) -->
- **Horizontal margins:** [value] on both sides
- **Safe areas:** [top/bottom insets]
- **Corner radii:** [standard radius for cards, buttons, inputs]
- **Shadow:** [definition if used]

---

# 4. Components

<!-- CUSTOMIZE: Define each component your project needs. Spec all states. -->

## 4.1 Buttons

| Variant | Height | Corner Radius | Fill | Text | Border | States |
|---------|--------|---------------|------|------|--------|--------|
| Primary | | | primaryAction | surfaceLight | none | default, pressed, disabled |
| Secondary | | | transparent | primaryActionText | divider | default, pressed, disabled |
| Text | | | transparent | primaryActionText | none | default, pressed, disabled |

## 4.2 Text Inputs

| State | Border | Background | Text | Placeholder | Helper |
|-------|--------|------------|------|-------------|--------|
| Default | divider | surfaceLight | textBody | textMuted | textMuted |
| Focused | primaryAction | surfaceLight | textBody | textMuted | textMuted |
| Error | error | surfaceLight | textBody | textMuted | error |
| Disabled | divider | pageBg | textMuted | textMuted | — |

## 4.3 Cards

<!-- CUSTOMIZE: Define card variants for your project's content types -->

## 4.4 Navigation

### App Bar
<!-- Variants: Default, WithBack, WithActions -->

### Bottom Navigation
<!-- Tab count, icons, labels, active state -->

## 4.5 Avatars

<!-- Size variants, shapes, placeholder/fallback treatment -->

## 4.6 Project-Specific Components

<!-- CUSTOMIZE: Add components unique to your project -->

---

# 5. Interactions & Motion

- **Page transitions:** [type, duration, easing]
- **Element reveals:** [fade-in, slide, scale]
- **Loading states:** [skeleton, spinner, shimmer]
- **Success feedback:** [animation, toast, haptic]
- **Gesture support:** [swipe, long press, pull to refresh]
- **Scroll behaviour:** [overscroll, parallax, sticky headers]

---

# 6. Accessibility

- **WCAG compliance:** AA minimum (AAA for critical text)
- **Touch targets:** 48×48px minimum
- **Contrast verification:**

| Text Token | Background Token | Ratio | Result |
|------------|-----------------|-------|--------|
| textPrimary | pageBg | | |
| textBody | cardBg | | |
| textMuted | surfaceLight | | |
| primaryActionText | pageBg | | |

- **Screen reader conventions:** [label format, announcement patterns]
- **Reduced motion:** [alternative for animations]

---

# 7. Platform Specifics

<!-- CUSTOMIZE: Adapt to your target platform(s) -->

- **Framework:** <!-- e.g., Flutter (Dart 3.0+), Material 3 -->
- **Platform adaptations:** [iOS vs Android differences]
- **Offline visual treatment:** [how UI signals offline state]
- **Theme implementation:** <!-- e.g., lib/core/theme/ constants -->
