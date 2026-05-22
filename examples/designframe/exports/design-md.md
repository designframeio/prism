---
name: prism-designframe
version: "0.99"
description: |-
  Designframe — minimalist adaptive design system by Mike Prasad.
  Built as a Tailwind CSS preset/extension; pure CSS output.
license: Apache-2.0
generator: df-prism/tools/emit-design-md.mjs
generated-at: 2026-05-22
---
# Prism Designframe

Generated DESIGN.md export from Designframe Prism (v1.0-export). Single-mode rendering; for multi-mode (dark, alt, key) consult the source PRISM.md or the DTCG bundle's overlay files.

## Brand & Style

**Prism Designframe is the D in MVCD** — a Display layer separated from Model / View / Controller, designed so engineers don't have to make design decisions and end users can customize UI without designer or engineer skills.

Two audiences, one solution: math + structural defaults that propagate intent without breaking layout.

Core conceptual goals: minimalist (no choice that isn't necessary), adaptive (survives across every viewport without manual tuning), semantic (HTML elements carry meaning; classes never override that), resilient (composed by AI, junior engineers, and non-designers without producing broken layouts).

## Colors

Color tokens use MD3 (Material Design 3) canonical names where applicable; DF-native paths preserved for traceability via the DF Native column. Single-mode rendering — light theme only (multi-mode overlays available via PRISM.md `## Modes` section + DTCG mode overlays).

| MD3 Name | Value | DF Native |
|---|---|---|
| `primary` | `#222222` | `color.brand.key` |
| `primary-container` | `#222222` | `color.brand.key-end` |
| `on-surface` | `#0c0c0c` | `color.brand.primary` |
| `on-surface-variant` | `#757575` | `color.brand.secondary` |
| `outline` | `#cccccc` | `color.brand.tertiary` |
| `on-primary` | `#ffffff` | `color.brand.invert` |
| `on-surface-disabled` | `#cccccc` | `color.brand.disabled` |
| `surface` | `#ffffff` | `color.brand.background` |
| `surface-bright` | `#dddddd` | `color.brand.background-start` |
| `surface-dim` | `#ffffff` | `color.brand.background-end` |
| `brand-alt-key` | `#000000` | `color.brand-alt.key` |
| `brand-alt-key-end` | `#222222` | `color.brand-alt.key-end` |
| `brand-alt-primary` | `#0c0c0c` | `color.brand-alt.primary` |
| `brand-alt-secondary` | `#757575` | `color.brand-alt.secondary` |
| `brand-alt-tertiary` | `#cccccc` | `color.brand-alt.tertiary` |
| `brand-alt-invert` | `#ffffff` | `color.brand-alt.invert` |
| `brand-alt-disabled` | `#cccccc` | `color.brand-alt.disabled` |
| `brand-alt-background` | `#ffffff` | `color.brand-alt.background` |
| `brand-alt-background-start` | `#cccccc` | `color.brand-alt.background-start` |
| `brand-alt-background-end` | `#ffffff` | `color.brand-alt.background-end` |
| `alert-notify-base` | `#60a5fa` | `color.alert.notify.base` |
| `alert-notify-heading` | `#1d4ed8` | `color.alert.notify.heading` |
| `alert-notify-text` | `#2563eb` | `color.alert.notify.text` |
| `alert-notify-background` | `#dbeafe` | `color.alert.notify.background` |
| `alert-warning-base` | `#facc15` | `color.alert.warning.base` |
| `alert-warning-heading` | `#a16207` | `color.alert.warning.heading` |
| `alert-warning-text` | `#ca8a04` | `color.alert.warning.text` |
| `alert-warning-background` | `#fef9c3` | `color.alert.warning.background` |
| `error` | `#f87171` | `color.alert.error.base` |
| `on-error-container` | `#b91c1c` | `color.alert.error.heading` |
| `alert-error-text` | `#dc2626` | `color.alert.error.text` |
| `error-container` | `#fee2e2` | `color.alert.error.background` |
| `alert-success-base` | `#4ade80` | `color.alert.success.base` |
| `alert-success-heading` | `#15803d` | `color.alert.success.heading` |
| `alert-success-text` | `#16a34a` | `color.alert.success.text` |
| `alert-success-background` | `#dcfce7` | `color.alert.success.background` |
| `background` | `#ffffff` | `color.theme.default.bg` |
| `on-background` | `#0c0c0c` | `color.theme.default.fg` |
| `inverse-surface` | `#0c0c0c` | `color.theme.invert.bg` |
| `inverse-on-surface` | `#ffffff` | `color.theme.invert.fg` |
| `theme-default-transparent-fg` | `#0c0c0c` | `color.theme.default-transparent.fg` |
| `theme-alt-bg` | `#ffffff` | `color.theme.alt.bg` |
| `theme-alt-fg` | `#0c0c0c` | `color.theme.alt.fg` |
| `theme-alt-transparent-fg` | `#0c0c0c` | `color.theme.alt-transparent.fg` |
| `theme-invert-transparent-fg` | `#ffffff` | `color.theme.invert-transparent.fg` |
| `theme-alt-invert-bg` | `#0c0c0c` | `color.theme.alt-invert.bg` |
| `theme-alt-invert-fg` | `#ffffff` | `color.theme.alt-invert.fg` |
| `theme-alt-invert-transparent-fg` | `#ffffff` | `color.theme.alt-invert-transparent.fg` |
| `theme-key-gradient-bg` | `#222222` | `color.theme.key-gradient.bg` |
| `theme-key-gradient-bg-start` | `#222222` | `color.theme.key-gradient.bg-start` |
| `theme-key-gradient-bg-end` | `#222222` | `color.theme.key-gradient.bg-end` |
| `theme-key-gradient-bg-direction` | `right top` | `color.theme.key-gradient.bg-direction` |
| `theme-key-gradient-fg` | `#ffffff` | `color.theme.key-gradient.fg` |
| `theme-key-alt-bg` | `#000000` | `color.theme.key-alt.bg` |
| `theme-key-alt-fg` | `#ffffff` | `color.theme.key-alt.fg` |
| `theme-key-alt-gradient-bg` | `#000000` | `color.theme.key-alt-gradient.bg` |
| `theme-key-alt-gradient-bg-start` | `#000000` | `color.theme.key-alt-gradient.bg-start` |
| `theme-key-alt-gradient-bg-end` | `#222222` | `color.theme.key-alt-gradient.bg-end` |
| `theme-key-alt-gradient-fg` | `#ffffff` | `color.theme.key-alt-gradient.fg` |
| `theme-header-bg` | `#ffffff` | `color.theme.header.bg` |
| `theme-header-fg` | `#0c0c0c` | `color.theme.header.fg` |
| `theme-header-nav-link` | `#757575` | `color.theme.header.nav-link` |
| `theme-footer-bg` | `#ffffff` | `color.theme.footer.bg` |
| `theme-footer-fg` | `#0c0c0c` | `color.theme.footer.fg` |
| `theme-footer-nav-link` | `#757575` | `color.theme.footer.nav-link` |

## Typography

The type scale anchors on a 16px / 24px size+line-height base. Every other step is a ratio of these anchors. Display + heading sizes use unitless line-height 1; body + utility sizes use absolute px values per the rhythm grid.

| Name | Size | Line Height |
|---|---|---|
| `base` | 16px | 24px |
| `d1` | 96px | 1 (unitless) |
| `d2` | 72px | 1 (unitless) |
| `d3` | 60px | 1 (unitless) |
| `h1` | 48px | 1 (unitless) |
| `h2` | 32px | 40px |
| `h3` | 24px | 32px |
| `h4` | 20px | 28px |
| `h5` | 18px | 28px |
| `h6` | 16px | 24px |
| `p1` | 20px | 28px |
| `p2` | 18px | 28px |
| `p3` | 16px | 24px |
| `p4` | 14px | 20px |
| `p5` | 12px | 16px |
| `nav` | 20px | 40px |
| `button` | 18px | 40px |
| `chip` | 12px | 24px |

## Layout

Spacing follows a 4px base unit. Each semantic context (gutter, shoulder, article, element, section, header, stack, form, sub) declares its own adaptive scale across breakpoints (base / sm / md / lg / xl / 2xl). Tokens shown at base, with xl variant in parentheses when distinct.

| Context | Base Value | Variants |
|---|---|---|
| `unit` | 4px | Layout base unit |
| `gutter` | 24px (→ 32px at xl) | Adaptive |
| `shoulder` | 24px (→ 40px at xl) | Adaptive |
| `article` | 16px (→ 24px at xl) | Adaptive |
| `element` | 16px | Adaptive |
| `section` | 48px (→ 64px at xl) | Adaptive |
| `header` | 12px (→ 16px at xl) | Adaptive |
| `stack` | 16px | Adaptive |
| `form` | 12px (→ 16px at xl) | Adaptive |
| `sub` | 8px (→ 12px at xl) | Adaptive |

## Elevation & Depth

Three composite shadow tiers (`shadow-light`, `shadow-base`, `shadow-heavy`) provide depth without committing to color overlay. Alpha companions (`shadow.alpha.*`) expose the alpha slot for custom shadow composition.

| Token | Value |
|---|---|
| `shadow-light` | `0px 0px 4px 0px rgba(0,0,0,0.125)` |
| `shadow-base` | `0px 2px 4px 0px rgba(0,0,0,0.25)` |
| `shadow-heavy` | `0px 4px 8px 2px rgba(0,0,0,0.25)` |

## Shapes

Border radius is brand-owned at the configurable level (`radius.brand.*`). Articles + cards derive their rounding from the adaptive `radius.article` scale. Pills use `radius.full` (9999px).

| Token | Value | Notes |
|---|---|---|
| `radius-brand-min` | 4px | Brand customizable |
| `radius-brand-base` | 0px | Brand customizable |
| `radius-brand-corner` | 4px | Brand customizable |
| `radius-brand-field` | 4px | Brand customizable |
| `radius-full` | 9999px | Pill / circle |
| `radius-article` | 16px (→ 24px at xl) | Card / article |

## Assets

Brand assets ship alongside the token system. Each asset declares a project-relative `$path`, usage rules via `$applied-guidance`, and optional variants for alternate treatments (`on-dark`, `mark`, etc.). Per spec §19, assets are Prism-native extensions — they emit to this DESIGN.md as a reference table; full usage rationale lives in the source PRISM.md.

| Asset | Type | Path | Description | Variants |
|---|---|---|---|---|
| `logo.primary` | image | `assets/logos/df-wordmark.svg` | Primary Designframe wordmark — "DESIGNFRAME" in solid | 1 variant |
| `logo.mark` | image | `assets/logos/df-mark.svg` | Designframe mark — three solid circles of decreasing size, aligned on the horizontal centerline of a 1024×1024 square canvas. The default variant places white circles on a | 2 variants |
| `logo.favicon` | image | `assets/logos/favicon.svg` | Designframe favicon — the three-circle mark inscribed in a full-bleed white circle. Geometrically the same motif as logo.mark but bounded by a circular outline rather than the square canvas, sized for 16-32px browser-tab and PWA-icon contexts where a circular mask is common. | — |
| `font.satoshi` | font-file | `assets/fonts/Satoshi-Variable.woff2` | Designframe canonical heading/body/display face. Satoshi by Indian Type Foundry — variable axis weight 300-900, upright. Geometric grotesque with circular round-letters that echo the three-circle mark and hard squared terminals that echo the "DESIGNFRAME" wordmark. | — |
| `font.satoshi-italic` | font-file | `assets/fonts/Satoshi-VariableItalic.woff2` | Satoshi italic companion — variable axis weight 300-900, italic style. Separate woff2 file (not a slnt/ital axis on the upright variable) per Indian Type Foundry's release convention. Pairs with font.satoshi under the same 'Satoshi' family name; CSS resolves to this file when font-style:italic is requested. | — |
| `font.general-sans` | font-file | `assets/fonts/GeneralSans-Variable.woff2` | Designframe canonical body face. General Sans by Indian Type Foundry — variable axis weight 200-700, upright. ITF's foundry-house mono-family sibling to Satoshi: grid-derived neutral grotesque, restrained character tuned for body-text legibility. Designed by the same team within the same modernist-restraint philosophy as Satoshi + Erode. | — |
| `font.general-sans-italic` | font-file | `assets/fonts/GeneralSans-VariableItalic.woff2` | General Sans italic companion — variable axis weight 200-700, italic style. True cursive italic (different letterforms from upright, not slant axis) per ITF convention. Shares the 'General Sans' family name with the upright file; CSS resolves to this file when font-style:italic is requested. | — |
| `font.commit-mono` | font-file | `assets/fonts/CommitMono-Variable.woff2` | Designframe canonical mono face. Commit Mono by Eigil Nikolajsen — variable axis weight 300-700. Geometric mono construction with intentionally ligature-free design ("Ligatures fundamentally alter the perception of code by visually merging multiple characters into one, which can make it harder to parse what's actually written" — Eigil). Used by typography.family.mono for code surfaces. | — |

## Components

Components inherit from the token system via the DF cascade. Per spec §10 strict-superset contract, component definitions are out of scope for the DESIGN.md emit — consult the source PRISM.md `## Components` section for combo-class patterns, variant vocabularies, and inner-element conventions.

Key conventions:
- Buttons follow a `<a class="button gradient"><span><p>Text</p></span></a>` inner-structure pattern (gradient border effects require nested span+p).
- Status indicators use two distinct vocabularies, never mixed: **alert** (`notify`, `warning`, `error`, `success`) for attention-level signaling; **lifecycle** (`active`, `completed`, `pending`, `declined`) for entity-state signaling.

## Do's and Don'ts

### Do
- Use design tokens (`bg-key`, `text-primary`, `bg-alert-error`) — they resolve through the theme cascade in every context.
- Apply theme classes at the section level (`.theme.invert`, `.theme.alt`, `.theme.key`) to trigger atomic theme swaps.
- Build pages outside-in (html > body > header / main > section > blocks > content).
- Set spacing/padding on the parent wrapper, never on individual children.
- Use semantic HTML directly (`<section>`, `<article>`, `<header>`, `<footer>`, `<nav>`).

### Don't
- Don't hardcode color values (`#0c0c0c`, `text-black`, `bg-white`) — these bypass the theme cascade.
- Don't put `block-*` classes on semantic content elements (`<hgroup>`, `<nav>`) — they add unwanted margin via `space-y`.
- Don't override theme contexts per-component — they are section-level decisions.
- Don't manually flip component colors in `.theme.key` sections — the cascade handles key↔invert swap.
- Don't use uppercase shorthand classes (`P4`, `D1`, `H2`) — these are undefined and silently do nothing.

