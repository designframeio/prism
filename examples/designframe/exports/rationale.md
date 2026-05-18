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

### Theme contexts

Theme contexts are semantic-tier tokens that compose primitives into role-based `{bg, fg}` pairs. The same semantic token (e.g. `color.theme.default.fg`) resolves to different values across modes — the role stays constant, the value changes.

The 14 theme contexts compose into four orthogonal axes: **palette** (default vs alt-brand), **mode** (light vs invert), **saturation** (transparent overlay vs solid surface), and **landmark** (header/footer landmarks have their own brand-owned color slots). Per df-rules.md §10.2, applying a `.theme.*` class on a `<section>` reassigns the `--color-*` cascade for everything inside; per-component overrides defeat the cascade.

The key-gradient context decomposes the gradient into bg/bg-start/bg-end/bg-direction so emitters can reconstruct the `linear-gradient(direction, start, end)` declaration; the bare `bg` field carries a solid fallback (defaults to `bg-start`) for consumers that don't support gradient bg.

Landmark contexts (header + footer) have brand-owned color slots distinct from main theme so brands can color their chrome (top nav, footer) independent of the article surface.

### Alert colors

System-owned colors for status-signaling. Per df-rules.md §10.7, alert tokens are namespaced (`bg-alert-notify`, NOT `bg-notify`) to prevent collision with semantic vocabulary.

### Shader system

DF's shader tokens are alpha multipliers used to derive subtle-tint backgrounds from any brand color. The three values map to `light` (0.15), `base` (0.60), `heavy` (0.85).

> **NOTE:** Bare `--shader` (no suffix) resolves to `--shader-base` (0.60), NOT `--shader-light` (0.15). This is a known footgun documented in df-knowledge.md "Shader utility naming asymmetry" — when authoring custom CSS, prefer the explicit suffix.

## Typography

DF's type scale is anchored on a **16px base font-size + 24px line-height** rhythm. Every scale step (d1-d3 display, h1-h6 heading, p1-p5 body, i1-i6 icon) is a multiple or fraction of these anchors.

### Font roles

Three font-family roles (`heading`, `body`, `display`) populated via `@font-face` declarations in `df-input.css` CONFIG Fonts. Default DF setup uses DIN2014 for heading/display and NotoSans for body. Replace via `typography.family.*` in frontmatter when forking.

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

## Elevation & Depth

Three shadow tiers (`light`, `base`, `heavy`) carry the elevation system. Plus a separate `shadow.alpha.*` sub-token scale for compositional shadows (inset, side-only, directional composites) where the full `shadow.*` tokens can't be reused.

Per df-rules.md §14.1, `box-shadow` is a single property — setting it replaces any previous value. Adding a raw custom shadow (e.g. focus ring) on an element with `shadow-light` wipes out the depth shadow entirely. Use shadow tokens (`shadow-light` / `shadow-base` / `shadow-heavy`) instead of hand-authored `box-shadow: 0 1px 3px rgb(0 0 0 / 0.1)`.

Composite shadows live in body blocks (not frontmatter) per spec §6.5 rule 5 — `$type: shadow` is a composite type whose `$value` is an object describing offset/blur/spread/color.

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

### Do's and Don'ts (asset-specific)

- **Do** use the gradient wordmark as the headline brand surface — homepage, sign-in screens, marketing-page headers, OG images.
- **Do** swap to the on-dark variant when bg darker than `color.theme.invert.bg` is the surface — the gradient on near-black lacks the contrast the gradient on white achieves.
- **Don't** recolor any asset variant. Brand owners forking Designframe replace primitives in PRISM.md frontmatter (`color.brand.key`, `color.brand.key-end`) — the asset SVGs re-render automatically once those primitives change. Hand-edit recoloring fights the cascade.
- **Don't** apply effects (drop shadow, glow, outline, emboss) to brand assets — DF's identity is the gradient + the geometry, not chrome treatments. If the asset needs visual weight, increase its size; don't decorate it.
- **Don't** use logo.favicon outside its 16-32px size band — it's a single-letter mark designed to remain readable at icon scale, and it looks anemic at larger sizes where the full mark or wordmark belongs.

## Modes

### dark

In dark mode, the default theme context redirects to the invert context. Brand primitives don't change; only the semantic-tier `$ref` targets reassign.

### alt

Alt-palette mode — every default-theme context redirects to its alt-palette equivalent. Brand-alt primitives carry the alternate identity (red/orange vs. pink/blue).

### key

Key mode — the full page treats every default-theme section as if `.theme.key` were applied. Saturated key brand color as canvas; components auto-invert per df-rules.md §10.4 (structural — not represented at token level).

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
