---
format: prism-spec/1.0
name: designframe
version: 0.99.0
prismMdVersion: 1.0
description: |
  Designframe — minimalist adaptive design system by Mike Prasad.
  Built as a Tailwind CSS preset/extension; pure CSS output.
license: Apache-2.0
homepage: https://designframe.io
designframeVersion: 0.99
designMdCompatible: true

brandOwnedRoots:
  - color.brand
  - color.brand-alt
  - color.theme.header
  - color.theme.footer
  - typography.family
  - radius.brand

systemOwnedRoots:
  - screen
  - layout
  - space
  - size
  - shadow
  - shader
  - blur
  - border
  - transition
  - typography.scale
  - color.alert

modes:
  - name: default
    description: Light mode + main brand palette (cascade root)
    base: true
  - name: dark
    description: Dark mode — bg flips to primary (near-black), fg to invert (white)
    aliases: [ invert ]
  - name: alt
    description: Alt palette — secondary brand identity (e.g. campaign mode)
  - name: key
    description: Key brand color as canvas — saturated section bg with component key↔invert swap

source:
  dfInputCss: ../../../df-ui/df-working/src/assets/designframe/df-input.css
  tailwindConfig: ../../../df-ui/df-working/tailwind.config.js
  dfPreset: ../../../df-ui/df-working/src/assets/designframe/df-preset.js
  rationaleSources:
    - ../../../df-ui/df-working/docs/df-rules.md
    - ../../../df-ui/df-working/docs/df-knowledge.md
    - ../../../df-ui/df-working/docs/taxonomy.md
    - ../../../df-ui/df-working/src/assets/designframe/configuration.md
    - ../../../df-ui/df-working/src/assets/designframe/theming.md

color:
  brand:
    key: { $type: color, $value: "#fb03b9", $brand-owned: true, $df-source: --qs-color-key, $description: "Brand key color (gradient start)." }
    key-end: { $type: color, $value: "#3883ff", $brand-owned: true, $df-source: --qs-color-key-end, $description: "Brand key gradient end-stop." }
    primary: { $type: color, $value: "#0c0c0c", $brand-owned: true, $df-source: --qs-color-primary, $description: "Primary text — darkest monotone." }
    secondary: { $type: color, $value: "#888888", $brand-owned: true, $df-source: --qs-color-secondary, $description: "Secondary text — mid gray for help text." }
    tertiary: { $type: color, $value: "#cccccc", $brand-owned: true, $df-source: --qs-color-tertiary, $description: "Tertiary text — light gray, still legible." }
    invert: { $type: color, $value: "#ffffff", $brand-owned: true, $df-source: --qs-color-invert, $description: "Invert color — white. Used on dark backgrounds." }
    disabled: { $type: color, $value: "#cccccc", $brand-owned: true, $description: DF disabled state color. Distinct from color.brand.tertiary semantically but may share value., $df-source: --qs-color-disabled }
    background: { $type: color, $value: "#ffffff", $brand-owned: true, $df-source: --qs-color-background, $description: "Base page background." }
    background-start: { $type: color, $value: "#ffe6f8", $brand-owned: true, $description: DF gradient background start (optional). Set same as color.brand.background if no gradient bg., $df-source: --qs-color-background-start }
    background-end: { $type: color, $value: "#ebf3ff", $brand-owned: true, $description: DF gradient background end (optional)., $df-source: --qs-color-background-end }
    background-direction: { $type: string, $value: "right top", $brand-owned: true, $df-source: --qs-color-background-direction, $description: "CSS gradient direction for background-start → background-end pair." }

  brand-alt:
    key: { $type: color, $value: "#fa0002", $brand-owned: true, $description: Alt brand key color., $df-source: --qs-color-key-alt }
    key-end: { $type: color, $value: "#faa002", $brand-owned: true, $description: Alt brand key gradient end., $df-source: --qs-color-key-end-alt }
    primary: { $type: color, $value: "#0c0c0c", $brand-owned: true, $description: Alt theme primary., $df-source: --qs-color-primary-alt }
    secondary: { $type: color, $value: "#888888", $brand-owned: true, $description: Alt theme secondary., $df-source: --qs-color-secondary-alt }
    tertiary: { $type: color, $value: "#cccccc", $brand-owned: true, $description: Alt theme tertiary., $df-source: --qs-color-tertiary-alt }
    invert: { $type: color, $value: "#ffffff", $brand-owned: true, $description: Alt theme invert., $df-source: --qs-color-invert-alt }
    disabled: { $type: color, $value: "#cccccc", $brand-owned: true, $description: Alt theme disabled., $df-source: --qs-color-disabled-alt }
    background: { $type: color, $value: "#ffffff", $brand-owned: true, $description: Alt theme background., $df-source: --qs-color-background-alt }
    background-start: { $type: color, $value: "#ff9f1c", $brand-owned: true, $description: Alt gradient bg start., $df-source: --qs-color-background-start-alt }
    background-end: { $type: color, $value: "#ffd166", $brand-owned: true, $description: Alt gradient bg end., $df-source: --qs-color-background-end-alt }

  alert:
    notify:
      bg: { $type: color, $value: "#3883ff", $system-owned: true, $description: Notify alert background. Pairs with alert.notify.fg., $df-source: --color-alert-notify-background }
      fg: { $type: color, $value: "#ffffff", $system-owned: true, $description: Notify alert foreground. Paired with alert.notify.bg. }
    warning:
      bg: { $type: color, $value: "#faa002", $system-owned: true, $description: "Warning alert background. Pairs with alert.warning.fg. Note: alert.warning hex placeholder; verify against df-preset.js at full extraction.", $df-source: --color-alert-warning-background }
      fg: { $type: color, $value: "#0c0c0c", $system-owned: true, $description: Warning alert foreground. Paired with alert.warning.bg. }
    error:
      bg: { $type: color, $value: "#fa0002", $system-owned: true, $df-source: --color-alert-error-background }
      fg: { $type: color, $value: "#ffffff", $system-owned: true, $description: Error alert foreground. }
    success:
      bg: { $type: color, $value: "#22c55e", $system-owned: true, $description: Success alert background. Pairs with alert.success.fg. Placeholder hex pending df-preset.js extraction., $df-source: --color-alert-success-background }
      fg: { $type: color, $value: "#ffffff", $system-owned: true, $description: Success alert foreground. }

  _aliases:
    primary: { $ref: "{color.brand.key}" }
    primary-end: { $ref: "{color.brand.key-end}" }
    fg-default: { $ref: "{color.brand.primary}" }
    fg-muted: { $ref: "{color.brand.secondary}" }
    fg-subtle: { $ref: "{color.brand.tertiary}" }
    fg-on-dark: { $ref: "{color.brand.invert}" }
    bg-canvas: { $ref: "{color.brand.background}" }

  fg:
    default: { $ref: "{color.theme.default.fg}" }
    muted: { $ref: "{color.brand.secondary}" }
    subtle: { $ref: "{color.brand.tertiary}" }
    on-key: { $ref: "{color.theme.key.fg}" }
    on-dark: { $ref: "{color.theme.invert.fg}" }
    disabled: { $ref: "{color.brand.disabled}" }

  bg:
    canvas: { $ref: "{color.theme.default.bg}" }
    key: { $ref: "{color.theme.key.bg}" }
    key-start: { $ref: "{color.theme.key-gradient.bg-start}" }
    key-end: { $ref: "{color.theme.key-gradient.bg-end}" }
    invert: { $ref: "{color.theme.invert.bg}" }

shader:
  light: { $type: number, $value: 0.15, $system-owned: true, $description: Lightest shader alpha., $df-source: --shader-light }
  base: { $type: number, $value: 0.60, $system-owned: true, $df-source: --shader-base, $description: "Bare 'shader' = this value (naming asymmetry — see Do's and Don'ts)." }
  heavy: { $type: number, $value: 0.85, $system-owned: true, $description: Heavy shader alpha., $df-source: --shader-heavy }

typography:
  family:
    heading: { $type: fontFamily, $value: [ "headingFontFamily", "sans-serif", "system-ui" ], $brand-owned: true, $description: "Heading font stack. Source: --font-heading + --font-heading-alt1/alt2. Replace headingFontFamily token at @font-face when forking.", $df-source: --font-heading }
    body: { $type: fontFamily, $value: [ "bodyFontFamily", "sans-serif", "system-ui" ], $brand-owned: true, $description: "Body font stack. Source: --font-body + alt1/alt2.", $df-source: --font-body }
    display: { $type: fontFamily, $value: [ "displayFontFamily", "sans-serif", "system-ui" ], $brand-owned: true, $description: Display font stack. Same as heading by DF convention; carve out if brand wants distinct display face., $df-source: --font-display }
  scale:
    base: { size: { $type: dimension, $value: "16px", $system-owned: true, $description: Base type size. Equivalent to 1rem and to text-p3., $df-source: --text-base-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $description: Base line height. Anchor for all DF type rhythm. See df-rules.md §lineHeight-anchor., $df-source: --text-base-lh } }
    d1: { size: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --text-d1-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d1-lh } }
    d2: { size: { $type: dimension, $value: "72px", $system-owned: true, $df-source: --text-d2-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d2-lh } }
    d3: { size: { $type: dimension, $value: "60px", $system-owned: true, $df-source: --text-d3-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d3-lh } }
    h1: { size: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --text-h1-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-h1-lh } }
    h2: { size: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --text-h2-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-h2-lh } }
    h3: { size: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-h3-size }, lh: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --text-h3-lh } }
    h4: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-h4-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-h4-lh } }
    h5: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-h5-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-h5-lh } }
    h6: { size: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-h6-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-h6-lh } }
    p1: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-p1-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-p1-lh } }
    p2: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-p2-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-p2-lh } }
    p3: { size: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-p3-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-p3-lh } }
    p4: { size: { $type: dimension, $value: "14px", $system-owned: true, $df-source: --text-p4-size }, lh: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-p4-lh } }
    p5: { size: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --text-p5-size }, lh: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-p5-lh } }
    nav: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-nav-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-nav-lh } }
    button: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-button-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-button-lh, $description: "Inherits text-nav-lh." } }
    chip: { size: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --text-chip-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-chip-lh } }

space:
  unit: { $type: dimension, $value: "4px", $system-owned: true, $df-source: --layout-unit, $description: "Base layout unit. All spacing multiples derive from this." }
  gutter:
    base: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --spacing-gutter-base }
    sm: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥640px., $df-source: --spacing-gutter-sm }
    md: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥768px., $df-source: --spacing-gutter-md }
    lg: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥960px., $df-source: --spacing-gutter-lg }
    xl: { $type: dimension, $value: "32px", $system-owned: true, $description: ≥1440px., $df-source: --spacing-gutter-xl }
    2xl: { $type: dimension, $value: "32px", $system-owned: true, $description: ≥1600px., $df-source: --spacing-gutter-2xl }
  shoulder:
    base: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --spacing-shoulder-base }
    sm: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --spacing-shoulder-sm }
    md: { $type: dimension, $value: "36px", $system-owned: true, $df-source: --spacing-shoulder-md }
    lg: { $type: dimension, $value: "36px", $system-owned: true, $df-source: --spacing-shoulder-lg }
    xl: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --spacing-shoulder-xl }
    2xl: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --spacing-shoulder-2xl }
  article: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-article-base }, xl: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --pad-article-xl } }
  element: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-element-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-element-xl } }
  section: { base: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --pad-section-base }, xl: { $type: dimension, $value: "64px", $system-owned: true, $df-source: --pad-section-xl } }
  header: { base: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --pad-header-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-header-xl } }
  stack: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-stack-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-stack-xl } }
  form: { base: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --gap-form-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-form-xl } }
  sub: { base: { $type: dimension, $value: "8px", $system-owned: true, $df-source: --spacing-sub-base }, xl: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --spacing-sub-xl } }
  hgroup:
    display: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-hgroup-display, $description: "HGROUP gap when display-level." }
    heading: { $type: dimension, $value: "8px", $system-owned: true, $df-source: --gap-hgroup-heading }
    title: { $type: dimension, $value: "4px", $system-owned: true, $df-source: --gap-hgroup-title }
  _aliases:
    md: { $ref: "{space.gutter.base}" }
    sm: { $ref: "{space.article.base}" }
    xs: { $ref: "{space.sub.base}" }
    2xs: { $ref: "{space.unit}" }

size:
  element:
    hairline: { $type: dimension, $value: "1px", $system-owned: true, $description: Hairline border/divider thickness., $df-source: --element-hairline-size }
    min: { $type: dimension, $value: "24px", $system-owned: true, $description: Smallest element height. Matches text-base-lh., $df-source: --element-min-size }
    sub: { $type: dimension, $value: "16px", $system-owned: true, $description: Sub-element height., $df-source: --element-sub-size }
    compact: { $type: dimension, $value: "32px", $system-owned: true, $description: Compact interactive height., $df-source: --element-compact-size }
    base: { $type: dimension, $value: "40px", $system-owned: true, $description: "Standard interactive height (button, input).", $df-source: --element-base-size }

radius:
  brand:
    min: { $type: dimension, $value: "4px", $brand-owned: true, $description: Smallest brand rounding., $df-source: --qs-rounded-min }
    base: { $type: dimension, $value: "8px", $brand-owned: true, $description: Base element rounding., $df-source: --qs-rounded-base }
    corner: { $type: dimension, $value: "8px", $brand-owned: true, $description: Standard container rounding., $df-source: --qs-rounded-corner }
    field: { $type: dimension, $value: "20px", $brand-owned: true, $description: Form input rounding., $df-source: --qs-rounded-field }
  full: { $type: dimension, $value: "9999px", $system-owned: true, $description: Pill/circle radius., $df-source: --rounded-full }
  chip: { $ref: "{radius.brand.min}" }
  article:
    base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --rounded-article-base, $description: "Article (card) rounding. Matches pad-article per breakpoint." }
    xl: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --rounded-article-xl }

shadow:
  alpha:
    light: { $type: number, $value: 0.125, $system-owned: true, $description: Light shadow alpha., $df-source: --shadow-alpha-light }
    base: { $type: number, $value: 0.25, $system-owned: true, $description: Base shadow alpha., $df-source: --shadow-alpha-base }
    heavy: { $type: number, $value: 0.25, $system-owned: true, $df-source: --shadow-alpha-heavy, $description: "Same numeric value as base; offset/blur do the differentiation." }

blur:
  light: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --blur-light }
  base: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --blur-base }
  heavy: { $type: dimension, $value: "64px", $system-owned: true, $df-source: --blur-heavy }
  max: { $type: dimension, $value: "128px", $system-owned: true, $df-source: --blur-max }

border:
  base: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-base, $description: "Utility-only; not auto-applied." }
  button: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-button }
  form: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-form }

transition:
  duration: { $type: duration, $value: "300ms", $system-owned: true, $description: Base transition duration., $df-source: --transition-duration-base }
  timing: { $type: cubicBezier, $value: [ 0.4, 0, 0.2, 1 ], $system-owned: true, $description: Base transition easing., $df-source: --transition-timing-base }
  property: { $type: string, $value: "all", $system-owned: true, $description: Base transition property., $df-source: --transition-property-base }

layout:
  cols:
    base: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-base }
    sm: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-sm }
    md: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-md }
    lg: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-lg }
    xl: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-xl }
    2xl: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-2xl }
  col-width:
    base: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --layout-col-width-base }
    sm: { $type: dimension, $value: "76px", $system-owned: true, $df-source: --layout-col-width-sm }
    md: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --layout-col-width-md }
    lg: { $type: dimension, $value: "52px", $system-owned: true, $df-source: --layout-col-width-lg }
    xl: { $type: dimension, $value: "84px", $system-owned: true, $df-source: --layout-col-width-xl }
    2xl: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --layout-col-width-2xl }

screen:
  sm: { $type: dimension, $value: "640px", $system-owned: true, $df-source: --screen-sm }
  md: { $type: dimension, $value: "768px", $system-owned: true, $df-source: --screen-md }
  lg: { $type: dimension, $value: "960px", $system-owned: true, $df-source: --screen-lg }
  xl: { $type: dimension, $value: "1440px", $system-owned: true, $df-source: --screen-xl }
  2xl: { $type: dimension, $value: "1600px", $system-owned: true, $df-source: --screen-2xl }
---

# Designframe

A minimalist adaptive design system built as a Tailwind CSS preset. Modular grid + typographic scale + structured color + consistent spacing, with pure CSS output.

## Overview

**Designframe is the D in MVCD** — a Display layer separated from Model / View / Controller, designed so engineers don't have to make design decisions and end users can customize UI without designer or engineer skills.

Two audiences, one solution: math + structural defaults that propagate intent without breaking layout.

### Core conceptual goals

- **Minimalist** — no choice that isn't necessary
- **Adaptive** — survives across every viewport without manual tuning
- **Semantic** — HTML elements carry meaning; classes never override that
- **Resilient** — composed by AI, junior engineers, and non-designers without producing broken layouts

### Customization layers

| Layer | Audience | Surface | Examples |
|---|---|---|---|
| Brand-configurable | Brand owner (set once) | Frontmatter `color.brand.*`, `typography.family.*`, `radius.brand.*` | Replace `key`, `primary`, font assignments, rounding |
| Engineer-immutable | Engineers, AI implementers | Consume semantic tokens; never redefine | `color.theme.default.fg`, `space.gutter.base`, all `space.*` |
| End-user-tunable | End users (e.g. space owners) | Configurator UI, not direct token edits | Intent-level controls translated through DF math |

The default answer to "should we expose this as configurable?" is **no** — unless removing the choice would prevent a legitimate use case.

## Colors

Three palettes coexist in Designframe: **brand** (main identity), **brand-alt** (optional alternate identity for campaign mode), and **alert** (system-owned semantic colors for notify/warning/error/success). Theme contexts (semantic-tier) compose primitives into role-based pairs that switch with the active mode.

### Brand palette

The brand identity is a **two-stop gradient** — `color.brand.key` (pink) to `color.brand.key-end` (blue) — carrying through buttons, callouts, link hovers, and key-themed sections. Solo applications of either stop are rare; the gradient is the visual signature.

```token
color.brand.key:
  $type: color
  $value: "#fb03b9"
  $brand-owned: true
  $df-source: --qs-color-key
  $description: Brand key color — gradient start. Pink, saturated.
  $applied-guidance: |
    Use as the brand-identity accent — button-gradient start, key-tinted backgrounds, link-hover via `.text-key`. When key is intended to be a gradient (the default), color.brand.key-end is the paired end-stop. Don't apply key as a solid-fill brand color unless the design specifically calls for it; default to the gradient pair.
  $ai-hint: |
    When generating brand-key UI elements, prefer gradient utilities (bg-gradient-key, button.gradient) over solid bg-key + text-key in isolation. The gradient is the brand; solid-key applications can look stylistically off without surrounding gradient context.
```

```token
color.brand.key-end:
  $type: color
  $value: "#3883ff"
  $brand-owned: true
  $df-source: --qs-color-key-end
  $description: Brand key gradient end-stop. Blue.
  $applied-guidance: |
    Paired with color.brand.key in every gradient — set them equal for a solid-key brand. Direction is controlled by color.brand.background-direction at theme level (default: right top).
```

```token
color.brand.primary:
  $type: color
  $value: "#0c0c0c"
  $brand-owned: true
  $df-source: --qs-color-primary
  $description: Primary text — darkest monotone.
  $applied-guidance: |
    Default body-text color in the cascade. Apply via `text-primary` for headings, body, captions, and labels — never as a hardcoded hex (`text-black`, `#000`) or raw color. Inside `.theme.invert`, `text-primary` auto-resolves to color.brand.invert via the cascade; hardcoded values stay dark and break against dark backgrounds. Per df-rules.md §10.1 the contract is "primary = the foreground for the current theme," not "primary = always black."
  $ai-hint: |
    When generating prose, headings, or labels, do NOT set color via inline styles or utilities — the default cascade resolves to primary automatically. Only use `text-primary` explicitly when overriding a previously-set foreground (e.g., re-asserting body color inside a card on a key-themed section).
```

```token
color.brand.secondary:
  $type: color
  $value: "#888888"
  $brand-owned: true
  $df-source: --qs-color-secondary
  $description: Secondary text — mid gray for help text.
  $applied-guidance: |
    Use via `text-secondary` for de-emphasized but still-readable content — help text, captions, metadata, form hints, footer subtext. Carries less attention weight than `text-primary` without dropping below WCAG AA body contrast on white. Inside `.theme.invert` / `.theme.key`, the cascade swaps to an appropriately lighter equivalent. Don't reach for `text-gray-500` or hardcoded grays — those bypass the cascade.
```

```token
color.brand.tertiary:
  $type: color
  $value: "#cccccc"
  $brand-owned: true
  $df-source: --qs-color-tertiary
  $description: Tertiary text — light gray, still legible.
  $applied-guidance: |
    Lowest text tier still intended to be read. Use via `text-tertiary` for short metadata — timestamps, dividers between inline items, lowest-priority labels. Below WCAG AA body contrast on raw white; reserve for short labels or pair with a shader-tinted background that lifts effective contrast. Don't use for body paragraphs or anywhere users must read a sustained line of text.
```

```token
color.brand.invert:
  $type: color
  $value: "#ffffff"
  $brand-owned: true
  $df-source: --qs-color-invert
  $description: Invert color — white. Used on dark backgrounds.
  $applied-guidance: |
    Foreground for dark themes (`.theme.invert`, `.theme.key`, `.theme.key-gradient`). Pairs with `bg-primary` in invert mode and `bg-key` in key mode. Don't apply `text-invert` directly to elements in light themes — it locks white-on-white. The cascade resolves `text-primary` to invert automatically when the parent section carries an inverting theme class; hand-applied `text-invert` defeats the cascade and won't re-flip on theme change.
  $ai-hint: |
    When generating UI inside a `.theme.invert` or `.theme.key` section, do NOT explicitly set `text-invert` on individual elements — the cascade does it. The only legitimate explicit use is in a fixed dark surface that should stay dark regardless of containing theme (rare; document the exception).
```

### Theme contexts

Theme contexts are semantic-tier tokens that compose primitives into role-based `{bg, fg}` pairs. The same semantic token (e.g. `color.theme.default.fg`) resolves to different values across modes — the role stays constant, the value changes.

```token
color.theme.default.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Default theme background. Light mode = white.
  $applied-guidance: |
    Default page-level background. Pairs with color.theme.default.fg per the fg/bg pairing rule. Don't override per-section; theme context is a section-level decision via `.theme` class on a `<section>`.
```

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground (primary text on canvas).
  $applied-guidance: |
    Default body-text color, paired with color.theme.default.bg. Inherited by `<p>`, `<h1>`–`<h6>`, `<a>` defaults — don't set color manually on these elements unless overriding for a deliberate exception.
  $ai-hint: |
    When generating prose paragraphs or headings, do NOT set color via inline styles or utilities. The default cascade handles it; setting `text-primary` is redundant and `text-black` is wrong (it bypasses the cascade per df-rules.md §10.1).
```

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Inverted theme background (dark mode).
  $applied-guidance: |
    Dark-mode section bg = primary (near-black). Apply via `.theme.invert` on a section element, not by setting bg-primary directly — the class triggers full atomic invert (card auto-revert, button gradient-key-invert behavior, shader recalibration). Manual color flipping misses these per df-rules.md §10.5.
```

```token
color.theme.invert.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Inverted theme foreground (dark mode).
  $applied-guidance: |
    Foreground role for inverted (dark) theme sections. Resolves to color.brand.invert via cascade when `.theme.invert` is applied to a section. Don't reference this semantic token directly in markup — write `text-primary` and let the cascade resolve to invert automatically inside `.theme.invert`. Lint rule 3 (contrast-ratio) verifies WCAG AA pairing with color.theme.invert.bg.
```

```token
color.theme.key.bg:
  $type: color
  $ref: "{color.brand.key}"
  $contrastTarget: large
  $description: Key theme background — saturated brand color as section bg.
  $applied-guidance: |
    Use for hero bands, CTA strips, brand-emphasized sections. Per df-rules.md §10.4, components inside `.theme.key` auto-invert (key↔invert swap on buttons/chips/form rings) so they remain visible on the saturated bg. Don't manually flip component colors in key sections — the cascade handles it. $contrastTarget=large signals the WCAG-AA threshold is 3:1 (display/hero), not 4.5:1 (body); body text inside a key section auto-inverts to higher-contrast invert per the cascade.
```

```token
color.theme.key.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Key theme foreground — invert (white) on saturated bg.
  $applied-guidance: |
    Foreground role for key-themed sections — resolves to invert (white) over the saturated key brand bg. Active inside `.theme.key` and `.theme.key-gradient` sections. The semantic-tier `$contrastTarget: large` on color.theme.key.bg signals the WCAG-AA threshold for *bg* is 3:1 (display-scale text); for body-text contrast use `text-primary` in a card or alt-styled child element so the cascade lifts contrast to 4.5:1. Per df-rules.md §10.4, components inside `.theme.key` auto-swap key↔invert; manual color flipping defeats the cascade.
```

The 14 theme contexts compose into four orthogonal axes: **palette** (default vs alt-brand), **mode** (light vs invert), **saturation** (transparent overlay vs solid surface), and **landmark** (header/footer landmarks have their own brand-owned color slots). Per df-rules.md §10.2, applying a `.theme.*` class on a `<section>` reassigns the `--color-*` cascade for everything inside; per-component overrides defeat the cascade.

```token
color.theme.default-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Default theme with transparent bg — defers to parent surface. DF-native — .theme.transparent.
```

```token
color.theme.default-transparent.fg:
  $type: color
  $ref: "{color.theme.default.fg}"
  $description: Default theme transparent foreground (fg unchanged, bg surrendered to parent).
```

```token
color.theme.alt.bg:
  $type: color
  $ref: "{color.brand-alt.background}"
  $description: Alt theme background — light mode + alt brand palette. DF-native — .theme.alt.
  $applied-guidance: |
    Background role for sections themed with the alternate brand palette (`.theme.alt`). Use the class on a `<section>`, not the semantic token directly — applying `.theme.alt` reassigns the entire `--color-*` cascade to alt counterparts (color.brand-alt.*) for everything inside. Per df-rules.md §10.2, direct `bg-background-alt` usage locks the element to alt regardless of surrounding theme; generic `bg-background` inside `.theme.alt` resolves here automatically.
```

```token
color.theme.alt.fg:
  $type: color
  $ref: "{color.brand-alt.primary}"
  $description: Alt theme foreground.
  $applied-guidance: |
    Foreground role paired with color.theme.alt.bg inside `.theme.alt` sections. Same cascade rule as the bg: use generic `text-primary` and let the cascade resolve to alt automatically — direct `text-primary-alt` usage locks the element to alt regardless of surrounding theme (df-rules.md §10.2 anti-pattern). Lint rule 3 (contrast-ratio) enforces WCAG AA against color.theme.alt.bg.
```

```token
color.theme.alt-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Alt theme with transparent bg.
```

```token
color.theme.alt-transparent.fg:
  $type: color
  $ref: "{color.theme.alt.fg}"
  $description: Alt theme transparent foreground.
```

```token
color.theme.invert-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Invert theme with transparent bg.
```

```token
color.theme.invert-transparent.fg:
  $type: color
  $ref: "{color.theme.invert.fg}"
  $description: Invert theme transparent foreground.
```

```token
color.theme.alt-invert.bg:
  $type: color
  $ref: "{color.brand-alt.primary}"
  $description: Alt + inverted theme — dark mode using alt palette. DF-native — .theme.alt-invert. Composes alt cascade on top of invert cascade.
```

```token
color.theme.alt-invert.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt-invert foreground.
```

```token
color.theme.alt-invert-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Alt-invert theme with transparent bg.
```

```token
color.theme.alt-invert-transparent.fg:
  $type: color
  $ref: "{color.theme.alt-invert.fg}"
  $description: Alt-invert transparent foreground.
```

The key-gradient context decomposes the gradient into bg/bg-start/bg-end/bg-direction so emitters can reconstruct the `linear-gradient(direction, start, end)` declaration; the bare `bg` field carries a solid fallback (defaults to `bg-start`) for consumers that don't support gradient bg.

```token
color.theme.key-gradient.bg:
  $type: color
  $ref: "{color.brand.key}"
  $contrastTarget: large
  $description: Key-gradient solid fallback for non-gradient consumers.
```

```token
color.theme.key-gradient.bg-start:
  $type: color
  $ref: "{color.brand.key}"
  $description: Key-gradient start stop.
```

```token
color.theme.key-gradient.bg-end:
  $type: color
  $ref: "{color.brand.key-end}"
  $description: Key-gradient end stop.
```

```token
color.theme.key-gradient.bg-direction:
  $type: string
  $ref: "{color.brand.background-direction}"
  $description: Key-gradient direction (CSS keyword form).
```

```token
color.theme.key-gradient.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Key-gradient foreground (invert on saturated bg).
```

```token
color.theme.key-alt.bg:
  $type: color
  $ref: "{color.brand-alt.key}"
  $contrastTarget: large
  $description: Alt key theme — solid alt-key brand color as section bg.
```

```token
color.theme.key-alt.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt key foreground.
```

```token
color.theme.key-alt-gradient.bg:
  $type: color
  $ref: "{color.brand-alt.key}"
  $contrastTarget: large
  $description: Alt key-gradient solid fallback.
```

```token
color.theme.key-alt-gradient.bg-start:
  $type: color
  $ref: "{color.brand-alt.key}"
  $description: Alt key-gradient start stop.
```

```token
color.theme.key-alt-gradient.bg-end:
  $type: color
  $ref: "{color.brand-alt.key-end}"
  $description: Alt key-gradient end stop.
```

```token
color.theme.key-alt-gradient.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt key-gradient foreground.
```

Landmark contexts (header + footer) have brand-owned color slots distinct from main theme so brands can color their chrome (top nav, footer) independent of the article surface.

```token
color.theme.header.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Header landmark background. Source — --qs-color-header-background.
```

```token
color.theme.header.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Header landmark foreground. Source — --qs-color-header-primary.
```

```token
color.theme.header.nav-link:
  $type: color
  $ref: "{color.brand.secondary}"
  $description: Header nav link color. Source — --qs-color-header-nav-link.
```

```token
color.theme.footer.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Footer landmark background. Source — --qs-color-footer-background.
```

```token
color.theme.footer.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Footer landmark foreground.
```

```token
color.theme.footer.nav-link:
  $type: color
  $ref: "{color.brand.secondary}"
  $description: Footer nav link color.
```

### Alert colors

System-owned colors for status-signaling. Per df-rules.md §10.7, alert tokens are namespaced (`bg-alert-notify`, NOT `bg-notify`) to prevent collision with semantic vocabulary.

```token
color.alert.error.bg:
  $type: color
  $value: "#fa0002"
  $system-owned: true
  $description: Error alert background.
  $applied-guidance: |
    Apply to alert containers signaling error state — form validation, destructive-action confirmations, system failures. Paired with color.alert.error.fg (white) for WCAG AA contrast on body text.
```

### Shader system

DF's shader tokens are alpha multipliers used to derive subtle-tint backgrounds from any brand color. The three values map to `light` (0.15), `base` (0.60), `heavy` (0.85).

> **NOTE:** Bare `--shader` (no suffix) resolves to `--shader-base` (0.60), NOT `--shader-light` (0.15). This is a known footgun documented in df-knowledge.md "Shader utility naming asymmetry" — when authoring custom CSS, prefer the explicit suffix.

## Typography

DF's type scale is anchored on a **16px base font-size + 24px line-height** rhythm. Every scale step (d1-d3 display, h1-h6 heading, p1-p5 body, i1-i6 icon) is a multiple or fraction of these anchors.

### Font roles

Three font-family roles (`heading`, `body`, `display`) populated via `@font-face` declarations in `df-input.css` CONFIG Fonts. Default DF setup uses DIN2014 for heading/display and NotoSans for body. Replace via `typography.family.*` in frontmatter when forking.

```token
typography.scale.p3.size:
  $type: dimension
  $value: "16px"
  $system-owned: true
  $description: Body text P3 size — equivalent to base. The anchor for the type scale.
  $applied-guidance: |
    Default body-text size. All other scale steps are ratios of this value. If you change it, recompute every paired line-height — DF's 24px line-height rhythm breaks at off-scale sizes. The p1-p5 ladder is a 1.25× ratio at the heavy end (p1=20px → p3=16px) and 0.875×/0.75× at the light end (p3=16px → p4=14px → p5=12px).
```

### Anti-patterns

- **Don't use uppercase shorthand classes** like `P4`, `D1`, `H2` — these are not defined and silently do nothing
- **Don't hardcode font-size**; use `text-p3` / `text-h1` / etc. via the type-scale tokens
- **Don't change `size` without also recomputing `lh`** — the rhythm depends on the paired ratio

## Layout

The grid system is **6-col mobile + 12-col desktop** with a 4px base unit. Semantic spacing contexts (gutter, shoulder, section, article, element, sub, stack, form) auto-adjust per breakpoint via DF's adaptive utility classes (`p-gutter`, `gap-article`, etc.).

### Block hierarchy

Build pages outside-in:

```
html > body
  header > nav
  main
    section.theme.section-grid > div.block-* > content (hgroup/nav/stack/elements)
  footer > nav
```

`block-*` classes belong on **wrapper divs** (or `<article>` for self-contained content), never on semantic content elements like `<hgroup>` or `<nav>` — putting block classes on semantic elements adds unwanted `space-y` margins between their children (e.g. breadcrumb links getting 24px gaps).

### Internal-padding cascade

Per df-rules.md §9.5, internal padding MUST cascade `gutter ≥ card ≥ element ≥ sub-element` at every breakpoint. This is a structural invariant — violating it breaks the adaptive grid math.

```token
space.gutter.base:
  $type: dimension
  $value: "24px"
  $system-owned: true
  $df-source: --spacing-gutter-base
  $description: Standard gutter spacing — mobile-first.
  $applied-guidance: |
    Use for section-level rhythm (between blocks in section-grid, between sections via m-section). Don't override per-component; gutter is a system-level decision and per-component overrides break the adaptive grid math.
```

## Elevation & Depth

Three shadow tiers (`light`, `base`, `heavy`) carry the elevation system. Plus a separate `shadow.alpha.*` sub-token scale for compositional shadows (inset, side-only, directional composites) where the full `shadow.*` tokens can't be reused.

Per df-rules.md §14.1, `box-shadow` is a single property — setting it replaces any previous value. Adding a raw custom shadow (e.g. focus ring) on an element with `shadow-light` wipes out the depth shadow entirely. Use shadow tokens (`shadow-light` / `shadow-base` / `shadow-heavy`) instead of hand-authored `box-shadow: 0 1px 3px rgb(0 0 0 / 0.1)`.

Composite shadows live in body blocks (not frontmatter) per spec §6.5 rule 5 — `$type: shadow` is a composite type whose `$value` is an object describing offset/blur/spread/color.

```token
shadow.light:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "0px"
    blur: "4px"
    spread: "0px"
    color: "rgba(0,0,0,0.125)"
  $system-owned: true
  $df-source: --shadow-light
  $description: Lightest elevation tier — surface separation without committing to depth. Alpha matches shadow.alpha.light.
```

```token
shadow.base:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "2px"
    blur: "4px"
    spread: "0px"
    color: "rgba(0,0,0,0.25)"
  $system-owned: true
  $df-source: --shadow-base
  $description: Standard elevation — default card / popover shadow. Alpha matches shadow.alpha.base.
```

```token
shadow.heavy:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "4px"
    blur: "8px"
    spread: "2px"
    color: "rgba(0,0,0,0.25)"
  $system-owned: true
  $df-source: --shadow-heavy
  $description: Heaviest elevation — modal / drawer / overlay shadow. Offset + blur do the differentiation from base; alpha is identical to base.
```

## Shapes

Border radius carries brand identity at the same tier as color and typography. Designframe exposes four brand-customizable radii under `radius.brand.*` (set via `--qs-rounded-*`), plus three system primitives for specific use cases.

### Brand radii hierarchy

The four `radius.brand.*` tokens form a hierarchy from smallest to largest:

| Token | Default | Used by |
|---|---|---|
| `radius.brand.min` | 4px | Chips, badges, inline tags — the smallest visible curvature |
| `radius.brand.base` | 8px | Buttons, form fields — the workhorse rounding |
| `radius.brand.corner` | 8px | Container corners (sections, blocks, cards) — typically equal to `base` |
| `radius.brand.field` | 20px | Form inputs at field scale — visually softer than buttons |

Brand owners change these four values to shift the visual character (sharp corners with all values at 0px; medium-soft with defaults; very soft pill-leaning with values of 16/24/24/32). The cascade handles propagation — components don't hardcode radius.

### System radii

Three additional tokens carry specific roles:

- **`radius.full`** (`9999px`) — pill / circle. Buttons with `.button.pill`, avatars, status dots. Effectively a "max" value that always renders as a half-pill regardless of element width.
- **`radius.chip`** — alias of `radius.brand.min` (4px). Renames the brand token to its primary use site so chip CSS doesn't reach across naming surfaces. If `radius.brand.min` changes, chips follow automatically.
- **`radius.article`** — adaptive scale (16px at base, 24px at xl) matching `space.article`. Cards and `<article>` containers use this so the rounding scales with their padding. The ratio (radius == padding) preserves a constant visual gap from content to corner across breakpoints.

### Do's and Don'ts

- **Do** use semantic radius tokens (`rounded-button`, `rounded-corner`, `rounded-article`) rather than literal `rounded-[8px]` — the cascade applies your brand customization across themes.
- **Do** match radius to padding for adaptive containers — `radius.article` mirrors `space.article` deliberately.
- **Don't** mix brand-rounding and system-rounding semantics on the same element — `rounded-full` on a card defeats the brand identity; `rounded-corner` on a chip looks under-styled.
- **Don't** hardcode `border-radius` in component CSS — use the token system so brand customizers can fork rounding without finding every literal.

## Components

Designframe uses a **combo class pattern**: a base type class plus space-separated modifiers applied in a consistent order — `type.subtype.variant.style.size.shape.state`.

Specificity escalates naturally: `.button` (0,1,0) → `.button.gradient` (0,2,0) → `.button.gradient.air.squared` (0,4,0). No `!important` needed.

### Button inner structure

All buttons require `<span><p>` inner structure for gradient border effects:

```html
<a class="button gradient"><span><p>Button Text</p></span></a>
<button class="button gradient secondary" type="button"><span><p>Submit</p></span></button>
```

Icon-only buttons skip the `<p>` wrapper on the icon: `<a class="button icon gradient"><span><i class="fa-light fa-heart"></i></span></a>`

### Variant vocabularies (df-rules.md §10.9)

Status indicators use one of two distinct vocabularies, never mixed:

- **Alert vocabulary** (`.notify`, `.success`, `.warning`, `.error`) — for components signaling alert state (attention level, pass/fail)
- **Lifecycle vocabulary** (`.active`, `.completed`, `.pending`, `.declined`) — for components signaling entity lifecycle position

A `.badge` uses alert vocab. A `.status-dot` uses lifecycle vocab. Mixing creates inconsistent mental models.

## Assets

Brand assets ship alongside the token system — logo lockups, marks, and favicons that consumers reference in product chrome, marketing surfaces, and platform integrations (favicon, PWA, OG image). Assets are file references with usage rules; tokens are visual property values. Per spec §19 each asset block declares `$path` (project-relative), `$applied-guidance` (clear-space + min-size + do/don't), and optional `$variants` for alternate treatments (on-dark, mark, etc.).

The Designframe identity is **a two-stop gradient wordmark** (pink `color.brand.key` → blue `color.brand.key-end`, right-top direction). Solo-color treatments exist for surfaces where the gradient lacks contrast or where simplification serves the platform (favicon, single-color print). Solo-color is the fallback, not the headline — default to the gradient lockup.

### Logo

```asset
logo.primary:
  $type: image
  $path: assets/logos/df-wordmark.svg
  $description: Primary Designframe wordmark — lowercase, two-stop gradient (key → key-end), right-top direction.
  $applied-guidance: |
    Default lockup for light surfaces. Maintain clear-space equal to space.gutter.base on all sides. Minimum size 24px height for legibility (the gradient anti-aliases poorly below that). Don't recolor — the gradient IS the brand. Don't outline, drop-shadow, or apply effects. Don't stretch non-uniformly. For dark surfaces, switch to logo.primary's on-dark variant; for solo-color contexts where the gradient washes out, use logo.mark.
  $clear-space:
    $ref: "{space.gutter.base}"
  $min-size:
    $ref: "{size.element.base}"
  $variants:
    on-dark: assets/logos/df-wordmark-on-dark.svg
```

```asset
logo.mark:
  $type: image
  $path: assets/logos/df-mark.svg
  $description: Designframe mark — rounded-square geometry, two-stop gradient fill, inset "df" monogram in invert.
  $applied-guidance: |
    Use when wordmark length is constrained (app tile, social avatar, square thumbnail, top-nav lockup). Mark inherits radius.brand.corner (8px); don't override the corner radius — it's a deliberate brand identity choice. Default mark uses gradient bg + white "df"; the on-dark variant inverts to white bg + gradient "df" for placement on dark surfaces where the gradient mark loses visual weight. Minimum size 32px square; below that the "df" letterforms lose legibility — use the favicon variant instead.
  $clear-space:
    $ref: "{space.article.base}"
  $min-size:
    $ref: "{size.element.compact}"
  $variants:
    on-dark: assets/logos/df-mark-on-dark.svg
    favicon: assets/logos/favicon.svg
```

```asset
logo.favicon:
  $type: image
  $path: assets/logos/favicon.svg
  $description: Designframe favicon — single-letter "d" mark scaled for 16-32px browser-tab and PWA-icon contexts.
  $applied-guidance: |
    Reserved for contexts where the full "df" monogram becomes unreadable (browser tabs at 16px, OS app dock at 32px, system notification badges). The single-letter "d" is a legibility-driven simplification, not a design preference — at any size where logo.mark renders legibly, prefer logo.mark.
  $min-size:
    $ref: "{size.element.sub}"
```

### Do's and Don'ts (asset-specific)

- **Do** use the gradient wordmark as the headline brand surface — homepage, sign-in screens, marketing-page headers, OG images.
- **Do** swap to the on-dark variant when bg darker than `color.theme.invert.bg` is the surface — the gradient on near-black lacks the contrast the gradient on white achieves.
- **Don't** recolor any asset variant. Brand owners forking Designframe replace primitives in PRISM.md frontmatter (`color.brand.key`, `color.brand.key-end`) — the asset SVGs re-render automatically once those primitives change. Hand-edit recoloring fights the cascade.
- **Don't** apply effects (drop shadow, glow, outline, emboss) to brand assets — DF's identity is the gradient + the geometry, not chrome treatments. If the asset needs visual weight, increase its size; don't decorate it.
- **Don't** use logo.favicon outside its 16-32px size band — it's a single-letter mark designed to remain readable at icon scale, and it looks anemic at larger sizes where the full mark or wordmark belongs.

## Modes

### dark

In dark mode, the default theme context redirects to the invert context. Brand primitives don't change; only the semantic-tier `$ref` targets reassign.

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

```token
color.bg.canvas:
  $ref: "{color.theme.invert.bg}"
  $mode: dark
```

```token
color.fg.default:
  $ref: "{color.theme.invert.fg}"
  $mode: dark
```

### alt

Alt-palette mode — every default-theme context redirects to its alt-palette equivalent. Brand-alt primitives carry the alternate identity (red/orange vs. pink/blue).

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

Key mode — the full page treats every default-theme section as if `.theme.key` were applied. Saturated key brand color as canvas; components auto-invert per df-rules.md §10.4 (structural — not represented at token level).

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

(Reserved for `df-*` premium components — see workspace `df-ui/CLAUDE.md` "Free vs Premium Boundary". Not yet authored in PRISM.md; current premium components are defined in `df-input.css` SECTION CONFIG Custom CSS Classes pending PRISM.md migration.)

## Lossiness & Constraints

This PRISM.md emits to multiple targets via `prism emit`; each target has scope-specific lossy fields enumerated below.

| Target | Lossy fields | Why |
|---|---|---|
| `df-input.css` | `$ai-hint`, `$applied-guidance`, full rationale prose, `$description` >120 chars | CSS has no carrier for these — CSS comments work but Tailwind/PostCSS strips them at build |
| `design-md.md` | `$ai-hint`, DF-native token names (mapped to MD3 aliases), `## Modes` / `## Components — Premium` / `## Lossiness & Constraints` (PRISM.md additions) | DESIGN.md format has no carrier for these |
| `tailwind-preset.js` | All prose; all `$brand-owned` / `$system-owned` flags | Tailwind preset is value-only; metadata doesn't survive |
| `tokens.css` | All prose; all metadata | Pure CSS custom property declarations only |
| `figma-tokens-studio.json` | `$ai-hint`, `$applied-guidance` (emitted to DTCG `$extensions["prism.*"]` which Tokens Studio ignores) | Tokens Studio only reads core DTCG fields |
| `tokens.json` (DTCG bundle) | None — PRISM.md → DTCG is lossless; PRISM.md extensions emit to `$extensions["prism.*"]` | DTCG `$extensions` is the documented escape hatch for non-standard fields |

## Do's and Don'ts

Cross-cutting rules that can't be token-encoded. Inherited from `df-rules.md`; consolidated here for emitted DESIGN.md / rationale.md consumers who don't have access to the full ruleset.

### Do

- **Use DF color tokens** (`text-primary`, `bg-key`, `bg-alert-error`) — they resolve through the theme cascade in every context
- **Apply `.theme.*` classes at the section level** to trigger the full atomic theme swap (card auto-revert, component key↔invert, shader recalibration)
- **Build pages outside-in** (`html > body > header / main > section > blocks > content`)
- **Set spacing/padding/margin on the parent**, never on individual children
- **Use semantic HTML directly** (`<section>`, `<article>`, `<header>`, `<footer>`, `<nav>`, `<hgroup>`)
- **Use the combo class pattern** in `type.subtype.variant.style.size.shape.state` order

### Don't

- **Don't hardcode color values** (`#0c0c0c`, `rgb(0,0,0)`, `text-black`, `bg-white`); use DF tokens — hardcoded values bypass the theme cascade and break under `.theme.invert` / `.theme.key` / `.theme.alt`
- **Don't put `block-*` classes on semantic content elements** like `<hgroup>` or `<nav>` — adds unwanted margin-top via the built-in `space-y`
- **Don't override theme contexts per-component** — they are section-level decisions
- **Don't use uppercase shorthand classes** like `P4`, `D1`, `H2` — silently do nothing
- **Don't manually flip component colors** in `.theme.key` sections — the cascade handles key↔invert swap; manual overrides fight the cascade
- **Don't add classes for default behavior** (a hover, body bg, h1 size — DF provides these automatically; redundant classes obscure intent)
- **Don't use raw Tailwind palette colors** (`bg-indigo-600`, `text-gray-500`) for UI — these bypass the cascade same as hex; reserve for deliberate exceptions outside DF theming (third-party embeds, brand-locked colors)

---

> **This file is the canonical Designframe design system definition. The `prism-v1/` bundle in this repo was hand-extracted from this source's predecessor state (df-input.css extraction at S1+S2). Future bundles emit from this PRISM.md via `prism parse` + `prism emit`. See [`PRODUCT.md`](PRODUCT.md) §5 for the pipeline.**
