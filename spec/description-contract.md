# `$description` Contract — Prism v1-Export

> **Last updated:** 2026-05-16 (S2). S1 draft + S2 PRISM.md cross-link.
>
> Spec only — no enforcement yet.
>
> **Where $description is authored:** in PRISM.md, the `$description` field is a YAML string inside a ```token``` code-fence block (for body tokens) or inside a frontmatter token entry (for atomic primitives). This contract governs the *content* of the description regardless of where it lives — the same style + structure rules apply in frontmatter YAML and in body ```token``` fences. See [`../../spec/prism-md-format.md`](../../spec/prism-md-format.md) §8 for the token-block grammar.

## Why this exists

DTCG's `$description` field is optional and unconstrained. Prism formalizes it as a **first-class layer of applied guidance** so AI consumers (claude.ai/design, future MCP App design tools) get more than a one-line blurb. Per PLAN.md §4 "Prose rationale as a first-class layer":

> An AI consumer needs *why this token exists, when to use it, and what not to do with it* — not just a label.

`rationale.md` carries cross-cutting brand/voice; `$description` carries per-token applied guidance.

## Required content (per token)

Each populated `$description` must include the following, in order, separated by sentence boundaries (no formal delimiter — keeps the field human-readable + tool-compatible):

1. **What it is** — one short clause. Prefer plain noun phrase; assume the consumer knows DTCG basics.
2. **DF-native name** — parenthetical mapping back to df-input.css. Format: `DF-native: <name>. Source: <css-var>.`
3. **When to use** — concrete usage signal. Naming a representative consumer (component, layout context) is preferred over abstract criteria.
4. **What not to do** — at least one anti-pattern OR a paired-token boundary (e.g., "don't override per-card; card density is a system decision"). Skip only when the token is so atomic that misuse isn't plausible (e.g., `screen.sm`).

Items 1+2 are **mandatory**; items 3+4 are **strongly recommended** but waivable for atomic system tokens (breakpoints, alpha sub-tokens) where applied guidance is implicit.

## Style rules

- **Tense:** present, neutral. "Use for…" not "you should use this for…".
- **Length:** ≤ 280 chars total. If you need more, escalate to `rationale.md`.
- **No marketing tone.** This is operational metadata.
- **No external URLs.** Reference cross-tokens by DTCG path (`{color.brand.key}`) or DF anchor (`§ ANCHOR Gutters`).
- **Avoid restating $type** — DTCG already has `$type: "color"`; don't add "This is a color token."

## Lint integration

Two of the 7 v1-Export lint rules touch `$description`:

- **`broken-ref`** (error) — `$ref` resolution failures, indirectly catches description references when descriptions use `{path}` syntax with typos.
- **`missing-primary`** (warning) — fires when no `color.primary` (DF-native) or canonical equivalent exists; description content is one signal but not the only one.

Future v1.x may add:

- **`description-too-thin`** (warning) — fires when `$description` < 40 chars and `$type` is color/dimension. Deferred for v1-Export to avoid blocking the first ship.
- **`description-missing-applied-guidance`** (info) — fires when `$description` lacks "use" / "not" / "avoid" / boundary-marker keywords. Heuristic; deferred.

## Exemplars

Five exemplars covering the major token categories.

### Exemplar 1 — Brand color (high-stakes, replaceable)

```json
{
  "color.brand.key": {
    "$type": "color",
    "$value": "#fb03b9",
    "$brand-owned": true,
    "$description": "DF brand key color (gradient start). DF-native: color.key. Source: --qs-color-key. Use as the brand-identity accent (button-gradient start, key-tinted backgrounds, link hover via .text-key). When key is a gradient, color.brand.key-end is the paired end-stop; don't apply solo as solid-fill brand color unless the design specifies."
  }
}
```

What it gets right: WIA (4 sentences) covers what/native-mapping/when/not. Names a concrete consumer (`.text-key`, gradient buttons). Calls out the paired-token boundary.

### Exemplar 2 — Spacing context (semantic, system-owned)

```json
{
  "space.gutter.base": {
    "$type": "dimension",
    "$value": "24px",
    "$system-owned": true,
    "$description": "DF standard gutter spacing — mobile-first. DF-native: space.gutter. Source: --spacing-gutter-base. Use for section-level rhythm (between blocks in section-grid, between sections via m-section). Do not override per-component; gutter is a system-level decision and per-component overrides break the adaptive grid math."
  }
}
```

What it gets right: explicitly names the consumer pattern (`section-grid`, `m-section`). The "don't" is paired with the *reason* (breaks adaptive grid math) — informs judgment for edge cases, doesn't just prohibit.

### Exemplar 3 — Card padding (component-adjacent, system-owned)

```json
{
  "space.article.base": {
    "$type": "dimension",
    "$value": "16px",
    "$system-owned": true,
    "$description": "Card / article padding — mobile-first. DF-native: space.article. Source: --pad-article-base. Use for .card / article.block-*.card internal padding via p-article. Don't override per-card; card density is a system-level decision and per-card overrides defeat the purpose of having a card primitive."
  }
}
```

Mirror of Exemplar 2 — same boundary phrasing pattern reused. Convention: "Don't override per-X" is the DF shibboleth for "this token is a system decision" — applies to any context-anchored spacing.

### Exemplar 4 — Typography size (member of a scale)

```json
{
  "typography.scale.p4.size": {
    "$type": "dimension",
    "$value": "14px",
    "$system-owned": true,
    "$description": "Body text P4 size — supporting / secondary copy. DF-native: text.p4-size. Source: --text-p4-size. Use for descriptive sub-copy (hgroup descriptions, card body, list-item secondary text). Paired with typography.scale.p4.lh at 20px (1.43 ratio); don't change size without recomputing lh — DF's 24px line-height anchor breaks at off-scale sizes."
  }
}
```

What it gets right: explicit paired-token mention (`p4.lh`), explicit ratio (1.43), explicit anchor (24px). Future emitter (DESIGN.md, Tailwind preset) can parse these as machine-readable hints if needed.

### Exemplar 5 — Atomic system value (waivable applied-guidance)

```json
{
  "screen.sm": {
    "$type": "dimension",
    "$value": "640px",
    "$system-owned": true,
    "$description": "Small breakpoint. DF-native: --screen-sm. Hardcoded in both df-input.css @media and df-preset.js — CSS custom properties cannot be used in @media queries."
  }
}
```

Why no "when to use" / "what not to do": atomic breakpoint with implicit usage and no plausible misuse. The description does carry an important *implementation* note (the @media-hardcode constraint) — this is a hidden constraint, the legitimate place for a comment per the working-rules.md "non-obvious WHY" criterion.

## Authoring workflow

1. Extract token from df-input.css.
2. Write `$description` per the contract above.
3. Run prism lint (when implemented) — flags broken `$ref`s in descriptions, missing fg/bg pairs, etc.
4. If you reach for >280 chars, move the overflow content to `rationale.md` instead and leave `$description` minimal.

## Open questions (deferred)

- **Localization** — `$description` is English-only in v1-Export. DTCG spec accommodates object-form descriptions for i18n; defer until a consumer asks.
- **Markdown in `$description`** — currently disallowed. claude.ai/design and Tokens Studio both render plain text; markdown would only matter for DESIGN.md emitter, which uses `rationale.md` for prose anyway.
- **Rationale-section back-links** — could add a `$rationaleAnchor: "Colors"` field on tokens that have prose rationale, but it's emitter-specific and easy to add later. Not in v1-Export.
