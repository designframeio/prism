---
name: prism-designframe
version: "0.99"
description: |-
  Designframe — minimalist adaptive design system by Mike Prasad.
  Built as a Tailwind CSS preset/extension; pure CSS output.
license: Apache-2.0
generator: df-prism/tools/emit-design-md.mjs
generated-at: 2026-05-18
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
| `primary` | `#fb03b9` | `color.brand.key` |
| `primary-container` | `#3883ff` | `color.brand.key-end` |
| `on-surface` | `#0c0c0c` | `color.brand.primary` |
| `on-surface-variant` | `#888888` | `color.brand.secondary` |
| `outline` | `#cccccc` | `color.brand.tertiary` |
| `on-primary` | `#ffffff` | `color.brand.invert` |
| `on-surface-disabled` | `#cccccc` | `color.brand.disabled` |
| `surface` | `#ffffff` | `color.brand.background` |
| `surface-bright` | `#ffe6f8` | `color.brand.background-start` |
| `surface-dim` | `#ebf3ff` | `color.brand.background-end` |
| `brand-alt-key` | `#fa0002` | `color.brand-alt.key` |
| `brand-alt-key-end` | `#faa002` | `color.brand-alt.key-end` |
| `brand-alt-primary` | `#0c0c0c` | `color.brand-alt.primary` |
| `brand-alt-secondary` | `#888888` | `color.brand-alt.secondary` |
| `brand-alt-tertiary` | `#cccccc` | `color.brand-alt.tertiary` |
| `brand-alt-invert` | `#ffffff` | `color.brand-alt.invert` |
| `brand-alt-disabled` | `#cccccc` | `color.brand-alt.disabled` |
| `brand-alt-background` | `#ffffff` | `color.brand-alt.background` |
| `brand-alt-background-start` | `#ff9f1c` | `color.brand-alt.background-start` |
| `brand-alt-background-end` | `#ffd166` | `color.brand-alt.background-end` |
| `alert-notify-bg` | `#3883ff` | `color.alert.notify.bg` |
| `alert-notify-fg` | `#ffffff` | `color.alert.notify.fg` |
| `tertiary` | `#faa002` | `color.alert.warning.bg` |
| `on-tertiary` | `#0c0c0c` | `color.alert.warning.fg` |
| `error` | `#fa0002` | `color.alert.error.bg` |
| `on-error` | `#ffffff` | `color.alert.error.fg` |
| `alert-success-bg` | `#22c55e` | `color.alert.success.bg` |
| `alert-success-fg` | `#ffffff` | `color.alert.success.fg` |
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
| `theme-key-gradient-bg` | `#fb03b9` | `color.theme.key-gradient.bg` |
| `theme-key-gradient-bg-start` | `#fb03b9` | `color.theme.key-gradient.bg-start` |
| `theme-key-gradient-bg-end` | `#3883ff` | `color.theme.key-gradient.bg-end` |
| `theme-key-gradient-bg-direction` | `right top` | `color.theme.key-gradient.bg-direction` |
| `theme-key-gradient-fg` | `#ffffff` | `color.theme.key-gradient.fg` |
| `theme-key-alt-bg` | `#fa0002` | `color.theme.key-alt.bg` |
| `theme-key-alt-fg` | `#ffffff` | `color.theme.key-alt.fg` |
| `theme-key-alt-gradient-bg` | `#fa0002` | `color.theme.key-alt-gradient.bg` |
| `theme-key-alt-gradient-bg-start` | `#fa0002` | `color.theme.key-alt-gradient.bg-start` |
| `theme-key-alt-gradient-bg-end` | `#faa002` | `color.theme.key-alt-gradient.bg-end` |
| `theme-key-alt-gradient-fg` | `#ffffff` | `color.theme.key-alt-gradient.fg` |
| `theme-header-bg` | `#ffffff` | `color.theme.header.bg` |
| `theme-header-fg` | `#0c0c0c` | `color.theme.header.fg` |
| `theme-header-nav-link` | `#888888` | `color.theme.header.nav-link` |
| `theme-footer-bg` | `#ffffff` | `color.theme.footer.bg` |
| `theme-footer-fg` | `#0c0c0c` | `color.theme.footer.fg` |
| `theme-footer-nav-link` | `#888888` | `color.theme.footer.nav-link` |

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
| `radius-brand-base` | 8px | Brand customizable |
| `radius-brand-corner` | 8px | Brand customizable |
| `radius-brand-field` | 20px | Brand customizable |
| `radius-full` | 9999px | Pill / circle |
| `radius-article` | 16px (→ 24px at xl) | Card / article |

## Assets

Brand assets ship alongside the token system. Each asset declares a project-relative `$path`, usage rules via `$applied-guidance`, and optional variants for alternate treatments (`on-dark`, `mark`, etc.). Per spec §19, assets are Prism-native extensions — they emit to this DESIGN.md as a reference table; full usage rationale lives in the source PRISM.md.

| Asset | Type | Path | Description | Variants |
|---|---|---|---|---|
| `logo.primary` | image | `assets/logos/df-wordmark.svg` | Primary Designframe wordmark — lowercase, two-stop gradient (key → key-end), right-top direction. | 1 variant |
| `logo.mark` | image | `assets/logos/df-mark.svg` | Designframe mark — rounded-square geometry, two-stop gradient fill, inset "df" monogram in invert. | 2 variants |
| `logo.favicon` | image | `assets/logos/favicon.svg` | Designframe favicon — single-letter "d" mark scaled for 16-32px browser-tab and PWA-icon contexts. | — |

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

