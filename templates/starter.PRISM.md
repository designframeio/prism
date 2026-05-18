---
format: prism-spec/1.0
name: my-design-system
version: 1.0.0
prismMdVersion: 1.0
designMdCompatible: true
description: My design system — replace this prose with your brand's identity.
license: Apache-2.0
homepage: ""

# Brand vs system ownership roots.
# brandOwnedRoots: tokens forkers/consumers customize for their brand.
# systemOwnedRoots: stable design-system machinery — alerts, spacing, scale.
brandOwnedRoots:
  - color.brand
  - typography.family
  - radius.brand
systemOwnedRoots:
  - color.alert
  - color.theme
  - shader
  - typography.scale
  - space
  - size
  - radius.article
  - radius.full
  - shadow
  - blur
  - border
  - transition
  - screen
  - layout

modes:
  - name: default
    description: Light mode — base palette.
    base: true
  # Add modes by appending here + authoring `## Modes ### <name>` subsections
  # with token fences carrying `$mode: <name>`.

# ── COLOR PRIMITIVES ──────────────────────────────────────────────────────
# TODO: Replace brand colors with your own. The starter ships neutral
# black-on-white + a placeholder accent — change these and the rest of the
# system inherits via $ref resolution.
color:
  brand:
    # Accent color. Replace with your brand's signature hue.
    # Both `key` and `key-end` set to the same color = flat (no gradient).
    # Set key-end to a different hue to enable a two-stop brand gradient.
    key:       { $type: color, $value: "#2563eb", $brand-owned: true, $description: "Brand accent color. TODO: replace with your brand's signature hue." }
    key-end:   { $type: color, $value: "#2563eb", $brand-owned: true, $description: "Accent gradient end-stop. Same as key by default; set differently to enable a gradient." }

    # Text hierarchy — neutral by default.
    primary:   { $type: color, $value: "#000000", $brand-owned: true, $description: "Primary text — solid black on white in the default theme." }
    secondary: { $type: color, $value: "#666666", $brand-owned: true, $description: "Secondary text — mid gray for help text, captions, metadata." }
    tertiary:  { $type: color, $value: "#bbbbbb", $brand-owned: true, $description: "Tertiary text — light gray for low-priority labels." }
    invert:    { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Invert color — white. Used on dark backgrounds." }
    disabled:  { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Disabled state color." }

    # Canvas backgrounds.
    background:           { $type: color,  $value: "#ffffff",     $brand-owned: true, $description: "Base page background — solid white." }
    background-start:     { $type: color,  $value: "#ffffff",     $brand-owned: true, $description: "Gradient background start. Same as background = no gradient (default)." }
    background-end:       { $type: color,  $value: "#ffffff",     $brand-owned: true, $description: "Gradient background end. Same as background = no gradient (default)." }
    background-direction: { $type: string, $value: "right top",   $brand-owned: true, $description: "CSS gradient direction." }

  alert:
    notify:  { bg: { $type: color, $value: "#2563eb", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }
    warning: { bg: { $type: color, $value: "#f5a623", $system-owned: true }, fg: { $type: color, $value: "#000000", $system-owned: true } }
    error:   { bg: { $type: color, $value: "#e02020", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }
    success: { bg: { $type: color, $value: "#22c55e", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }

# ── SHADER ALPHAS ─────────────────────────────────────────────────────────
shader:
  light: { $type: number, $value: 0.15, $system-owned: true, $description: "Lightest shader alpha — subtle tint." }
  base:  { $type: number, $value: 0.60, $system-owned: true }
  heavy: { $type: number, $value: 0.85, $system-owned: true }

# ── TYPOGRAPHY ────────────────────────────────────────────────────────────
typography:
  family:
    heading: { $type: fontFamily, $value: ["Inter", "sans-serif", "system-ui"], $brand-owned: true, $description: "Heading font stack. Replace with your brand's display face." }
    body:    { $type: fontFamily, $value: ["Inter", "sans-serif", "system-ui"], $brand-owned: true, $description: "Body font stack." }
    display: { $type: fontFamily, $value: ["Inter", "sans-serif", "system-ui"], $brand-owned: true, $description: "Display font stack. Defaults to heading; override for a distinct display face." }
  scale:
    base:
      size: { $type: dimension, $value: "16px", $system-owned: true, $description: "Base type size. Equivalent to 1rem." }
      lh:   { $type: dimension, $value: "24px", $system-owned: true, $description: "Base line height — anchors type rhythm." }
    h1:
      size: { $type: dimension, $value: "48px", $system-owned: true }
      lh:   { $type: number,    $value: 1,      $system-owned: true }
    h2:
      size: { $type: dimension, $value: "32px", $system-owned: true }
      lh:   { $type: dimension, $value: "40px", $system-owned: true }
    h3:
      size: { $type: dimension, $value: "24px", $system-owned: true }
      lh:   { $type: dimension, $value: "32px", $system-owned: true }
    p3:
      size: { $type: dimension, $value: "16px", $system-owned: true }
      lh:   { $type: dimension, $value: "24px", $system-owned: true }
    p4:
      size: { $type: dimension, $value: "14px", $system-owned: true }
      lh:   { $type: dimension, $value: "20px", $system-owned: true }

# ── SPACING ───────────────────────────────────────────────────────────────
space:
  unit: { $type: dimension, $value: "4px", $system-owned: true, $description: "Base unit — all spacing is a multiple of this." }
  gutter:
    base: { $type: dimension, $value: "16px",  $system-owned: true, $description: "Default gutter — column gap between layout columns." }
    sm:   { $type: dimension, $value: "24px",  $system-owned: true, $description: "≥640px." }
    md:   { $type: dimension, $value: "28px",  $system-owned: true, $description: "≥768px." }
    lg:   { $type: dimension, $value: "32px",  $system-owned: true, $description: "≥960px." }
    xl:   { $type: dimension, $value: "40px",  $system-owned: true, $description: "≥1440px." }
    2xl:  { $type: dimension, $value: "48px",  $system-owned: true, $description: "≥1600px." }
  shoulder:
    base: { $type: dimension, $value: "24px",  $system-owned: true, $description: "Page-level horizontal padding — adapts up at wider breakpoints." }
    sm:   { $type: dimension, $value: "32px",  $system-owned: true }
    md:   { $type: dimension, $value: "36px",  $system-owned: true }
    lg:   { $type: dimension, $value: "36px",  $system-owned: true }
    xl:   { $type: dimension, $value: "40px",  $system-owned: true }
    2xl:  { $type: dimension, $value: "48px",  $system-owned: true }
  article:
    base: { $type: dimension, $value: "24px",  $system-owned: true, $description: "Default padding inside cards / articles." }
    xl:   { $type: dimension, $value: "32px",  $system-owned: true }

size:
  element:
    hairline: { $type: dimension, $value: "1px",  $system-owned: true }
    min:      { $type: dimension, $value: "24px", $system-owned: true }
    base:     { $type: dimension, $value: "40px", $system-owned: true, $description: "Standard interactive height (button, input)." }

# ── RADIUS ────────────────────────────────────────────────────────────────
radius:
  brand:
    min:    { $type: dimension, $value: "4px",  $brand-owned: true, $description: "Smallest brand rounding." }
    base:   { $type: dimension, $value: "8px",  $brand-owned: true, $description: "Base element rounding." }
    corner: { $type: dimension, $value: "8px",  $brand-owned: true, $description: "Standard container rounding." }
    field:  { $type: dimension, $value: "20px", $brand-owned: true, $description: "Form input rounding." }
  full: { $type: dimension, $value: "9999px", $system-owned: true, $description: "Pill / circle radius." }
  article:
    base: { $type: dimension, $value: "16px", $system-owned: true, $description: "Article (card) rounding." }
    xl:   { $type: dimension, $value: "24px", $system-owned: true }

# ── SHADOW ────────────────────────────────────────────────────────────────
shadow:
  alpha:
    light: { $type: number, $value: 0.125, $system-owned: true, $description: "Light shadow alpha." }
    base:  { $type: number, $value: 0.25,  $system-owned: true }
    heavy: { $type: number, $value: 0.25,  $system-owned: true }

# ── BORDER ────────────────────────────────────────────────────────────────
border:
  base:   { $type: dimension, $value: "1px", $system-owned: true, $description: "Utility-only; not auto-applied." }
  button: { $type: dimension, $value: "1px", $system-owned: true }
  form:   { $type: dimension, $value: "1px", $system-owned: true }

# ── TRANSITION ────────────────────────────────────────────────────────────
transition:
  duration: { $type: duration,    $value: "300ms",            $system-owned: true, $description: "Base transition duration." }
  timing:   { $type: cubicBezier, $value: [0.4, 0, 0.2, 1],   $system-owned: true, $description: "Base transition easing." }
  property: { $type: string,      $value: "all",              $system-owned: true, $description: "Base transition property." }

# ── BREAKPOINTS ───────────────────────────────────────────────────────────
screen:
  sm:  { $type: dimension, $value: "640px",  $system-owned: true }
  md:  { $type: dimension, $value: "768px",  $system-owned: true }
  lg:  { $type: dimension, $value: "960px",  $system-owned: true }
  xl:  { $type: dimension, $value: "1440px", $system-owned: true }
  2xl: { $type: dimension, $value: "1600px", $system-owned: true }

# ── LAYOUT ────────────────────────────────────────────────────────────────
layout:
  cols:
    base: { $type: number, $value: 6,  $system-owned: true }
    sm:   { $type: number, $value: 6,  $system-owned: true }
    md:   { $type: number, $value: 6,  $system-owned: true }
    lg:   { $type: number, $value: 12, $system-owned: true }
    xl:   { $type: number, $value: 12, $system-owned: true }
    2xl:  { $type: number, $value: 12, $system-owned: true }

blur:
  light: { $type: dimension, $value: "24px",  $system-owned: true }
  base:  { $type: dimension, $value: "48px",  $system-owned: true }
  heavy: { $type: dimension, $value: "64px",  $system-owned: true }
  max:   { $type: dimension, $value: "128px", $system-owned: true }
---

# {{name}}

> **This is a starter `PRISM.md`** — a minimal, brand-neutral seed for a
> new design system. It ships with black-on-white text and a single
> placeholder accent color. Keep the structure, replace the brand colors
> and fonts and name with your own.
>
> After editing this file, run `npm run build` from your Prism Kit root
> to emit your design system into `exports/`. See [`spec/prism-md-format.md`](./spec/prism-md-format.md)
> for the full format reference.

## Overview

Replace this section with your design system's identity statement. What
is your brand? Who is the system designed for? What are its core
principles?

The starter intentionally ships neutral colors so the visual emphasis is
on *your* brand once you customize. The semantic tier ($ref pairs below)
inherits from your edits — change `color.brand.key` and every reference
to the accent color updates downstream.

## Colors

The starter defines three palettes:

- **Brand** — the identity surface. Accent color + monotone text scale.
  This is what you replace for your own brand.
- **Alert** — system-owned semantic colors (notify / warning / error /
  success). Stable across consumers; override only with strong reason.
- **Theme contexts** — semantic-tier `{bg, fg}` pairs that resolve via
  cascade depending on which `.theme.*` class wraps a section.

```token
color.theme.default.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Default theme background.
  $applied-guidance: |
    Page-level background for the base mode. Pairs with
    color.theme.default.fg per the fg/bg pairing rule. Apply via the
    cascade default at body level; only override on a section with a
    deliberate `.theme.*` class to scope a different palette.
```

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground.
  $applied-guidance: |
    Default body-text color, paired with color.theme.default.bg.
    Inherited by `<p>`, `<h1>`–`<h6>`, `<a>` defaults — don't set color
    manually on these unless overriding for a deliberate exception.
```

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Inverted theme background — used in dark mode or for high-contrast emphasis sections.
  $applied-guidance: |
    Dark-mode section bg. Apply via `.theme.invert` on a section element
    to flip a scoped subtree to high-contrast.
```

```token
color.theme.invert.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Inverted theme foreground — pairs with theme.invert.bg.
  $applied-guidance: |
    Pairs with color.theme.invert.bg for high-contrast text on dark
    surfaces. Inherits automatically when `.theme.invert` is in scope.
```

## Typography

Replace with your brand's typography rationale. What faces are used,
when, and why? What's the scale system (modular, golden ratio, custom)?
Document the rules consumers should follow — not just the values, but
when to break them.

The starter ships an Inter-based system stack at three roles
(heading / body / display) and a six-step scale aligned to a 24px line-
height anchor for consistent vertical rhythm.

## Layout

Replace with your design system's layout principles. What's the grid
column count at each breakpoint? How are page margins (shoulder),
column gaps (gutter), and card padding (article) related?

## Elevation & Depth

Replace with your elevation philosophy. The starter ships three shadow
alphas (light / base / heavy) — describe how you compose shadow
intensity and what each tier signals (resting, raised, modal).

## Shapes

Replace with your shape language. Why these radii? When does a corner
get pill-rounded vs subtly rounded vs sharp?

## Components

Document the components that depend on these tokens — buttons, cards,
inputs, navs, modals. This section is prose; the actual component
definitions live in your Tailwind preset, React components, or
equivalent.

## Modes

The starter ships a single base mode. To add additional modes (dark,
high-contrast, alt brand), append entries to the `modes:` list in
frontmatter, then author per-mode token fences here with
`$mode: <name>`:

```token
# Example — uncomment + edit when adding a dark mode:
# color.brand.background:
#   $mode: dark
#   $type: color
#   $value: "#000000"
```

Mode overlays are partial-tree redirects; they only declare what
changes. Tokens not redeclared inherit the base value.

## Do's and Don'ts

### Do

- **Edit `color.brand.*` first** — your brand identity lives there
- **Pair every `*.bg` with `*.fg`** — the lint enforces it (Rule 8)
- **Use `$ref` for semantic-tier tokens** — point at brand primitives so
  forks propagate
- **Document the *why*** — `$applied-guidance` is for tool consumers
  (AI agents, design-tool integrators); write it for them

### Don't

- **Don't add new top-level groups** without updating
  `brand|systemOwnedRoots` in frontmatter (Rule 21 will flag conflicts)
- **Don't duplicate values** — use `$ref` to alias. Duplication breaks
  the rebrand flow when consumers change a single primitive
- **Don't strip `$description`** — Layer-1 lint Rule 18 warns on thin
  descriptions for a reason: tokens without context are landmines for
  consumers six months from now
