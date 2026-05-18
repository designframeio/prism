# PRISM.md Designer Guide

> **Last updated:** 2026-05-16 (S2). This is the **designer-facing companion** to the technical spec at [`prism-md-format.md`](prism-md-format.md). If you've never touched PRISM.md before, start here.
>
> **You don't need to read the technical spec to use PRISM.md.** The spec is the canonical reference for parser authors and edge cases; this guide is what you actually read to author a design system file.

---

## What is PRISM.md, in plain language?

**PRISM.md is a single Markdown file that defines your entire design system.** Colors, typography, spacing, themes, brand voice, do's and don'ts — all in one place. From this one file, Prism emits the formats your tools and team actually consume:

- **A Tailwind preset** — installs into any Tailwind project; gives you `bg-key`, `text-primary`, etc.
- **A `tokens.css` file** — CSS custom properties for any non-Tailwind project
- **A `df-input.css` file** — if you're using Designframe as your framework runtime
- **A Tokens Studio JSON** — imports into Figma so designers and developers share the same source
- **A Google DESIGN.md file** — the format that claude.ai/design and other AI tools speak
- **A DTCG JSON bundle** — the W3C standard for design tokens (works with Style Dictionary, etc.)

You edit one file. Six (or more) target formats stay in sync automatically.

### Why bother with PRISM.md instead of just writing CSS?

A few real reasons:

1. **AI tools can read it.** Upload PRISM.md (or its emitted DESIGN.md / DTCG) to claude.ai/design and the AI now understands your brand's full design system — not just the colors, but the rationale, the anti-patterns, the "when to use this" guidance.

2. **You write prose, not CSS variables.** Want to explain why a card's padding is system-level and shouldn't be overridden? Type a paragraph in the body. That paragraph survives every emit; it shows up in the rationale, in the AI's understanding, in your team's onboarding.

3. **Designers can edit it.** PRISM.md doesn't require CSS or JavaScript expertise. The colors live in a YAML block; you change `#fb03b9` to your brand hex and re-emit. Engineers handle the toolchain; designers handle the design system.

4. **One source, many surfaces.** Without PRISM.md, your brand colors live in `df-input.css` (engineer territory), Figma variables (designer territory), maybe a `theme.json` for a CMS, and a brand guidelines PDF. They drift. With PRISM.md, you edit once and re-emit; everything stays consistent.

5. **It's also a valid Google DESIGN.md.** PRISM.md is a strict superset of Google's DESIGN.md format — anything you author in PRISM.md can be read by any tool that understands DESIGN.md, with no translation step.

---

## When to use it

Three common scenarios:

| Scenario | Use PRISM.md? |
|---|---|
| **Starting a new design system from scratch** | Yes — copy the template, customize, emit. The format scales from a 10-color toy to a full Designframe-scale system. |
| **You have an existing Designframe project and want a portable definition** | Yes — run `prism import df-input.css` (when the CLI ships) to bootstrap a PRISM.md from your existing source. Hand-edit from there. |
| **You have a non-DF design system (your own CSS, Tailwind project, etc.)** | Sometimes — PRISM.md works without Designframe, but the DF-specific tokens (semantic spacing contexts, theme cascade) are most valuable on DF or DF-adjacent projects. For a vanilla Tailwind project, plain DTCG JSON might be a lighter fit. |

If you're a Designframe consumer, the answer is almost always yes.

---

## Before you start

You'll need:

- A copy of `PRISM.template.md` (in this same `spec/` folder)
- A text editor (any editor that handles Markdown — VS Code, Sublime, etc.)
- Your brand's hex colors and font names handy
- ~30 minutes for a basic customization pass; ~2 hours for a full first-draft including prose

You'll NOT need:

- CSS or JavaScript knowledge
- Familiarity with W3C DTCG spec (the format hides that complexity)
- A working `prism` CLI (the file is hand-editable and can be reviewed before any CLI exists)

---

## Step-by-step: from blank to first PRISM.md

### Step 1 — Copy the template

```bash
cp spec/PRISM.template.md PRISM.md
```

Or copy it to your own project's root as `PRISM.md`. If you're authoring multiple design systems in one repo, use `<project>.PRISM.md` instead (e.g., `commonspace.PRISM.md`, `seersite.PRISM.md`).

### Step 2 — Set your project identity

Open the file. At the top, you'll see a YAML block (everything between the first `---` and the second `---`). Find these three fields and replace the CHANGEME values:

```yaml
name: CHANGEME-project-name              # → kebab-case, e.g., "my-design-system"
version: 0.1.0                            # → leave as 0.1.0 for first draft
description: |
  CHANGEME — one-paragraph description    # → write what your design system is
                                          #   for + who its audience is
```

That's it for identity. You can fill in `license` and `homepage` if you have them; skip otherwise.

### Step 3 — Replace your brand colors (the visible change)

Scroll down to the `color:` block in the YAML frontmatter. You'll see:

```yaml
color:
  brand:
    key:        { $type: color, $value: "#0066cc", ... }   # ← your brand main
    key-end:    { $type: color, $value: "#0066cc", ... }   # ← gradient end-stop
    primary:    { $type: color, $value: "#0c0c0c", ... }   # ← darkest text
    secondary:  { $type: color, $value: "#666666", ... }   # ← muted text
    tertiary:   { $type: color, $value: "#cccccc", ... }   # ← lightest legible text
    invert:     { $type: color, $value: "#ffffff", ... }   # ← white-on-dark
    background: { $type: color, $value: "#ffffff", ... }   # ← page bg
```

**Replace each `$value` hex with your brand color.**

#### What does "key" mean?

`key` is your brand identity accent — the color that says "this is us." For Designframe, it's pink (`#fb03b9`). For a fintech brand it might be a specific blue. It's NOT necessarily your most-used color — it's your most-distinctive.

#### What about "key-end"?

If your brand has a gradient as its identity (Designframe goes pink → blue), `key` is the start and `key-end` is the end. If your brand is a **single solid color**, set `key` and `key-end` to the **same hex**. Don't leave one as the template placeholder.

```yaml
# Solid-color brand (no gradient)
key:     { $type: color, $value: "#0066cc", ... }
key-end: { $type: color, $value: "#0066cc", ... }   # ← same hex

# Gradient brand
key:     { $type: color, $value: "#fb03b9", ... }
key-end: { $type: color, $value: "#3883ff", ... }   # ← different hex
```

#### Primary / Secondary / Tertiary — pick your text scale

These are your **monotone text colors**, ordered most-emphasized to least:

- `primary` (darkest) — body text, headings on light backgrounds
- `secondary` (mid gray) — help text, descriptions, captions
- `tertiary` (light gray) — placeholder text, subtle dividers, faint metadata

For a light-mode design system, the typical scale is `#0c0c0c → #666666 → #cccccc` or similar. Pick values your text remains legible at.

#### Invert / Background

- `invert` — what color text becomes when it's on a dark background. Usually white (`#ffffff`).
- `background` — your page-level base. Usually white for light designs.

### Step 4 — Decide if you need an alt palette

The template has a `brand-alt:` block right after `brand:`. This is **optional** — it's only used if your design system has a *secondary brand identity* (alt theme, campaign mode, secondary product line).

**If you don't need it:** delete the entire `brand-alt:` block. Also remove `color.brand-alt` from the `brandOwnedRoots:` list above, and remove the `alt` mode from the `modes:` block.

**If you do need it:** replace the alt colors with your alt-identity hex values, same way you did for `brand`.

### Step 5 — Replace your fonts

In the `typography:` block:

```yaml
typography:
  family:
    heading: { $value: ["CHANGEMEHeadingFontFamily", "sans-serif", "system-ui"], ... }
    body:    { $value: ["CHANGEMEBodyFontFamily", "sans-serif", "system-ui"], ... }
    display: { $value: ["CHANGEMEDisplayFontFamily", "sans-serif", "system-ui"], ... }
```

Replace each `CHANGEME...FontFamily` with the actual font-family name you'd use in CSS. Three notes:

1. **The same font can appear in multiple roles.** If you only use one font, put it in all three. If `heading` and `display` are the same, that's fine.
2. **Keep the fallbacks** (`sans-serif`, `system-ui`) unless you have a specific reason to change them.
3. **Font files themselves live elsewhere.** PRISM.md only declares the *names*; the actual `@font-face` declarations live in your CSS pipeline. The names must match.

### Step 6 — Decide what's "brand-owned" vs "system-owned"

You'll see two arrays in the frontmatter:

```yaml
brandOwnedRoots:
  - color.brand
  - color.brand-alt
  - typography.family
  - radius.brand

systemOwnedRoots:
  - screen
  - layout
  - space
  - shadow
  - shader
  - typography.scale
  - color.alert
```

**Plain-language explanation:**

- **Brand-owned** = tokens that another team forking your design system would want to change to match their brand. Your brand pink, your fonts, your border-radius style. Things that say "this is us."
- **System-owned** = tokens that should stay the same across all consumers of your design system. The spacing scale, the breakpoints, the type-size ratios. Things that say "this is the math."

The template's defaults are reasonable. Adjust only if:

- You're declaring **new top-level paths** specific to your project (e.g., `color.charts` if you have a chart-color palette — decide if it's brand or system).
- Your brand truly considers something else replaceable (e.g., if your design system lets consumers change the breakpoints, move `screen` from `systemOwnedRoots` to `brandOwnedRoots`).

In doubt: leave it as-is. The defaults work for ~90% of design systems.

### Step 7 — Choose your modes

```yaml
modes:
  - name: default
    description: Light mode + main brand palette
    base: true                            # ← exactly one mode must be base

  - name: dark
    description: Dark mode — bg flips to primary, fg to invert
    aliases: [invert]

  - name: alt
    description: Alt palette — secondary brand identity

  - name: key
    description: Saturated key brand color as section bg
```

**Modes are "alternate skins" for your design system.** Each mode reassigns certain tokens to different values. The base mode (light + main palette) is the default; other modes overlay on top.

**Common decisions:**

| Mode | Keep? |
|---|---|
| `default` | **Always required.** This is your light-mode + main-brand baseline. |
| `dark` | Keep if your product supports dark mode. Delete the mode AND its `## Modes > ### dark` body section if not. |
| `alt` | Only if you have an alt palette (Step 4). Delete both the mode declaration and the body section otherwise. |
| `key` | Keep if you use saturated-brand-color sections in your design (hero bands, CTA strips). Delete otherwise. |

The `aliases: [invert]` line means "dark mode is also reachable by the name `invert`" — Designframe convention. Leave it if you're emitting for Designframe; remove if you find it confusing.

### Step 8 — Write your Overview prose

Scroll past the YAML frontmatter to the body Markdown. You'll see:

```markdown
# CHANGEME — Design System Name

CHANGEME — one-paragraph elevator pitch.

## Overview

CHANGEME-OVERVIEW. Cover: who designed this, who it's for, what makes it
distinctive, what its core conceptual goals are.
```

Replace the H1 with your design system's name. Then write the Overview section in prose — a few paragraphs covering:

- **What this design system is for** (your product, your team, your brand)
- **Who it's for** (audience: engineers? designers? AI tools? all three?)
- **What makes it distinctive** (your core conceptual goals — what choices it removes, what philosophies it embodies)

**The Overview is the most important prose in the file.** AI tools, new team members, and external consumers read it first. Aim for 200-400 words.

Don't worry about the other sections yet — fill them in later, or hand-author them as needed. The Overview alone is enough to start.

### Step 9 — Add prose to the other sections (incremental)

Below Overview, you'll find sections for Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts. Each starts with a `CHANGEME` prose placeholder.

**You don't have to fill them all in.** A useful authoring pattern:

1. **First pass:** write Overview + Colors + Typography prose. Skip the rest.
2. **Use the design system for a week or two.** Notice which sections you wish had documented rules.
3. **Second pass:** fill in the sections that earned their place. Skip the rest.

Sections you skip remain as `CHANGEME` placeholders in your PRISM.md but won't break anything — emitters tolerate empty sections.

#### What to write per section

**Colors section** — palette philosophy, theme contexts, alert colors, shader system, fg/bg pairing rule. The Designframe PRISM.md (`../PRISM.md`) is a good reference for shape.

**Typography section** — font strategy, type scale rationale, line-height anchor. Why these sizes? What ratio?

**Layout section** — grid system, semantic spacing contexts (gutter, shoulder, article, element), block hierarchy. The "outside-in build order" rule lives here.

**Elevation & Depth section** — shadow tiers, layering rules. Usually short.

**Shapes section** — border-radius system, when each radius applies. Short.

**Components section** — combo class pattern (if using DF), button structure, form input wrapper pattern. Reference your component documentation here.

**Do's and Don'ts section** — cross-cutting rules that can't be token-encoded. Anti-patterns for your specific design system. This is where your "don't hardcode `#000000`, use `text-primary`" rules live.

### Step 10 — Add token-level rationale where it earns its place

In the body, you'll see code blocks marked ` ```token `:

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground (body text on canvas).
  $applied-guidance: |
    CHANGEME — applied guidance. Default body-text color, inherited by
    p / h1-h6 / a.
```

These are **per-token prose blocks**. They carry guidance specific to one token — when to use it, when not to, what its paired token is, what gotchas exist.

**Two fields earn their place:**

- **`$applied-guidance`** — paragraph-length operational advice. "Don't override per-card; card density is a system decision."
- **`$ai-hint`** — guidance specifically for AI consumers (claude.ai/design, MCP design tools). "When generating prose paragraphs, do NOT set color manually — the cascade handles it."

**You don't need to fill these in for every token.** A useful rule:

- **If a token has a non-obvious "when to use" or "when NOT to use," add `$applied-guidance`.**
- **If you've ever explained the same thing twice to an engineer or AI tool, add `$ai-hint`.**
- **Otherwise, leave them blank.** A token with just `$description` is fine.

### Step 11 — Define your modes (if you have any)

Scroll to the `## Modes` section. For each mode you kept in Step 7, there's a body sub-section showing the token redirects:

```token
color.theme.default.bg:
  $ref: "{color.theme.invert.bg}"
  $mode: dark
```

**You usually don't need to edit these.** The template's defaults handle the standard cases:

- Dark mode redirects default-theme → invert-theme
- Alt mode redirects default-theme → alt-theme
- Key mode redirects default-theme → key-theme

If you removed a mode in Step 7, delete its corresponding `### mode-name` sub-section here.

If you have **custom mode behavior** (e.g., a "high-contrast" mode that does something unusual), add a new `### high-contrast` sub-section with your custom redirects, and add `- name: high-contrast` to the `modes:` array in frontmatter.

### Step 12 — Clean up the template's HTML comments

The template ships with extensive `<!-- ... -->` comments explaining each section. Once you've customized the file:

- **Delete the top comment block** (lines 1-30ish) — the "PRISM.md TEMPLATE" banner with usage instructions
- **Delete per-section explanation comments** — anything that starts with `<!-- ` and ends with `-->`
- **Delete the closing banner** at the very bottom

Comments are valid Markdown — leaving them in doesn't break anything — but they bloat the file. Your final PRISM.md should be ~250-500 lines (vs. the template's ~330 with comments).

### Step 13 — Validate (manually for now)

Until the `prism lint` CLI ships, validate by hand:

1. **Open the file in a Markdown preview** (VS Code, GitHub, etc.) — confirm the body renders cleanly. No syntax errors in the YAML frontmatter, no broken Markdown.
2. **Scan for remaining `CHANGEME` placeholders** — `grep -i CHANGEME PRISM.md` should return zero hits (or only in sections you deliberately left as placeholders).
3. **Check that every mode declared in `modes:` has a corresponding `### mode-name` body section.**
4. **Check that every color you reference in mode overlays exists in the primitive `color.*` tree.**
5. **Optional: upload to claude.ai/design** as a `tokens.json` or DESIGN.md and confirm the AI can read it cleanly.

When the CLI ships:

```bash
prism lint PRISM.md
```

…will do all of the above automatically + 17 more checks (14 PRISM.md rules + 7 bundle rules per the spec).

### Step 14 — Emit, share, use

Once the CLI exists:

```bash
prism parse PRISM.md           # produces tokens.json + manifest + bundle
prism emit tailwind-preset     # produces tailwind-preset.js
prism emit design-md           # produces DESIGN.md
prism emit tokens-css          # produces tokens.css
prism emit df-input-css        # produces df-input.css (for Designframe runtime)
```

Each emit produces a file you can hand to whoever needs it — the Tailwind preset to engineers, the DESIGN.md to AI tools, the `tokens.css` to non-Tailwind teams. They all stay in sync because they're emitted from one PRISM.md source.

---

## What you DON'T have to do

Common mistakes designers make on first authoring pass — feel free to skip these:

| Skip this | Why |
|---|---|
| **Filling in every section** | Empty sections are fine. Add prose where it earns its place. |
| **Filling in every `$applied-guidance` and `$ai-hint`** | These are optional. Add them where the guidance is non-obvious. |
| **Worrying about the `space.*` scale** | The defaults are Designframe's standard scale. Don't customize unless your brand needs different rhythm. |
| **Reading the technical spec end-to-end** | You don't need to. Reference it for edge cases. |
| **Hand-crafting every theme context** | The semantic-tier `color.theme.*` tokens have sensible defaults — only customize if your brand needs special theme behavior. |
| **Filling in alt mode if you don't have an alt palette** | Delete the alt mode + alt palette block + alt body section entirely. |

---

## Common patterns + examples

### Pattern 1 — A minimal PRISM.md

If you want the smallest possible PRISM.md that still parses, here's the skeleton:

```yaml
---
format: prism-spec/1.0
name: minimal-system
prismMdVersion: 1.0

modes:
  - name: default
    base: true

color:
  brand:
    key:        { $type: color, $value: "#0066cc", $brand-owned: true }
    primary:    { $type: color, $value: "#000000", $brand-owned: true }
    invert:     { $type: color, $value: "#ffffff", $brand-owned: true }
    background: { $type: color, $value: "#ffffff", $brand-owned: true }
---

# Minimal System

## Overview

A minimal design system used for demonstration purposes.
```

That's it. ~25 lines. The minimum is just enough — you can grow it later by adding tokens and sections as needed.

### Pattern 2 — Single-color brand (no gradient)

```yaml
color:
  brand:
    key:     { $type: color, $value: "#0066cc", $brand-owned: true }
    key-end: { $type: color, $value: "#0066cc", $brand-owned: true }   # same hex
    # ...
```

When `key` and `key-end` are equal, the emitted CSS gradients become solid colors. Tools that expect a gradient still work; they just render as a single color.

### Pattern 3 — Adding a custom token namespace

Beyond DF's standard tokens, you can add project-specific ones. For example, chart colors:

```yaml
color:
  # ... standard brand/brand-alt/alert blocks ...
  charts:
    series-1: { $type: color, $value: "#3883ff", $brand-owned: true }
    series-2: { $type: color, $value: "#fb03b9", $brand-owned: true }
    series-3: { $type: color, $value: "#22c55e", $brand-owned: true }
    # ...
```

Then add `color.charts` to your `brandOwnedRoots:` array. Emitters will produce `--color-charts-series-1` CSS variables, `theme.extend.colors.charts['series-1']` in Tailwind, etc.

### Pattern 4 — A token with rich rationale

```token
color.brand.key:
  $type: color
  $value: "#fb03b9"
  $brand-owned: true
  $description: Brand key color (gradient start). Pink, saturated.
  $applied-guidance: |
    Use as the brand-identity accent — button-gradient start, key-tinted
    backgrounds, link-hover via `.text-key`. When key is intended to be
    a gradient (the default), color.brand.key-end is the paired end-stop.
    Don't apply key as a solid-fill brand color unless the design specifically
    calls for it; default to the gradient pair.
  $ai-hint: |
    When generating brand-key UI elements, prefer gradient utilities
    (bg-gradient-key, button.gradient) over solid bg-key + text-key in
    isolation. The gradient is the brand; solid-key applications can look
    stylistically off without surrounding gradient context.
```

That's a "richly annotated" token. The `$description` is one clause; the `$applied-guidance` is paragraph-length operational advice; the `$ai-hint` is AI-specific. All three together are ~150 words. Use this shape for tokens that earn the explanation; skip it for atomic tokens that don't need it.

### Pattern 5 — Cross-references in prose

When writing prose, you can link to other tokens or sections:

```markdown
The brand identity is a two-stop gradient — [color.brand.key] (pink) to
[color.brand.key-end] (blue) — carrying through buttons, callouts, and
key-themed sections.

See [Colors#brand-palette] for the full gradient pair rationale.
```

The `[token.path]` and `[Section#sub-section]` references become anchor links in emitted output. AI tools follow them. Humans don't have to type them; the spec validator can catch typos.

---

## Anti-patterns (what NOT to do)

### Anti-pattern 1: Inventing new top-level paths without thought

Bad:

```yaml
color:
  myCustomShades:
    pinkish: ...
```

Good — extend DF's namespace conventions:

```yaml
color:
  brand:
    pink-shade-light: ...   # extends the brand namespace
```

OR if it really is a new category:

```yaml
color:
  charts:           # new namespace; document in body why it exists
    series-1: ...
```

The thoughtful version: when you invent a new path, also add it to `brandOwnedRoots` / `systemOwnedRoots` and document the namespace in your Colors body section.

### Anti-pattern 2: Hardcoding hex everywhere instead of using $ref

Bad:

```token
color.theme.invert.bg:
  $type: color
  $value: "#0c0c0c"        # hex hardcoded
```

Good:

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"   # reference; changes if primary changes
```

The `$ref` keeps semantic-tier tokens linked to their primitive source. If you ever change `color.brand.primary`, every `$ref` to it updates automatically.

### Anti-pattern 3: Writing prose that just re-states what the YAML already says

Bad:

```markdown
## Colors

This section has color tokens. The colors are: brand.key, brand.primary,
brand.secondary, ...
```

Good:

```markdown
## Colors

The brand identity is a two-stop pink-to-blue gradient. Monotone text scales
from primary (near-black) through secondary (mid gray) to tertiary (light
gray, still legible). Dark mode flips primary and invert; alert colors live
outside the brand palette and stay system-owned.
```

Prose earns its place by explaining the **why** and the **rules**, not by listing what's in the YAML.

### Anti-pattern 4: Putting brand-customizable values in `systemOwnedRoots`

Bad:

```yaml
brandOwnedRoots: []
systemOwnedRoots: [color, typography, space, radius]
```

Good:

```yaml
brandOwnedRoots: [color.brand, color.brand-alt, typography.family, radius.brand]
systemOwnedRoots: [screen, layout, space, shadow, shader, typography.scale]
```

Marking everything system-owned defeats the brand-customization contract — adopters can't fork your design system because every token is "stable across all consumers." Only mark things `$system-owned` if you mean it.

---

## Glossary (in plain language)

- **DTCG** — "Design Tokens Community Group." The W3C standard for design tokens. PRISM.md emits DTCG JSON when it parses. You don't need to know DTCG to author PRISM.md; the format hides it.
- **Token** — a named value (color, spacing, font size) used as a building block of the design system. `color.brand.key` is a token.
- **Primitive** — a token with a literal value (hex, dimension, number). The "raw" tier.
- **Semantic** — a token that references a primitive via `$ref`. The "role-based" tier. `color.theme.default.fg` is semantic; it points to `color.brand.primary`.
- **Component** — a token specific to one component (e.g., `button.padding`). Deferred to v2; not yet authored in PRISM.md v1.
- **Mode overlay** — a YAML/Markdown block that redirects token references for an alternate "skin" (dark mode, alt palette, key-saturated). Lives in the `## Modes` body section or in a sibling `PRISM.<mode>.md` file.
- **Frontmatter** — the YAML block at the top of the file, between the opening and closing `---` lines.
- **`$ref`** — a reference syntax. `$ref: "{color.brand.primary}"` means "this token's value is the same as color.brand.primary's."
- **`$brand-owned` / `$system-owned`** — flags on each token saying "this is replaceable on fork" or "this is stable across all consumers." Defaults inherited from `brandOwnedRoots` / `systemOwnedRoots` in frontmatter.
- **`$applied-guidance`** — paragraph-length operational advice for humans. "Don't override per-card."
- **`$ai-hint`** — guidance specifically for AI tools. "When generating a button in a dark section, do NOT set bg manually."
- **DESIGN.md** — Google's design-system definition format. PRISM.md is a strict superset of DESIGN.md, so any PRISM.md is also a valid DESIGN.md (just with extras Google's tools ignore).
- **Designframe** — Mike Prasad's minimalist adaptive design system. The reference framework PRISM.md was designed around. PRISM.md works on any design system; Designframe is the canonical example.

---

## Where to get help

- **The template itself** — `spec/PRISM.template.md`. HTML-comment guidance per section.
- **The full format spec** — `spec/prism-md-format.md`. 18 sections covering every edge case. Reference, not read-through.
- **A real worked example** — `df-prism/PRISM.md`. Designframe's own design system in PRISM.md form. Use as a reference for what filled-in prose looks like.
- **The product overview** — `df-prism/PRODUCT.md`. Architecture + pipeline + roadmap.
- **The DESIGN.md spec (external)** — `https://github.com/google/design.md`. Useful for understanding the superset compatibility claim.

If you find a gap in this guide, please surface it — the guide is iterative, and the most useful improvements come from real first-time authoring.

---

## Document history

| Date | Session | Change |
|---|---|---|
| 2026-05-16 | S2 | Created — designer-facing companion to the technical spec + template. |
