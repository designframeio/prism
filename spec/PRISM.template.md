<!--
================================================================================
PRISM.md TEMPLATE — Starter for new design systems
================================================================================

Copy this file to your project root as `PRISM.md` (or `<project>.PRISM.md` for
namespaced variants), then replace every CHANGEME placeholder with your project's
values. Delete any sections / tokens you don't need; the canonical 11-section
H2 order is preserved if you omit sections (just don't reorder).

Required edits before first emit:
  1. Replace CHANGEME-NAME with your project's kebab-case name
  2. Replace CHANGEME color hex values with your brand palette
  3. Replace CHANGEME font-family identifiers with your @font-face names
  4. Write your Overview section prose (designer voice / philosophy)
  5. Optional: remove modes you don't need (delete from `modes:` + delete their
     ## Modes sub-section); only `default` (base: true) is required.

First-time author? Start with: ./designer-guide.md (warm-tone walkthrough)
Spec reference: ../spec/prism-md-format.md (`prism-spec/1.0` canonical contract)
Worked example: ../PRISM.md (Designframe's own design system)

Once edited, parse + lint + emit with:
  prism parse PRISM.md             # validate + emit tokens.json bundle
  prism lint PRISM.md              # 14 PRISM.md rules + 7 bundle rules
  prism emit tailwind-preset       # produce tailwind-preset.js
  prism emit design-md             # produce DESIGN.md
  prism emit df-input.css          # produce Designframe-runtime CSS

Delete this comment block and all <!-- ... --> commentary throughout once you've
read them. They don't affect rendering or parsing but reduce file size.
================================================================================
-->

---
# ====================================================================
# FORMAT IDENTITY (required — do not edit these 3 fields)
# ====================================================================
format: prism-spec/1.0
prismMdVersion: 1.0

# ====================================================================
# PROJECT METADATA (replace CHANGEME values)
# ====================================================================
name: CHANGEME-project-name              # kebab-case, no spaces, no special chars
version: 0.1.0                            # semver
description: |
  CHANGEME — one-paragraph description of what this design system is for,
  who its audience is, and what makes it distinctive.
license: MIT                              # SPDX identifier or "proprietary"
homepage: https://CHANGEME.example.com    # public URL; or remove if private

# Set to true if this PRISM.md should also parse as a Google DESIGN.md.
# Default true — the format is a strict superset; only set false if you
# deliberately want to use a PRISM.md feature that breaks DESIGN.md compat.
designMdCompatible: true

# If targeting a specific Designframe version, declare it here. Otherwise omit.
# designframeVersion: 0.99

# ====================================================================
# BRAND / SYSTEM OWNERSHIP ROOTS
# ====================================================================
# These paths determine the default $brand-owned / $system-owned flag for
# tokens under each root. Individual tokens can override via explicit
# $brand-owned: true/false in their definition.
#
# Brand-owned tokens are intended to be REPLACED when consumers fork your
# design system. System-owned tokens are stable across all consumers.
# ====================================================================
brandOwnedRoots:
  - color.brand          # Brand colors (key, primary, secondary, etc.)
  - color.brand-alt      # Optional alt palette (delete if unused)
  - typography.family    # Font assignments
  - radius.brand         # Rounding scale set by brand identity

systemOwnedRoots:
  - screen               # Breakpoints — usually inherited from DF
  - layout               # Grid system
  - space                # Semantic spacing contexts
  - shadow               # Elevation system
  - shader               # Shader alpha values
  - typography.scale     # Type scale (sizes + line heights)
  - color.alert          # Status/alert colors

# ====================================================================
# MODES (delete any modes you don't need; `default` with base: true required)
# ====================================================================
modes:
  - name: default
    description: Light mode + main brand palette
    base: true                            # exactly one mode MUST have base: true

  # Delete the next three if you don't need them.
  # `aliases:` lets a single mode definition serve multiple consumer-vocab names.

  - name: dark
    description: Dark mode — bg flips to primary, fg to invert
    aliases: [invert]                     # DF-native synonym for "dark"

  - name: alt
    description: Alt palette — secondary brand identity

  - name: key
    description: Saturated key brand color as section bg

# ====================================================================
# SOURCE POINTERS (only relevant if bootstrapping from a Designframe project)
# ====================================================================
# Delete this section if authoring from scratch (no DF source to point at).
# Set by `prism import` automatically when bootstrapping; hand-edit only if
# the source files move.
#
# source:
#   dfInputCss: src/assets/designframe/df-input.css
#   tailwindConfig: tailwind.config.js
#   dfPreset: src/assets/designframe/df-preset.js

# ====================================================================
# ATOMIC PRIMITIVE TOKENS
# ====================================================================
# Tokens in this section live in frontmatter (per spec §6.5 decision rule):
#   - single $value literal OR single $ref reference
#   - $description (if any) ≤ 200 chars
#   - no $applied-guidance, no $ai-hint
#   - not a composite type (shadow / typography / border / transition / gradient)
#
# Tokens needing prose rationale, AI hints, or applied-guidance go in the
# body sections below as ```token``` code-fence blocks.
# ====================================================================

color:
  # ------------------------------------------------------------------
  # BRAND PALETTE — replace every CHANGEME hex with your brand colors.
  # If your brand has no gradient, set key == key-end (both hex equal).
  # ------------------------------------------------------------------
  brand:
    key:        { $type: color, $value: "#0066cc", $brand-owned: true, $description: "Brand key color (gradient start). CHANGEME." }
    key-end:    { $type: color, $value: "#0066cc", $brand-owned: true, $description: "Brand key gradient end. Same as key if no gradient. CHANGEME." }
    primary:    { $type: color, $value: "#0c0c0c", $brand-owned: true, $description: "Primary text — darkest monotone." }
    secondary:  { $type: color, $value: "#666666", $brand-owned: true, $description: "Secondary text — mid gray for help text." }
    tertiary:   { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Tertiary text — light gray, still legible." }
    invert:     { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Invert color — white. Used on dark backgrounds." }
    disabled:   { $type: color, $value: "#cccccc", $brand-owned: true }
    background: { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Base page background." }

  # ------------------------------------------------------------------
  # ALT PALETTE — delete entire color.brand-alt block if not using alt mode.
  # ------------------------------------------------------------------
  brand-alt:
    key:        { $type: color, $value: "#cc6600", $brand-owned: true, $description: "Alt brand key (CHANGEME or remove brand-alt entirely)." }
    key-end:    { $type: color, $value: "#cc6600", $brand-owned: true }
    primary:    { $type: color, $value: "#0c0c0c", $brand-owned: true }
    secondary:  { $type: color, $value: "#666666", $brand-owned: true }
    tertiary:   { $type: color, $value: "#cccccc", $brand-owned: true }
    invert:     { $type: color, $value: "#ffffff", $brand-owned: true }
    background: { $type: color, $value: "#ffffff", $brand-owned: true }

  # ------------------------------------------------------------------
  # ALERT COLORS — system-owned defaults; usually keep as-is.
  # Each level MUST have both bg and fg per the fg/bg pairing rule.
  # ------------------------------------------------------------------
  alert:
    notify:  { bg: { $type: color, $value: "#3883ff", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }
    warning: { bg: { $type: color, $value: "#faa002", $system-owned: true }, fg: { $type: color, $value: "#0c0c0c", $system-owned: true } }
    error:   { bg: { $type: color, $value: "#fa0002", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }
    success: { bg: { $type: color, $value: "#22c55e", $system-owned: true }, fg: { $type: color, $value: "#ffffff", $system-owned: true } }

# ====================================================================
# SHADER SYSTEM (alpha values for opacity-modulated brand colors)
# ====================================================================
# These are DF-conventional defaults. Change only if your design specifies
# different shader weights. NOTE: bare `shader` = base (0.60), NOT light.
# ====================================================================
shader:
  light: { $type: number, $value: 0.15, $system-owned: true }
  base:  { $type: number, $value: 0.60, $system-owned: true }
  heavy: { $type: number, $value: 0.85, $system-owned: true }

# ====================================================================
# TYPOGRAPHY
# ====================================================================
# Family identifiers must match your @font-face declarations. Replace
# CHANGEMEFontFamily with your actual @font-face names.
# ====================================================================
typography:
  family:
    heading: { $type: fontFamily, $value: ["CHANGEMEHeadingFontFamily", "sans-serif", "system-ui"], $brand-owned: true }
    body:    { $type: fontFamily, $value: ["CHANGEMEBodyFontFamily", "sans-serif", "system-ui"], $brand-owned: true }
    display: { $type: fontFamily, $value: ["CHANGEMEDisplayFontFamily", "sans-serif", "system-ui"], $brand-owned: true }

  # Type scale — DF's modular scale anchored at 16px/24px base.
  # Change only if your brand needs a different rhythm; recompute every
  # paired line-height when you change size (spec §11 + df-rules.md).
  scale:
    base: { size: { $type: dimension, $value: "16px", $system-owned: true }, lh: { $type: dimension, $value: "24px", $system-owned: true } }
    d1: { size: { $type: dimension, $value: "96px", $system-owned: true }, lh: { $type: number, $value: 1, $system-owned: true } }
    d2: { size: { $type: dimension, $value: "72px", $system-owned: true }, lh: { $type: number, $value: 1, $system-owned: true } }
    d3: { size: { $type: dimension, $value: "60px", $system-owned: true }, lh: { $type: number, $value: 1, $system-owned: true } }
    h1: { size: { $type: dimension, $value: "48px", $system-owned: true }, lh: { $type: number, $value: 1, $system-owned: true } }
    h2: { size: { $type: dimension, $value: "32px", $system-owned: true }, lh: { $type: dimension, $value: "40px", $system-owned: true } }
    h3: { size: { $type: dimension, $value: "24px", $system-owned: true }, lh: { $type: dimension, $value: "32px", $system-owned: true } }
    h4: { size: { $type: dimension, $value: "20px", $system-owned: true }, lh: { $type: dimension, $value: "28px", $system-owned: true } }
    h5: { size: { $type: dimension, $value: "18px", $system-owned: true }, lh: { $type: dimension, $value: "28px", $system-owned: true } }
    h6: { size: { $type: dimension, $value: "16px", $system-owned: true }, lh: { $type: dimension, $value: "24px", $system-owned: true } }
    p1: { size: { $type: dimension, $value: "20px", $system-owned: true }, lh: { $type: dimension, $value: "28px", $system-owned: true } }
    p2: { size: { $type: dimension, $value: "18px", $system-owned: true }, lh: { $type: dimension, $value: "28px", $system-owned: true } }
    p3: { size: { $type: dimension, $value: "16px", $system-owned: true }, lh: { $type: dimension, $value: "24px", $system-owned: true } }
    p4: { size: { $type: dimension, $value: "14px", $system-owned: true }, lh: { $type: dimension, $value: "20px", $system-owned: true } }
    p5: { size: { $type: dimension, $value: "12px", $system-owned: true }, lh: { $type: dimension, $value: "16px", $system-owned: true } }

# ====================================================================
# SPACING — semantic contexts (DF philosophy: NO T-shirt scales)
# ====================================================================
# Each scale shows base + xl breakpoints; populate base + sm + md + lg +
# xl + 2xl for full responsive coverage.
# ====================================================================
space:
  unit: { $type: dimension, $value: "4px", $system-owned: true, $description: "Base layout unit; all spacing derives from this." }
  gutter:
    base: { $type: dimension, $value: "24px", $system-owned: true }
    xl:   { $type: dimension, $value: "32px", $system-owned: true }
  shoulder:
    base: { $type: dimension, $value: "24px", $system-owned: true }
    xl:   { $type: dimension, $value: "40px", $system-owned: true }
  article: { base: { $type: dimension, $value: "16px", $system-owned: true }, xl: { $type: dimension, $value: "24px", $system-owned: true } }
  element: { base: { $type: dimension, $value: "16px", $system-owned: true } }
  sub:     { base: { $type: dimension, $value: "8px",  $system-owned: true }, xl: { $type: dimension, $value: "12px", $system-owned: true } }

# ====================================================================
# RADIUS
# ====================================================================
radius:
  brand:
    min:    { $type: dimension, $value: "4px",  $brand-owned: true }
    base:   { $type: dimension, $value: "8px",  $brand-owned: true }
    corner: { $type: dimension, $value: "8px",  $brand-owned: true }
    field:  { $type: dimension, $value: "20px", $brand-owned: true }
  full: { $type: dimension, $value: "9999px", $system-owned: true }

# ====================================================================
# BREAKPOINTS
# ====================================================================
# These are DF defaults; change only if your responsive grid differs.
# Values are hardcoded both here and in any df-input.css @media queries
# (CSS custom properties cannot be used in @media queries).
# ====================================================================
screen:
  sm:  { $type: dimension, $value: "640px",  $system-owned: true }
  md:  { $type: dimension, $value: "768px",  $system-owned: true }
  lg:  { $type: dimension, $value: "960px",  $system-owned: true }
  xl:  { $type: dimension, $value: "1440px", $system-owned: true }
  2xl: { $type: dimension, $value: "1600px", $system-owned: true }
---

<!--
================================================================================
BODY SECTIONS — fill in prose + token fences here
================================================================================
The H2 sections below MUST appear in canonical order (spec §7). You can
OMIT any section, but you can't REORDER them. Aliases accepted:
  - "## Overview" or "## Brand & Style"
  - "## Layout" or "## Layout & Spacing"
  - "## Elevation & Depth" or "## Elevation"

3 sections are PRISM.md additions beyond DESIGN.md:
  - "## Modes" — mode-overlay definitions
  - "## Components — Premium" — DF-specific premium components
  - "## Lossiness & Constraints" — declared lossiness per emit target

Delete sections you don't need; don't reorder what remains.
================================================================================
-->

# CHANGEME — Design System Name

CHANGEME — one-paragraph elevator pitch. What is this design system? Who is it for?

## Overview

<!-- Write your design philosophy here. -->

CHANGEME-OVERVIEW. Cover: who designed this, who it's for, what makes it distinctive, what its core conceptual goals are.

## Colors

<!-- Atomic brand colors live in frontmatter; semantic theme contexts + tokens
     with applied-guidance live below as ```token``` fences. Add as needed. -->

CHANGEME — paragraph describing your color philosophy.

### Theme contexts

```token
color.theme.default.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Default theme background.
  $applied-guidance: |
    CHANGEME — applied guidance for this token. When to use, when not to,
    paired-token boundary. Default body-bg, paired with color.theme.default.fg.
```

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground (body text on canvas).
  $applied-guidance: |
    CHANGEME — applied guidance. Default body-text color, inherited by p / h1-h6 / a.
```

<!-- Add color.theme.invert.{bg,fg}, color.theme.key.{bg,fg} etc as needed. -->

## Typography

CHANGEME — paragraph describing your type philosophy. What anchors the scale? What's the line-height rhythm?

## Layout

CHANGEME — describe your grid system, semantic spacing contexts, block hierarchy.

## Elevation & Depth

CHANGEME — describe your shadow tiers and elevation rules.

## Shapes

CHANGEME — describe your border-radius system and modifier combinations.

## Components

CHANGEME — describe component patterns. If using DF combo-class pattern, explain it here.

## Modes

<!-- Define each mode declared in frontmatter modes[] except the base mode.
     Delete sub-sections for modes you removed from frontmatter. -->

### dark

CHANGEME — describe dark mode philosophy.

```token
color.theme.default.bg:
  $ref: "{color.theme.invert.bg}"
  $mode: dark
```

```token
color.theme.default.fg:
  $ref: "{color.theme.invert.fg}"
  $mode: dark
```

### alt

CHANGEME — describe alt-palette mode philosophy.

```token
color.theme.default.bg:
  $ref: "{color.brand-alt.background}"
  $mode: alt
```

```token
color.theme.default.fg:
  $ref: "{color.brand-alt.primary}"
  $mode: alt
```

### key

CHANGEME — describe key-saturated mode philosophy.

```token
color.theme.default.bg:
  $ref: "{color.theme.key.bg}"
  $mode: key
```

```token
color.theme.default.fg:
  $ref: "{color.theme.key.fg}"
  $mode: key
```

## Components — Premium

<!-- DF-specific df-* premium components. Delete this section if your design
     system doesn't have a free/premium tier split. -->

CHANGEME or delete this entire section.

## Lossiness & Constraints

| Target | Lossy fields | Why |
|---|---|---|
| `df-input.css` | `$ai-hint`, `$applied-guidance`, full rationale prose | CSS has no carrier; Tailwind/PostCSS strips comments at build |
| `design-md.md` | `$ai-hint`, DF-native names (mapped to MD3 aliases), 3 PRISM.md-only sections (Modes, Components — Premium, Lossiness & Constraints) | DESIGN.md format has no carrier for these |
| `tailwind-preset.js` | All prose; all `$brand-owned` / `$system-owned` flags | Tailwind preset is value-only |
| `tokens.css` | All prose; all metadata | Pure CSS custom property declarations |
| `figma-tokens-studio.json` | `$ai-hint`, `$applied-guidance` (emitted to DTCG `$extensions["prism.*"]` which Tokens Studio ignores) | Tokens Studio reads core DTCG only |
| `tokens.json` (DTCG bundle) | None — PRISM.md → DTCG is lossless | PRISM.md extensions emit to DTCG `$extensions["prism.*"]` |

## Do's and Don'ts

<!-- Cross-cutting rules that can't be token-encoded.
     Inherit DF's rules below or replace with your own conventions. -->

### Do

- Use design tokens via class utilities (e.g. `text-primary`, `bg-key`) rather than hardcoded values
- Apply theme classes (`.theme.invert`, `.theme.key`) at the section level
- Build pages outside-in: html > body > header / main > section > blocks > content
- Set spacing on parent elements, not on individual children
- Use semantic HTML directly

### Don't

- Don't hardcode color values; use tokens
- Don't put `block-*` classes on semantic content elements
- Don't override theme contexts per-component
- Don't use uppercase shorthand classes
- Don't manually flip component colors in themed sections — let the cascade handle it

<!--
================================================================================
END OF TEMPLATE
================================================================================
After replacing all CHANGEME values and writing prose, delete every
<!-- ... --> comment block in this file (they're optional once you understand
the structure). Then run:

  prism lint PRISM.md           # validate
  prism parse PRISM.md          # emit DTCG bundle
  prism emit <target>           # emit consumer-specific artifacts

Questions? See ../spec/prism-md-format.md for full format specification.
================================================================================
-->
