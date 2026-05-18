# PRISM.md — Format Specification

> **Last updated:** 2026-05-16 (S2). Format version this spec defines: `prism-spec/1.0`.
>
> **Status:** Draft 1, 2026-05-16. Not yet executable — no parser exists. Defines the contract that future `prism import` / `prism parse` / `prism emit` CLI subcommands will honor.
>
> **Audience:** human authors (designers + developers) writing PRISM.md by hand; AI agents reading PRISM.md as a design system definition; tool authors implementing PRISM.md parsers + emitters.
>
> **Related documents:**
> - [`./designer-guide.md`](./designer-guide.md) — **warm-tone walkthrough for designers authoring a PRISM.md by hand** (start here if you're authoring, not parsing)
> - [`./PRISM.template.md`](./PRISM.template.md) — sharable starter template with CHANGEME placeholders
> - [`../PRISM.md`](../PRISM.md) — worked example: Designframe's own design system in PRISM.md form
> - [`../PRODUCT.md`](../PRODUCT.md) — product identity + end-to-end picture (start here if you're new to Prism overall)
> - [`../PLAN.md`](../PLAN.md) — implementation spec + decision log + session log
> - [`../CLAUDE.md`](../CLAUDE.md) — project context for new Claude Code sessions
> - [`../examples/designframe/`](../examples/designframe/) — the canonical example Prism Kit (Designframe at v0.99 emitted as a complete kit; what `prism parse` + `prism emit` produce)

---

## 1. Purpose & Scope

**PRISM.md** is Designframe Prism's native authoring format — a single Markdown file that fully defines a complete design system: tokens, modes, themes, components, structural rules, and design rationale. It is **the canonical human-authored surface** for design-system definition in Prism.

PRISM.md is:
- **A complete design-system definition** — not just tokens, but the prose, rationale, applied guidance, and AI-targeting hints needed for a system to be understood and used correctly across teams and tools
- **Markdown-first** — readable by humans, parseable by tools, ingestable by AI without normalization gymnastics
- **A strict superset of W3C DTCG** (Design Tokens Community Group format) — every DTCG token type expressible in JSON is expressible in PRISM.md
- **A strict superset of Google DESIGN.md** — any valid DESIGN.md is a valid PRISM.md (just without DF + Prism extensions)
- **The primary input to the Prism pipeline** — `prism extract`, `prism lint`, and `prism emit` all consume PRISM.md as their canonical input

PRISM.md is NOT:
- A runtime — no `<script>` is loaded by browsers
- A schema — the YAML frontmatter + Markdown body have a contract, but it's a content format, not a JSON Schema (use `schema/prism-md.schema.json` if you need machine validation)
- A direct CSS authoring format — `df-input.css` and `tokens.css` are emitted artifacts from PRISM.md

## 2. Design Principles

Five principles govern format decisions. When extending or interpreting this spec, defer to these:

1. **Human-readable first**. A non-technical stakeholder should be able to read PRISM.md and understand the design system. If a feature makes machine-parsing easier but reduces human readability, it doesn't belong.
2. **AI-ingestable second**. The format pattern should be predictable enough that an AI consumer (LLM, design agent, code generator) can parse the structure without per-project tuning. Consistent section ordering + token-block syntax + cross-reference convention are the levers.
3. **Backward compatible with DTCG and DESIGN.md**. The format extends, never breaks, established standards. DTCG token types preserved verbatim; DESIGN.md section order + YAML frontmatter shape preserved as the canonical body structure.
4. **Designframe identity preserved**. DF-native naming (`space.shoulder`, `color.theme.alt.fg`) is canonical; generic vocabulary (`space.md`, `color.fg.muted`) is alias-only. The MVCD-removes-choice philosophy that drives DF carries into PRISM.md.
5. **Single source of truth per project**. There is exactly one PRISM.md per project (or `<project>.PRISM.md` variant). No splitting tokens across files unless using documented `@import` for organization. The whole design system lives in one file you can read top-to-bottom.

## 3. Format Identity & File Discrimination

### 3.1 Filenames

| Form | When to use |
|---|---|
| `PRISM.md` | Default for any project using Prism. Convention is uppercase like `README.md`, `CLAUDE.md`, `DESIGN.md`. |
| `<project>.PRISM.md` | Project-namespaced form when multiple Prism files coexist (e.g., `commonspace.PRISM.md`, `seersite.PRISM.md` in a monorepo). Project name precedes `PRISM.md`. |

Tools MUST accept both forms. The filename does not determine format identity — frontmatter does.

### 3.2 Frontmatter discriminator

Every PRISM.md MUST declare format identity in YAML frontmatter:

```yaml
---
format: prism-spec/1.0
---
```

The `format` field is the authoritative discriminator. If a file has `format: prism-spec/1.0` (or later), it is a PRISM.md regardless of filename. If a file lacks the field, it is NOT a PRISM.md (it may be a vanilla DESIGN.md or arbitrary Markdown; tools should fall back to DESIGN.md parsing per §10 superset contract).

The version after `prism-spec/` follows semver: `1.0`, `1.1`, `2.0`. Parsers MUST accept any version equal to or earlier than their supported version; MAY reject newer.

### 3.3 Extension recommendation

Files MUST end in `.md` so existing Markdown tooling (linters, syntax highlighters, GitHub rendering) works without special configuration. There is no `.prism` or `.dps` extension. Format identity is in frontmatter, not extension.

## 4. Relationship to Other Formats

### 4.1 PRISM.md ⊃ DESIGN.md (strict superset)

Any valid Google DESIGN.md is a valid PRISM.md. Specifically:
- DESIGN.md's 8-section H2 order (Overview, Colors, Typography, Layout, Elevation, Shapes, Components, Do's and Don'ts) is preserved as PRISM.md's canonical body structure
- DESIGN.md's YAML frontmatter token shape is preserved
- DESIGN.md's named aliases (Brand & Style, Layout & Spacing, Elevation) are accepted alongside canonical names
- DESIGN.md's lint rules apply to the DESIGN.md-shaped subset of PRISM.md

A parser that only understands DESIGN.md SHOULD be able to parse a PRISM.md and extract a valid (if reduced) DESIGN.md from it. Tools targeting PRISM.md can re-emit valid DESIGN.md as one of their emitter outputs.

The converse is NOT guaranteed. PRISM.md extends DESIGN.md with:
- Designframe-specific token namespaces (§11)
- Prism-specific fields like `$ai-hint`, `$contrastTarget`, `$brand-owned`, `$df-source` (§12)
- Mode-overlay syntax (§13)
- Component-tier composition rules (§11.4)

A vanilla DESIGN.md parser will see these as unknown frontmatter / unknown YAML keys and SHOULD ignore them per DESIGN.md's "unknown keys are warnings, not errors" rule.

### 4.2 PRISM.md → DTCG JSON (lossless emission)

PRISM.md is a strict superset of DTCG. Every PRISM.md token becomes a DTCG token in emitted `tokens.json`. The mapping is exhaustive (every DTCG token type is expressible) and lossless (no PRISM.md token loses information during DTCG emission).

DTCG fields PRISM.md preserves verbatim:
- `$value`, `$type`, `$description`, `$ref`
- `$extensions` (used for tool-specific metadata)

PRISM.md extensions that emit to DTCG `$extensions`:
- `$ai-hint`, `$df-source`, `$brand-owned`, `$system-owned`, `$contrastTarget`, `$applied-guidance`

Any tool consuming the DTCG output gets the full token tree; only consumers reading `$extensions["prism.ai-hint"]` etc. see the Prism-specific layer.

### 4.3 PRISM.md ↔ df-input.css (asymmetric round-trip)

PRISM.md → df-input.css is **byte-similar** (Decision 2 locked B): same tokens, same values, formatting normalized. The emitted df-input.css passes DF's existing audits (structural-check, ghost-class-check) cleanly.

df-input.css → PRISM.md is **bootstrap-only**: a one-time `prism import df-input.css` operation that produces a PRISM.md draft from an existing DF project. After bootstrap, df-input.css becomes an emitted artifact; the PRISM.md is the editing surface. There is no continuous bidirectional sync.

The asymmetry is structural: PRISM.md carries fields (`$ai-hint`, `$applied-guidance`, full prose rationale) that df-input.css cannot represent. Byte-equivalent round-trip (DTCG terminology: "lossless round-trip") would require enriching df-input.css with PRISM.md-only fields, which defeats the purpose of PRISM.md as the expressive layer.

### 4.4 Authoritative-trajectory summary

| Project state | PRISM.md role | df-input.css role |
|---|---|---|
| New project | Canonical authoring surface; primary input | Emitted artifact (via `prism emit df-input.css`) |
| Existing DF project (pre-migration) | Does not exist yet | Authoritative — current source of truth |
| Existing DF project (post-migration, after `prism import`) | Canonical authoring surface; primary input | Emitted artifact |

There is no project state where both PRISM.md and df-input.css are simultaneously source-of-truth. Migration is one-time and one-directional.

## 5. File-Level Structure

A PRISM.md file has two top-level regions:

```
┌────────────────────────────────────┐
│ Frontmatter (YAML, required)       │  Machine metadata + atomic primitives
│   between leading and trailing --- │
├────────────────────────────────────┤
│ Body (Markdown, optional)          │  Prose + semantic/component tokens
│   from first H1 onward             │  with applied-guidance and rationale
└────────────────────────────────────┘
```

The frontmatter MUST be the first content in the file. The body MAY be empty (frontmatter-only PRISM.md is valid — equivalent to a tokens-only DTCG bundle without rationale).

### 5.1 Region responsibilities

| Region | Carries | Read-fast properties |
|---|---|---|
| **Frontmatter** | Format identity, project metadata, atomic primitives (colors, dimensions with single literal values), mode declarations, brand vs system classification roots | Parseable by any YAML parser in one pass; tools can extract metadata without walking body |
| **Body** | Section overviews, semantic-tier tokens (theme contexts, fg/bg pairs), component tokens, rationale prose, do's and don'ts, AI hints | Carries the "design intent" layer; ignorable if a consumer only needs tokens |

### 5.2 Splitting and imports

PRISM.md is designed to be a single file. For very large design systems, an `@import` directive in frontmatter allows splitting:

```yaml
---
format: prism-spec/1.0
imports:
  - PRISM.colors.md
  - PRISM.typography.md
---
```

Imported files MUST themselves be valid PRISM.md (with `format: prism-spec/1.0` declared). Imports are flattened at parse time; emitted DTCG is unified. There is no namespacing — duplicate token paths across imports are validation errors.

Defer using `@import` until single-file PRISM.md genuinely doesn't fit. Splitting prematurely creates the same fragmentation problem df-input.css has.

## 6. Frontmatter Schema

The frontmatter is a YAML object with required + optional top-level keys.

### 6.1 Required fields

```yaml
format: prism-spec/1.0          # See §3.2
name: my-design-system           # Project name (kebab-case)
prismMdVersion: 1.0              # Same as format version
```

### 6.2 Optional metadata

```yaml
version: 1.0.0                   # Project version (semver)
description: |
  One-paragraph description of this design system.
license: MIT                     # SPDX identifier or "proprietary"
homepage: https://example.com
designframeVersion: 0.99         # If targeting a specific DF version
designMdCompatible: true         # Declare DESIGN.md superset compliance
bootstrapped-from: src/assets/designframe/df-input.css   # Set by `prism import`
bootstrapped-at: 2026-05-16T17:00:00Z                    # ISO-8601 timestamp of import
```

**`bootstrapped-from`** (set by `prism import`, never hand-edited): path to the `df-input.css` (or equivalent DF source root) that seeded this PRISM.md. Presence of this field marks the file as bootstrap-derived. Re-running `prism import` against an already-bootstrapped file is an error unless `--overwrite` is passed.

**`bootstrapped-at`** (set by `prism import`): ISO-8601 timestamp of the bootstrap operation. Lets tools detect stale bootstraps when the underlying DF source has been edited.

### 6.3 Brand customization roots

```yaml
brandOwnedRoots:                 # Paths under these roots are $brand-owned by default
  - color.brand
  - color.brand-alt
  - typography.family
  - radius.brand
systemOwnedRoots:                # Paths under these roots are $system-owned by default
  - screen
  - layout
  - space
  - shadow
  - shader
  - typography.scale
```

These declare the default `$brand-owned` / `$system-owned` flag for tokens under each root. Individual tokens can override via explicit `$brand-owned: true/false` in their definition.

### 6.4 Mode declarations

```yaml
modes:
  - name: default
    description: Light mode, main palette (also the base mode — no overlay needed)
    base: true
  - name: dark
    description: Dark mode using main palette
    aliases: [invert]            # DF-native synonym
  - name: alt
    description: Alt palette, light mode
  - name: key
    description: Key brand color as canvas (saturated bg)
```

Each declared mode (except the base) MUST be defined either inline in §13 mode-overlay sections, or in a sibling overlay file (`PRISM.<mode>.md`).

### 6.5 Atomic primitives (the bulk of frontmatter)

Primitive tokens with pure literal values live in frontmatter under their DTCG-tree path:

```yaml
color:
  brand:
    key:
      $type: color
      $value: "#fb03b9"
      $brand-owned: true
      $description: Brand key color (gradient start).
      $df-source: --qs-color-key
    primary:
      $type: color
      $value: "#0c0c0c"
      $brand-owned: true
      $df-source: --qs-color-primary

space:
  unit:
    $type: dimension
    $value: 4px
    $system-owned: true
    $df-source: --layout-unit
  gutter:
    base:
      $type: dimension
      $value: 24px
      $system-owned: true
      $df-source: --spacing-gutter-base
```

**Frontmatter-vs-body decision rule (canonical):**

A token belongs in **frontmatter** if and only if ALL of the following hold:

1. It has a single `$value` literal OR a single `$ref` reference (not both)
2. Its `$description` (if present) is ≤ 200 characters (a single short clause)
3. It has no `$applied-guidance` field
4. It has no `$ai-hint` field
5. It is not a composite-type token (shadow, typography, border, transition, gradient — see §9.3)

If ANY condition fails, the token belongs in a **body** ` ```token ` block (§8). Composite types ALWAYS go in body since their `$value` is an object, not a literal — putting them in frontmatter produces unreadably-dense YAML.

This rule is enforceable by validator: a future PRISM.md lint rule (`frontmatter-misplacement`, candidate v1.1) can flag tokens in frontmatter that exceed the criteria and suggest body placement.

### 6.6 Source file pointers

```yaml
source:
  dfInputCss: src/assets/designframe/df-input.css
  tailwindConfig: tailwind.config.js
  dfPreset: src/assets/designframe/df-preset.js
  rationaleSources:
    - docs/df-rules.md
    - docs/df-knowledge.md
    - docs/taxonomy.md
```

Used by `prism import` to know what to read when bootstrapping from a DF project. Optional — only relevant for projects that have an underlying DF source.

## 7. Section Taxonomy & Canonical Order

The body uses H2 (`##`) headings to define sections. Canonical order is the DESIGN.md 8-section sequence plus 3 PRISM.md additions, in this order:

| # | Heading | Aliases accepted | What lives here |
|---|---|---|---|
| 1 | `## Overview` | `## Brand & Style` | Design philosophy, voice, audience framing |
| 2 | `## Colors` | — | Palette story, theme contexts (semantic tier), shader system, alert colors, fg/bg pairing rule |
| 3 | `## Typography` | — | Font strategy, type scale rationale, line-height anchor explanation |
| 4 | `## Layout` | `## Layout & Spacing` | Grid system, semantic spacing contexts, adaptive vs responsive class explanation, block hierarchy |
| 5 | `## Elevation & Depth` | `## Elevation` | Shadow tiers, layering rules, blur scale |
| 6 | `## Shapes` | — | Border-radius system, modifier combinations |
| 7 | `## Components` | — | Component-tier tokens, combo class pattern, free vs premium boundary |
| **8** | `## Assets` | — | **PRISM.md addition (spec/1.0)** — brand assets (logo, marks, icon sets) with usage rules. See §19 for asset-block grammar. Optional. |
| **9** | `## Modes` | `## Mode Overlays` | **PRISM.md addition** — mode-specific reassignments (see §13). Optional; if absent, only base mode is defined. |
| **10** | `## Components — Premium` | `## Premium Components` | **PRISM.md addition** — DF-specific `df-*` premium components (separate from free §7). Optional. |
| **11** | `## Lossiness & Constraints` | — | **PRISM.md addition** — declared lossiness when this PRISM.md gets emitted to specific consumer targets. Required if emitting to multiple targets. |
| 12 | `## Do's and Don'ts` | — | Rules that can't be token-encoded (semantic HTML, block hierarchy, hardcoded-value anti-patterns). Trailing section — closes the document with the cross-cutting rule list. |

Sections MAY be omitted (no warning) but MUST appear in canonical order when present. Out-of-order sections trigger PRISM.md lint rule `section-order` (warning severity).

Aliases are accepted by parsers; emitters MAY normalize to canonical names.

### 7.1 H3 sub-sections

Within each H2 section, authors organize with H3 (`###`) sub-headings. There is no enforced H3 taxonomy — sub-sections are scoped to the parent and serve organizational purposes. Common H3 patterns:

- Under `## Colors`: `### Brand palette`, `### Alert colors`, `### Shaders`
- Under `## Typography`: `### Display scale`, `### Heading scale`, `### Body scale`
- Under `## Components`: `### Button`, `### Card`, `### Form input`

## 8. Token-Block Grammar

Body tokens (semantic, component, or any token with prose rationale beyond a one-line `$description`) live in fenced code blocks tagged `token`.

### 8.1 Syntax

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: |
    Inverted theme background. In dark mode, sections with .theme.invert
    use this as their bg color. Pairs with color.theme.invert.fg via the
    fg/bg pairing rule.
  $applied-guidance: |
    Use when a section needs dark treatment (hero with white text on
    near-black, contrast-establishing footer). Do not override per-section
    — invert is a system-level theme decision; per-section overrides defeat
    the cascade.
  $ai-hint: |
    When generating code for a dark section, prefer applying .theme.invert
    on the section element rather than setting bg + fg colors directly.
    The class triggers a full atomic invert (card auto-revert, key↔invert
    component swap) that manual color setting misses.
  $df-source-context: theme-invert cascade (df-input.css ANCHOR Theming)
```

### 8.2 Block structure

Each `token` block contains exactly one token definition. The block is parsed as YAML:

- The first key is the token path (DTCG dotted-path notation)
- The value is an object with `$`-prefixed keys following DTCG + PRISM.md extensions
- Multi-line strings use YAML's `|` (literal) or `>` (folded) blocks
- Cross-references use `$ref: "{path.to.token}"` syntax (see §14)

### 8.3 Block placement

Token blocks live underneath the H3 sub-section that organizes them. Adjacent prose explains the family or category; individual tokens carry their per-token applied guidance and AI hints inside the block.

Pattern:

```markdown
### Brand palette

DF uses a brand-key gradient as the identity accent — a two-stop gradient
(key → key-end) carries through buttons, callouts, link hovers, and
key-themed sections. The pair is intentional: solo-key applications
should be rare.

```token
color.brand.key:
  $type: color
  $value: "#fb03b9"
  $brand-owned: true
  $description: Brand key color (gradient start).
  $df-source: --qs-color-key
```

```token
color.brand.key-end:
  $type: color
  $value: "#3883ff"
  $brand-owned: true
  $description: Brand key color (gradient end).
  $df-source: --qs-color-key-end
```

[continuing paragraph...]
```

### 8.4 Multiple tokens in one block (discouraged)

YAML permits multiple top-level keys in one document. PRISM.md SHOULD NOT use this — one token per block keeps cross-reference targeting unambiguous and per-token prose adjacent to its own block. The validator MAY warn on multi-token blocks.

## 9. W3C DTCG Compatibility & Type Mapping

PRISM.md preserves every DTCG token type. The `$type` field uses DTCG vocabulary verbatim.

### 9.1 Direct DTCG types

| `$type` | `$value` shape | Example |
|---|---|---|
| `color` | Hex string `"#rrggbb"` or rgba object | `"#fb03b9"` |
| `dimension` | String with unit | `"24px"`, `"1.5rem"` |
| `fontFamily` | String or array of strings | `["headingFontFamily", "sans-serif", "system-ui"]` |
| `fontWeight` | Number or named token | `400`, `bold` |
| `number` | Numeric literal | `0.15`, `1.5` |
| `string` | String literal | `"right top"` |
| `duration` | String with `ms` or `s` unit | `"300ms"` |
| `cubicBezier` | 4-number array | `[0.4, 0, 0.2, 1]` |
| `shadow` | Object with offsetX/Y, blur, spread, color | See §9.3 |
| `typography` | Object with fontFamily, fontWeight, fontSize, lineHeight | See §9.3 |
| `border` | Object with width, style, color | See §9.3 |
| `transition` | Object with duration, timing-function | See §9.3 |
| `gradient` | Array of color stops or DTCG gradient object | See §9.3 |

### 9.2 String-form vs object-form dimensions

DTCG is moving toward an object-typed value form (`{ "value": 16, "unit": "px" }`); the alpha spec also accepts the string form (`"16px"`). **PRISM.md uses string form** for tool-compatibility breadth. The mechanical transform to object form happens at emission time per target.

### 9.3 Composite types

Composite types (shadow, typography, border, transition, gradient) use DTCG object-form `$value`:

```token
shadow.base:
  $type: shadow
  $value:
    offsetX: 0px
    offsetY: 2px
    blur: 4px
    spread: 0px
    color: rgba(0,0,0,0.25)
  $description: Base shadow tier.
```

### 9.4 Unknown types

Tokens with `$type` values outside the DTCG-defined set are valid PRISM.md but emit to DTCG `$extensions["prism.type"]` rather than `$type` (which DTCG would reject as unknown). Use sparingly — prefer extending via `$ai-hint` / `$applied-guidance` instead of inventing new types.

## 10. DESIGN.md Superset Contract

### 10.1 What PRISM.md inherits from DESIGN.md

The following are 1:1 inherited:

1. **8-section H2 order** (Overview → Do's and Don'ts) per §7
2. **Section name aliases** (Brand & Style, Layout & Spacing, Elevation) per §7
3. **YAML frontmatter shape** — DESIGN.md's frontmatter token tree is valid PRISM.md frontmatter (just without the `format: prism-spec/1.0` discriminator and PRISM.md extensions)
4. **MD3-style naming convention** as an accepted alias surface — `color.primary`, `color.on-primary`, `color.surface-container-*` are valid PRISM.md alias paths under `color._aliases` (per the DESIGN.md ecosystem convention)
5. **Lint rules from `@google/design.md`** — apply to PRISM.md within the DESIGN.md-shaped subset; failures map to PRISM.md lint output

### 10.2 What PRISM.md adds beyond DESIGN.md

1. **`format` frontmatter discriminator** — DESIGN.md does not declare format identity; PRISM.md does
2. **DF-specific token namespaces** — `color.theme.{default,alt,invert,key}.{bg,fg}`, `space.{gutter,shoulder,article,...}`, etc. (§11)
3. **`$brand-owned` / `$system-owned` flags** — explicit fork-contract markers
4. **`$ai-hint`, `$applied-guidance`, `$df-source` fields** — applied-guidance + provenance layer (§12)
5. **Mode overlay syntax** — `## Modes` section with reassignment grammar (§13)
6. **Component-tier composition** — combo-class rules in `## Components` (§11.4)
7. **`@import` directive** — file-splitting for very large systems (§5.2)
8. **`source` frontmatter block** — pointers to underlying DF source for `prism import` (§6.6)
9. **Cross-reference syntax in prose** — `[token.path]` inline references in body text (§14.2)

### 10.3 Round-trip with DESIGN.md

`prism emit design-md` produces a valid `DESIGN.md` from any PRISM.md. The emission:
- Preserves the 8 canonical sections in order
- Maps DF-native token names to MD3 aliases (e.g., `color.theme.default.fg` → `color.on-primary`) via the manifest's MD3 crosswalk
- Drops PRISM.md-only frontmatter fields and `$ai-hint` / `$applied-guidance` (these emit to DTCG `$extensions` for tools that read DTCG, but DESIGN.md has no carrier)
- Preserves prose rationale verbatim within the canonical 8 sections; PRISM.md additions (§9 Modes, §10 Components-Premium, §11 Lossiness) are dropped (DESIGN.md has no equivalent surface)

The lossy fields are captured in the `manifest.lossiness[]` entry `design-md-superset-drop` (added by the design-md emitter).

## 11. Designframe Native Extensions

Tokens with DF-specific semantics. These extend DTCG with DF's structural identity.

### 11.1 Token namespaces

| Namespace | Purpose | DF source |
|---|---|---|
| `color.brand.*` | Brand-owned palette (key, primary, secondary, tertiary, invert, disabled, background) | `--qs-color-*` |
| `color.brand-alt.*` | Alt-palette equivalents | `--qs-color-*-alt` |
| `color.alert.{notify,warning,error,success}.{bg,fg}` | Alert color pairs | `--color-alert-*` |
| `color.theme.{default,alt,invert,key,key-gradient,...}.{bg,fg}` | Semantic theme contexts | df-rules.md §10 |
| `color.theme.{header,footer}.{bg,fg,nav-link}` | Landmark theme contexts | `--qs-color-header-*` |
| `space.{unit,gutter,shoulder,section,article,element,sub,stack,form,hgroup}` | Semantic spacing contexts | `--spacing-*`, `--pad-*`, `--gap-*` |
| `size.element.{hairline,sub,min,compact,base}` | Element dimensional sizing | `--element-*-size` |
| `radius.brand.{min,base,corner,field}` | Brand-owned rounding | `--qs-rounded-*` |
| `radius.{article,full,chip}` | Derived rounding | `--rounded-*` |
| `typography.family.{heading,body,display}` | Font role assignments | `--font-*` |
| `typography.scale.{base,d1-d3,h1-h6,p1-p5,i1-i6,nav,button,chip}.{size,lh}` | Type scale | `--text-*` |
| `shadow.{light,base,heavy}` | Shadow scale | `--shadow-*` |
| `shadow.alpha.{light,base,heavy}` | Shadow alpha sub-tokens | `--shadow-alpha-*` |
| `shader.{light,base,heavy}` | Shader alpha values | `--shader-*` |
| `blur.{light,base,heavy,max}` | Blur radii | `--blur-*` |
| `border.{base,button,form}` | Border widths | `--border-*` |
| `transition.{duration,timing,property}` | Transition defaults | `--transition-*-base` |
| `screen.{sm,md,lg,xl,2xl}` | Breakpoints | `--screen-*` |
| `layout.{cols,col-width}.{base,sm,md,lg,xl,2xl}` | Grid columns and widths | `--layout-*` |

Each namespace MUST follow DF's existing structure if extracting from a DF project. New namespaces (for PRISM.md-first projects with no DF source) follow the same naming pattern conventions.

### 11.2 DF-specific token fields

```yaml
$df-source: --qs-color-key           # Source CSS variable in df-input.css
$df-source-context: ANCHOR Quick Setup Variables  # Section where the source lives
$df-cascade-layer: brand-setter      # One of: brand-setter, runtime, theme-scoped
```

`$df-source` is the most-used; it provides bidirectional traceability between PRISM.md tokens and df-input.css variables. Required for any token extracted from DF source; optional for PRISM.md-first projects.

### 11.3 fg/bg pairing rule (DF-inherited)

Every token at the semantic tier whose path ends in `.bg` MUST have a paired token at the same path ending in `.fg`. Validator enforces; this is DF's contrast-ratio + accessibility convention encoded in the format.

Exception: transparent backgrounds (where `$value: "transparent"` or the bg is paired with a parent context's bg). These declare `$contrastTarget: "decorative"` to suppress contrast-ratio validation per §12.

### 11.4 Component composition

Component-tier tokens reference primitives + semantic tokens via `$ref`. PRISM.md does NOT (in v1.0) define the combo-class grammar — that's v2's `compositions/` layer. Component tokens here are pure tokens (button.padding, card.radius, header.height) with `$ref` to their underlying primitives.

```token
button.padding-x:
  $type: dimension
  $ref: "{space.element.base}"
  $description: Button horizontal padding.
  $applied-guidance: |
    Apply via the .button combo class — never inline. Compositional
    overrides (.button.gradient, .button.air) layer on top without
    re-declaring padding.
```

## 12. Prism Native Extensions

Fields PRISM.md adds beyond DTCG + DESIGN.md, for design intent + AI ingestion + applied guidance.

### 12.1 `$ai-hint`

Plain-language guidance directed at AI consumers (LLMs, code generators, design agents). Distinct from `$description` (general human description) and `$applied-guidance` (general human applied advice) — `$ai-hint` is specifically the "what an AI consumer needs to know to use this token correctly that isn't obvious from name + value."

```yaml
$ai-hint: |
  When generating a button component in a dark section, do NOT set bg or
  text colors manually — apply .theme.invert on the parent section and
  let DF's auto-invert cascade flip button colors. Manual flipping breaks
  on theme-key sections where the cascade does something different.
```

Used by claude.ai/design and other AI design tools to avoid common DF authoring mistakes.

### 12.2 `$applied-guidance`

Operational guidance — when to use, when not to use, paired-token relationships, edge-case behavior. Longer-form than `$description`; can be multi-paragraph.

```yaml
$applied-guidance: |
  Use for any .block-card content inset. The 16px value at base + 24px at xl
  is paired with space.gutter's 24px/32px — internal padding ≤ block gutter
  (per df-rules.md §9.5 cascade).
  
  Do not override per-card; card density is a system-level decision. If a
  specific card pattern needs different padding (compact card, full-bleed
  card), define a new card variant token (card.padding-compact, card.padding-bleed)
  rather than overriding card.padding inline.
```

### 12.3 `$brand-owned` / `$system-owned`

Boolean flags. Exactly one MUST be true; both being true or both being false is a validation error. Default per token depends on `brandOwnedRoots` / `systemOwnedRoots` declarations in frontmatter (§6.3).

```yaml
$brand-owned: true     # Replaceable when forking for a new brand
$system-owned: false   # Implicit when $brand-owned: true
```

### 12.4 `$contrastTarget`

For colors used in fg/bg pairs. Tells the `contrast-ratio` lint rule what WCAG threshold to apply.

| Value | WCAG threshold | Use case |
|---|---|---|
| `body` (default) | ≥ 4.5:1 | Standard body text |
| `large` | ≥ 3:1 | Large text (≥18pt regular or ≥14pt bold) |
| `decorative` | (skipped) | Color is decorative or pairs with parent surface; contrast not enforced |

### 12.5 `$df-source` (covered in §11.2)

### 12.6 `$rationale-anchor`

Optional link from a token to a section in the body where it's discussed in prose. Lets emitters cross-reference token → rationale section.

```yaml
$rationale-anchor: Colors#brand-palette
```

Anchor uses `Section#sub-section` notation matching the body H2/H3 heading hierarchy.

## 13. Mode Overlays

PRISM.md supports modes (dark, alt, invert, key) inline in the body under `## Modes`, OR as sibling files (`PRISM.dark.md`, `PRISM.alt.md`).

### 13.1 Inline mode syntax

Under `## Modes`, each declared mode (per frontmatter §6.4) has an H3 sub-section. Within, `token` blocks specify only the tokens whose values change for that mode.

```markdown
## Modes

### dark

In dark mode, the default theme context redirects to the invert context.
Brand primitives are unchanged; semantic-tier tokens reassign.

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

Alt palette mode — every brand color redirects to its brand-alt equivalent.

[tokens...]
```

### 13.2 Sibling-file mode syntax

For large mode definitions, use sibling files:

```
PRISM.md            # base mode (default)
PRISM.dark.md       # dark mode overlay
PRISM.alt.md        # alt mode overlay
PRISM.key.md        # key mode overlay
```

Each sibling MUST declare `format: prism-spec/1.0` + `mode: <name>` in frontmatter:

```yaml
---
format: prism-spec/1.0
mode: dark
extends: PRISM.md
---
```

The body contains token blocks (no need for `## Modes` wrapping) — every token in the file is implicitly tagged with the mode declared in frontmatter.

### 13.3 Inline vs sibling choice

| Use inline `## Modes` when | Use sibling files when |
|---|---|
| Modes are small (5-15 tokens each) | Modes are large (50+ tokens) |
| All modes ship together always | Modes can ship independently |
| Editing surface is one file | Editing surface benefits from separation |

Both produce identical emitted DTCG output. Choice is authoring ergonomics, not semantics.

### 13.4 `$mode` field

In inline mode syntax, every token block under a mode H3 sub-section has an implicit `$mode: <name>` field. Authors MAY declare it explicitly for clarity. In sibling files, `$mode` is implicit from frontmatter.

### 13.5 Declared-but-undefined modes

Every mode declared in frontmatter `modes[]` (per §6.4) MUST be defined either inline (under `## Modes` per §13.1) or as a sibling file (`PRISM.<mode>.md` per §13.2). A declared-but-undefined mode is a validation error — lint rule `mode-undefined` (severity: error) fires when a mode appears in frontmatter without a matching definition.

The base mode (the mode flagged with `base: true` in frontmatter) does NOT require an overlay definition — it IS the base file's token tree. Exactly one mode in `modes[]` MUST have `base: true`; more than one or zero is a validation error.

Aliases declared in frontmatter mode entries (`aliases: [invert]` on `name: dark`) bind the alias to the same definition. A consumer asking for the `invert` mode receives the `dark` mode's tokens. Aliases do not need their own sibling files or inline definitions.

## 14. Cross-References

### 14.1 `$ref` in token blocks (DTCG-standard)

Token-to-token references use the DTCG `$ref` field with curly-brace path syntax:

```yaml
$ref: "{color.brand.primary}"
```

The path is the dotted DTCG path to the target token. Quotes around the path are required (it's a YAML string, not an object).

### 14.2 Inline references in prose

Body prose can reference tokens with square-bracket syntax:

```markdown
The [color.brand.key] color pairs with [color.brand.key-end] in DF's gradient
buttons.
```

These are anchor links; emitters MAY transform them to HTML anchors pointing at the relevant token block. Validators check that bracketed paths resolve to defined tokens (broken-ref equivalent for prose).

### 14.3 Section references

Sections referenced from prose use `Section#sub-section` notation:

```markdown
See [Colors#brand-palette] for the full gradient pair rationale.
```

Same path convention as `$rationale-anchor` per §12.6.

## 15. Validation Rules

PRISM.md validation runs alongside DTCG bundle validation (lint rules in `lint-rules.md`). PRISM.md-specific rules:

| # | Rule | Severity | Check |
|---|---|---|---|
| 1 | `format-declared` | error | Frontmatter contains `format: prism-spec/<version>` |
| 2 | `frontmatter-yaml-valid` | error | Frontmatter parses as YAML without errors |
| 3 | `token-block-yaml-valid` | error | Every ` ```token` fence parses as YAML |
| 4 | `token-path-unique` | error | No two token blocks define the same path within the same `$mode` bucket (mode-overlay redefinitions across modes are legitimate — see note below) |
| 5 | `ref-resolvable` | error | Every `$ref` resolves to a defined token |
| 6 | `prose-ref-resolvable` | warning | Every `[token.path]` in prose resolves to a defined token or section |
| 7 | `brand-or-system-owned` | error | Every token has exactly one of `$brand-owned: true` or `$system-owned: true` (after defaults from frontmatter roots) |
| 8 | `fg-bg-paired` | error | Every `color.*.bg` has a paired `color.*.fg` (except `$contrastTarget: "decorative"`) |
| 9 | `section-order` | warning | H2 sections appear in canonical order per §7 |
| 10 | `mode-declared` | error | Tokens with `$mode: X` appear in a mode declared in frontmatter §6.4 |
| 11 | `mode-undefined` | error | Every mode in frontmatter `modes[]` (except aliases) has an inline `## Modes` definition OR a sibling `PRISM.<mode>.md` file. Exactly one mode has `base: true`. |
| 12 | `df-source-reachable` | warning | When `$df-source` is set, the source file in `source.dfInputCss` (if declared) contains that variable |
| 13 | `dtcg-type-valid` | error | `$type` value is in the DTCG-defined set OR `$extensions.prism.type` is used |
| 14 | `design-md-compatible` | info | When `designMdCompatible: true` is declared, file passes `npx @google/design.md lint` on the DESIGN.md-shaped subset |
| 15 | `asset-path-exists` | error | Every `## Assets` declaration's `$path` resolves to an existing file under `assets/` (added 2026-05-16 with §19) |
| 16 | `asset-clear-space-token-resolvable` | warning | Asset `$clear-space` + `$min-size` token refs resolve to a defined token (added 2026-05-16 with §19) |
| 17 | `asset-type-valid` | error | Asset `$type` is in the valid asset-type set (added 2026-05-16 with §19) |
| 18 | `description-too-thin` | info | `$description` field is non-empty and ≥ 10 chars; not a TODO/TBD/FIXME stub. Exempts canonical-terse forms like `"≥640px."` / `"<768px."` (DF responsive-variant shorthand where the breakpoint is the description). Aspirational quality, added 2026-05-18 |
| 19 | `description-missing-applied-guidance` | info | Tier-1 tokens (top-5 brand identity + 4 primary theme `.bg`/`.fg` pairs) with `$description` should also have `$applied-guidance` for AI-tool consumers (aspirational, added 2026-05-18) |
| 20 | `mode-overlay-completeness` | warning | Every non-base mode in `frontmatter.modes[]` has ≥ 1 body token-fence with `$mode: <name>` — declared-but-empty mode overlays are flagged (added 2026-05-18) |
| 21 | `brand-vs-system-conflict` | error | A token's `$brand-owned` / `$system-owned` flag must agree with its position in `brandOwnedRoots` / `systemOwnedRoots` — flags a token marked `$system-owned` under a brand root (or vice versa) (added 2026-05-18) |

These rules run via `prism lint <PRISM.md>`. Composability with bundle lint rules (the 7 rules in `lint-rules.md`) is documented in lint runtime: PRISM.md rules fire first; if they pass, the file is parsed into DTCG and bundle rules fire on the emitted bundle.

**Note on Rule 4 `token-path-unique` (added 2026-05-17):** uniqueness applies per `(path, $mode)` bucket, not per `path` alone. The same logical token (e.g. `color.theme.default.bg`) may legitimately be redefined in each declared mode — that's the entire purpose of mode overlays per §13. The rule fires only when two token blocks share the same path AND the same `$mode` value (or both lack `$mode`, i.e. both belong to the base mode). This clarification reflects the broad-reading interpretation the parser implements; the original prose was ambiguous between narrow (path-only) and broad (per-mode) semantics. Surfaced via Checkpoint #4 implementation against the real Designframe PRISM.md.

## 16. Round-Trip Semantics

### 16.1 PRISM.md → DTCG (lossless)

Every PRISM.md token becomes a DTCG token in `tokens.json`. PRISM.md extensions (`$ai-hint`, `$applied-guidance`, `$df-source`, `$contrastTarget`, `$mode`) emit to DTCG `$extensions["prism.*"]` per §4.2.

Round-trip stability: `PRISM.md → DTCG → PRISM.md` is **stable but not byte-identical** (formatting may normalize: YAML quote style, key ordering, code-fence whitespace). Semantically identical. Authors should treat DTCG as a derived artifact, not a hand-editing surface.

### 16.2 PRISM.md → df-input.css (byte-similar, per D2 lock)

`prism emit df-input.css` produces a valid df-input.css that:
- Defines every PRISM.md token as a `--var` under the appropriate CONFIG section
- Preserves DF naming (`--qs-*` for brand-owned, bare `--*` for system-owned)
- Passes DF's structural-check + ghost-class-check audits
- Drops PRISM.md-only fields (`$ai-hint`, `$applied-guidance`, full rationale) — these have no CSS carrier

Round-trip semantics: `PRISM.md → df-input.css → PRISM.md` (via `prism import`) is **lossy** — `$ai-hint` + `$applied-guidance` + non-`$description` prose are dropped. Authors should treat df-input.css as a derived artifact; don't edit it after emission.

### 16.3 df-input.css → PRISM.md (bootstrap only)

`prism import df-input.css` is a **one-time** operation that produces a PRISM.md draft from an existing DF project. Behavior:
- Parses df-input.css `:root` blocks → extracts tokens
- Reads df-preset.js → extracts theme-extend palette
- Reads df-rules.md / df-knowledge.md / taxonomy.md → seeds rationale sections
- Outputs PRISM.md with: complete frontmatter (all atomic primitives), skeletal body sections (with TODO markers for prose), source pointers populated
- Marks the file with `bootstrapped-from: <df-input.css path>` in frontmatter

After bootstrap, the PRISM.md is the editing surface. Re-running `prism import` is an error unless `--overwrite` is passed (prevents accidental data loss for hand-authored PRISM.md edits).

### 16.4 PRISM.md → emitters

All bundle emitters (`tailwind-preset.js`, `tokens.css`, `figma-tokens-studio.json`, `design-md.md`) read from the DTCG produced in §16.1. The PRISM.md → DTCG step is the canonical pivot; emitters never read PRISM.md directly.

## 17. Worked Example

A complete-enough PRISM.md exercising the major features. This file is simultaneously a valid PRISM.md, a valid DESIGN.md (the DESIGN.md-shaped subset parses), and a complete-enough design system to emit a DTCG bundle from.

````markdown
---
format: prism-spec/1.0
name: example-design-system
version: 0.1.0
prismMdVersion: 1.0
designframeVersion: 0.99
designMdCompatible: true
description: |
  A minimal example design system demonstrating PRISM.md core features.

brandOwnedRoots: [color.brand, color.brand-alt, typography.family, radius.brand]
systemOwnedRoots: [screen, layout, space, shadow, shader, typography.scale]

modes:
  - { name: default, base: true }
  - { name: dark, aliases: [invert] }

color:
  brand:
    key:
      $type: color
      $value: "#fb03b9"
      $brand-owned: true
      $df-source: --qs-color-key
    key-end:
      $type: color
      $value: "#3883ff"
      $brand-owned: true
      $df-source: --qs-color-key-end
    primary:
      $type: color
      $value: "#0c0c0c"
      $brand-owned: true
      $df-source: --qs-color-primary
    secondary:
      $type: color
      $value: "#888888"
      $brand-owned: true
    invert:
      $type: color
      $value: "#ffffff"
      $brand-owned: true
    background:
      $type: color
      $value: "#ffffff"
      $brand-owned: true

space:
  gutter:
    base: { $type: dimension, $value: "24px", $system-owned: true }
    xl:   { $type: dimension, $value: "32px", $system-owned: true }
---

# Example Design System

## Overview

This is a minimalist design system anchored on a brand-pink gradient (key)
with monotone text hierarchy.

## Colors

### Brand palette

The brand identity is a two-stop gradient — [color.brand.key] (pink) to
[color.brand.key-end] (blue). Solo applications of either are rare; the
gradient is the visual signature.

### Theme contexts

Default theme uses [color.brand.background] as bg + [color.brand.primary]
as fg. Dark mode reassigns these via mode overlay.

```token
color.theme.default.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Default theme background.
```

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground.
  $applied-guidance: |
    Default body text color. Pairs with color.theme.default.bg.
  $ai-hint: |
    When generating prose paragraphs, do not set color manually — the
    default p element inherits this via cascade.
```

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Inverted theme background (dark mode).
```

```token
color.theme.invert.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Inverted theme foreground (dark mode).
```

## Typography

[Typography section content...]

## Layout

[Layout content...]

## Modes

### dark

In dark mode, default-theme contexts redirect to invert-theme contexts.
Brand primitives don't change; semantic refs do.

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

## Do's and Don'ts

- **Don't** hardcode color values; use [color.theme.default.fg], etc.
- **Don't** override theme contexts per-component; they are system-level.
- **Do** apply .theme.invert at the section level to flip a section dark.
````

This 80-ish-line file would emit:
- A complete DTCG `tokens.json` with 11 tokens (6 brand primitives, 2 spacing primitives, 4 semantic theme tokens, 2 of which have applied-guidance + ai-hint fields)
- A `tokens.dark.json` overlay with 2 redirects
- A valid `df-input.css` with `--qs-*` brand vars + `--spacing-gutter-*` system vars
- A valid `DESIGN.md` (passing `@google/design.md lint`) with 4 sections (Overview, Colors, Typography, Layout) plus Do's and Don'ts

## 18. Versioning, Parser Model, & Open Questions

### 18.1 Format versioning

`format: prism-spec/X.Y` follows semver:
- **Patch (X.Y.Z)**: Documentation clarification; no parser changes required
- **Minor (X.Y)**: Additive fields or sections; older parsers MAY warn on unknown additions but MUST NOT error
- **Major (X.0)**: Breaking changes; older parsers MUST reject

The version this spec defines is **`prism-spec/1.0`**.

### 18.2 Parser reference model

A conforming parser performs the following passes:

```
1. Read file → split frontmatter (between leading/trailing ---) from body
2. Parse frontmatter as YAML → extract metadata + atomic primitives → produce initial token tree
3. Walk body H2 sections in order → identify section identity (canonical or alias)
4. For each section, walk content:
   a. Plain prose → append to section's prose buffer
   b. ```token fence → parse YAML, add token to tree at declared path
5. Apply $mode tagging from §13 inline mode sections
6. Resolve $ref values → validate against §15 rule ref-resolvable
7. Apply default $brand-owned / $system-owned from frontmatter roots
8. Run validation rules §15 (1-13)
9. Emit DTCG JSON (§16.1)
```

Parser MAY be implemented in any language. Reference parser planned in Node (per PLAN.md §9 P0.3 lock — extraction script language).

### 18.3 Open questions

Deferred to subsequent spec versions:

1. **Composition tokens (combo classes as first-class)** — v2's `compositions/` layer in DTCG terms; PRISM.md v1.0 punts to "use rationale prose for now." A future `composition` block type may emerge.
2. **Structure tokens (block hierarchy grammar)** — v2's `structure/` layer; same punt.
3. **Localization** — `$description` is single-language in v1.0. DTCG allows object-form descriptions for i18n; PRISM.md will follow when consumers request.
4. **Multi-project monorepos** — `<project>.PRISM.md` naming supports it, but cross-file token references aren't yet specified. Probable v1.1 addition.
5. **Schema validation file** — a JSON Schema for PRISM.md frontmatter could land in `schema/prism-md.schema.json`. Deferred until v2's `schema/` layer becomes active.

### 18.4 Spec stability commitment

Format `prism-spec/1.0` is committed to backward compatibility through `prism-spec/1.x`. Authoring a PRISM.md against this spec will continue to parse correctly under all 1.x parsers. The next major version (`prism-spec/2.0`), if needed, will be specified in a separate doc that explicitly enumerates breaking changes.

---

## 19. Asset Blocks

Assets are first-class Prism extensions (per §12) that live alongside tokens in PRISM.md body. They represent **files referenced by usage rules** — logos, icons, photography, font files, audio, video — distinct from tokens which represent **visual property values**. The block grammar parallels §8 (Token-Block Grammar) but the semantics + emit behavior differ.

Assets are introduced in `prism-spec/1.0` as an additive feature. PRISM.md files that don't declare any assets remain valid; older parsers MAY warn on `## Assets` body sections but MUST NOT error.

### 19.1 Syntax

Asset blocks use ` ```asset ` fence tags (parallel to ` ```token `):

```asset
logo.primary:
  $type: image
  $path: assets/logos/df-wordmark.svg
  $description: Primary Designframe wordmark for use on light backgrounds.
  $applied-guidance: |
    Minimum size 24px height for legibility. Maintain clear-space equal to
    space.gutter.base on all sides. Don't recolor — the gradient IS the brand.
  $clear-space:
    $ref: "{space.gutter.base}"
  $min-size:
    $ref: "{size.element.base}"
  $variants:
    on-dark: assets/logos/df-wordmark-on-dark.svg
    mark: assets/logos/df-mark.svg
```

The first key is the **asset path** (DTCG-style dotted-path, e.g. `logo.primary`, `icon.search`). Asset paths live in a separate namespace from token paths — no collision possible.

### 19.2 Field schema

| Field | Required? | Type | Purpose |
|---|---|---|---|
| `$type` | required | string | One of: `image`, `icon`, `font-file`, `audio`, `video`. |
| `$path` | required | string | Project-relative path to the asset file (resolved from PRISM.md's directory). |
| `$description` | recommended | string ≤ 200 chars | One-line description of the asset. |
| `$applied-guidance` | optional | multi-line string | Usage rationale, do/don't, accessibility considerations. |
| `$clear-space` | optional | `{ $ref: "{token.path}" }` | Minimum clear-space, referencing a space token. |
| `$min-size` | optional | `{ $ref: "{token.path}" }` | Minimum display size, referencing a size token. |
| `$variants` | optional | object | Named alternate files (e.g., `on-dark`, `mark`, `animated`). Each variant value is a path string. |
| `$ai-hint` | optional | multi-line string | AI-targeting hint (rendering, alt-text, placement). |
| `$df-source` | not applicable | — | Assets don't map to CSS vars; field omitted from asset blocks. |

### 19.3 Block placement

Asset blocks live under the body's `## Assets` H2 section (per §7 Section Taxonomy — adds `Assets` as canonical section between `Components` and `Modes`). Group related assets under H3 sub-sections by family:

```markdown
## Assets

### Logo

```asset
logo.primary:
  ...
```

### Icon Set

```asset
icon.search:
  ...
```
```

### 19.4 Path semantics

`$path` and `$variants[*]` values are **project-relative** (resolved from PRISM.md's directory). The Prism Kit mirrors the project's asset structure: e.g., for the Designframe Prism Kit, `examples/designframe/assets/logos/df-wordmark.svg` ships at the kit root.

External URLs in `$path` are **not supported in `prism-spec/1.0`** — deferred to a future minor version (when consumer demand for CDN-hosted assets emerges). Until then, all assets ship inside the kit.

### 19.5 Cross-references between tokens and assets

Tokens MAY `$ref` an asset path (e.g., a `background-image` token could reference `{logo.primary}`). This composition pattern is rare but supported — emitters that render both tokens and assets together (showcase.html, design-system.html) MUST resolve cross-namespace references correctly.

### 19.6 Validation rules (PRISM.md Layer 1 additions)

Three asset-specific validation rules extend §15:

| # | Rule | Severity | Check |
|---|---|---|---|
| 15 | `asset-path-exists` | error | Every `$path` resolves to an actual file in the bundle |
| 16 | `asset-clear-space-token-resolvable` | warning | When `$clear-space` is set, its `$ref` target exists in the token tree |
| 17 | `asset-type-valid` | error | `$type` is in the documented vocabulary (`image`/`icon`/`font-file`/`audio`/`video`) |

These bring the total PRISM.md Layer-1 rule count from 14 to 17. Implementation of rules 15-17 lands alongside the asset feature; existing rules 1-14 remain unchanged.

### 19.7 Emit semantics

Assets emit differently per consumer target:

| Target | Asset emission |
|---|---|
| `tokens.json` (DTCG bundle) | Asset metadata emits to a sibling `assets.json` file (DTCG has no native asset type; this is a Prism extension) |
| `tokens.css` | Asset paths emit as `--asset-{path}: url(...)` CSS custom properties (URL-encoded path) |
| `df-input.css` | Same as tokens.css (paths-as-CSS-vars) |
| `tailwind-preset.js` | Skipped — Tailwind preset is value-only, no asset surface |
| `figma-tokens-studio.json` | Skipped — Tokens Studio reads tokens, not assets |
| `design-md.md` | Asset paths emit in `## Assets` section as markdown image references with usage tables |
| `design-system.html` | Full asset gallery — rendered images, usage rules, variants, downloads |
| `showcase.html` | Skipped (token-only regression fixture) |

---

## Document history

| Date | Session | Change |
|---|---|---|
| 2026-05-16 | S2 | Draft 1 created. 5 design decisions locked (canonical authoring surface, byte-similar round-trip, DESIGN.md strict superset, hybrid syntax, PRISM.md + project variant naming). |
| 2026-05-17 | S54+1 (Checkpoint #4) | §15 Rule 4 `token-path-unique` clarified to per-(path, $mode) bucket semantics. Original wording was ambiguous between narrow (path-only) and broad (per-mode) readings; broad reading is the implementation contract. T5 calibration. |
| 2026-05-17 | S54+2 (Asset feature) | §19 Asset Blocks added — new section type for brand asset declarations (logo / mark / favicon) with `$path`, `$applied-guidance`, `$clear-space`/`$min-size` $ref to tokens, and `$variants`. §7 Section Taxonomy updated with `## Assets` at canonical position #8 (between Components and Modes); subsequent rows renumbered. §15 Validation Rules extended with rules 15-17 (asset-path-exists / asset-clear-space-token-resolvable / asset-type-valid). Format stays `prism-spec/1.0` — purely additive. |
