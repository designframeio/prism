# Prism v1-Export — Lint Rules Specification (Bundle Layer)

> **Last updated:** 2026-05-16 (S2). S1 draft + S2 PRISM.md cross-link.
>
> Spec only — no implementation in v1-Export skeleton. Implementation lands in a subsequent session as a `prism lint` subcommand (Node). Per PLAN.md §4 "Validation harness from day one."
>
> **Two-tier lint architecture** (added S2):
> - **Layer 1 — PRISM.md rules (14 rules)** — run against the input PRISM.md file. Covers format identity, YAML validity, token-block syntax, $ref resolution in MD, brand/system-owned exclusivity, fg/bg pairing in MD, section order, mode declarations, DTCG type validity, DESIGN.md compatibility. Defined in [`../../spec/prism-md-format.md`](../../spec/prism-md-format.md) §15.
> - **Layer 2 — Bundle rules (7 rules, this file)** — run against the emitted DTCG bundle. Covers broken-ref + missing-primary + contrast-ratio + orphaned-tokens + missing-section + section-order (at the rationale.md level) + manifest-lossiness-complete.
>
> Layer 1 fires first (against PRISM.md); if it passes, the file is parsed to DTCG and Layer 2 fires (against the bundle). Failures at either layer block emission downstream. Both layers share the same `prism lint` CLI but report results as distinct rule sets.

## Why this exists

> A design-system format that can't be validated is a design-system format that drifts. (PLAN.md §4)

Google's DESIGN.md CLI shipped 7 lint rules out of the gate; Prism was originally going to defer validation to v2. Reversal: v1-Export ships its own 7 rules so token bundles can be CI-validated from day one.

## Rule catalog

| # | Rule | Severity | Applies to | Status |
|---|------|----------|------------|--------|
| 1 | `broken-ref` | error | `tokens/*.json`, `rationale.md` | spec |
| 2 | `missing-primary` | warning | `tokens/*.json` | spec |
| 3 | `contrast-ratio` | warning | semantic tier color pairs | spec |
| 4 | `orphaned-tokens` | warning | `tokens/*.json` | spec |
| 5 | `missing-section` | info | `rationale.md` | spec |
| 6 | `section-order` | warning | `rationale.md` | spec |
| 7 | `manifest-lossiness-complete` | error | `manifest.json` | spec |

`lintRulesVersion` in `manifest.json` is `1.0.0`. Bumps follow semver against rule set changes:
- patch: rule wording / message text change
- minor: new rule added or default severity loosened
- major: rule removed, default severity tightened, or check semantics change

---

## Rule 1 — `broken-ref` *(error)*

**Applies to:** every `$ref` value in any tokens file (`tokens/tokens.json`, `tokens/tokens.dark.json`, etc.) and any `{token.path}` reference inside a `$description` field.

**Check:** resolve every `$ref` value as a DTCG path. The reference must target an existing token whose tree branch chain terminates at a leaf token with a non-null `$value`. Across-overlay refs are allowed (a base ref can target an overlay-only token); same-file refs that don't resolve are errors.

**Failure cases:**
- `$ref: "{color.brnd.key}"` (typo) → error
- `$ref: "{color.brand}"` (resolves to a group, not a leaf) → error
- `$ref: "{color.brand.future-key}"` (doesn't exist) → error
- Description text `"see {space.gutter.bse}"` (typo in `{}`-form path) → error

**Rationale:** broken `$ref` is silently fatal in some DTCG consumers (Tokens Studio renders nothing; Tailwind preset emits the raw path string). Catching at lint time avoids consumer-side mystery failures.

---

## Rule 2 — `missing-primary` *(warning)*

**Applies to:** `tokens/tokens.json` color tree.

**Check:** at least one of these tokens exists with a defined leaf value:
- `color.brand.primary` (DF-native)
- `color.primary` (generic alias)
- `color._aliases.primary` (`$ref` form)

If none exist, fire warning: "No canonical primary brand color found. Consumers expecting a `primary` token will fall back to alphabetical first color."

**Rationale:** every consumer ecosystem (Tailwind, MD3 / DESIGN.md, Tokens Studio templates, claude.ai/design code generator) expects a `primary` color as the default brand accent. Missing it doesn't break Prism — but it breaks downstream tooling silently.

---

## Rule 3 — `contrast-ratio` *(warning)*

**Applies to:** any token at the **semantic tier** with name pattern `color.*.bg` AND its paired `color.*.fg` token. Also applies to declared `_pairings` (e.g., theme overlays where `bg-of-mode` pairs with `fg-of-mode`).

**Check:**
1. For each `color.*.bg`, resolve its paired `color.*.fg` (sibling with `.fg` suffix replacing `.bg`).
2. Compute WCAG 2.2 contrast ratio using the standard relative-luminance formula.
3. If the bg/fg pair is intended for body text (default — unless `$contrastTarget: "large"` set on either): require ≥ 4.5:1 (WCAG AA body).
4. If `$contrastTarget: "large"` is declared on either token: require ≥ 3:1 (WCAG AA large-text).
5. If `$contrastTarget: "decorative"` is declared: skip check.

**Failure cases:**
- `color.theme.default.bg = #ffffff` paired with `color.theme.default.fg = #cccccc` → contrast 1.61:1 → warning
- `color.alert.notify.bg = #3883ff` paired with `color.alert.notify.fg = #ffffff` → 3.4:1 → passes large-text (3:1) but fails body-text default (4.5:1) → warning unless `$contrastTarget: "large"` set

**Rationale:** DF already enforces fg/bg pairing implicitly via theme variants (`color.theme.alt.fg` / `color.theme.alt.bg`). Lint enforces *contrast quality* alongside *pairing existence*. Mike has flagged accessibility (a11y testing) as Mike-owned per `df-ui/CLAUDE.md` Open threads; this lint partially front-runs the manual a11y pass for color pairs.

**v1-Export caveat:** semantic tier is not yet authored (S1 ships primitives only). Rule 3 is dormant on the S1 bundle but ready to fire as soon as semantic-tier tokens land.

---

## Rule 4 — `orphaned-tokens` *(warning)*

**Applies to:** `tokens/tokens.json` and overlays.

**Check:** a token is *orphaned* when:
1. It has a `$value` (alias-only `$ref` leaves are inherently referenced and excluded), AND
2. Its dotted-path is not the target of any other token's `$ref`, AND
3. It is not declared as a "well-known root" (per the allowlist below).

Well-known roots (skipped from orphan check). v1-Export's primitives surface is mostly consumer-direct (no component tier yet exists to reference primitives internally), so the allowlist is intentionally broad — Rule 4 becomes meaningfully active once a component tier or composition layer lands that references primitives. Until then it acts as a structural-validity check for the few roots NOT listed here.

| Root pattern | Why exempt |
|---|---|
| `color.brand.*` | Brand identity surface; replaced when forking |
| `color.brand-alt.*` | Alt brand identity surface |
| `color.alert.*` (all children) | Alert colors are direct consumer surfaces |
| `color.theme.*-transparent.bg` | Transparent literal bgs (`$value: "transparent"`); $contrastTarget=decorative siblings, intentionally not referenced |
| `color.theme.{header,footer}.{bg,fg,nav-link}` | Landmark semantic-tier consumer surfaces (`--qs-color-header-*` / `--qs-color-footer-*`); intentionally consumer-direct without internal references |
| `space.*` (all spacing contexts + breakpoint variants) | Adaptive spacing is consumer-direct (`p-gutter xl:p-gutter-xl`) |
| `size.*` | Element dimensional sizing (button heights, hairlines) is consumer-direct |
| `typography.*` (full subtree: family + scale + all sizes/lhs) | Type scale entries are consumed directly by `text-*` utilities |
| `radius.*` (full subtree: brand + full + chip + article) | Border radii are consumed directly |
| `shadow.*` (full subtree: composite shadows + alpha sub-tokens) | Shadow primitives + alpha sub-tokens are consumer-direct |
| `blur.*` | Blur radii are consumer-direct |
| `shader.*` | Shader alphas are exposed for opacity-modulated bg composition |
| `border.*` | Border widths are consumer-direct |
| `transition.*` | Transition primitives (duration/timing/property) are consumer-direct |
| `screen.*` | Breakpoints are hard-coded in @media queries + consumed by Tailwind preset |
| `layout.cols.*` and `layout.col-width.*` | Grid system primitives are consumer-direct |

For everything else, if no `$ref` chain leads to it, fire warning: "Token defined but never referenced. Candidate for: (a) removal, (b) adoption into a semantic-tier token, (c) emitter-side direct usage."

**Rationale:** unused tokens accumulate cost (cognitive load + manifest noise). They're often a signal that a token was added speculatively for a use that never materialized. Warning, not error — some tokens are deliberately exposed for consumer composition without being internally referenced.

**v1-Export caveat:** the rule is largely dormant in v1-Export — the bundle's primitives surface is intentionally consumer-direct without internal references. The rule activates once a component tier or v2 `compositions/` layer lands that references primitives internally; then primitives no composition consumes become real orphans. The allowlist above defines what's *currently* expected; when component-tier tokens are added in v1.1+, the allowlist will narrow to expose more primitives to the orphan check. Tracked as a v1.x calibration concern.

---

## Rule 5 — `missing-section` *(info)*

**Applies to:** `rationale.md`.

**Check:** for each populated token category, the corresponding `rationale.md` section must exist (heading present, content optional):

| Token category | Required section |
|---|---|
| `color.*` | `## Colors` |
| `typography.*` | `## Typography` |
| `space.*`, `size.*`, `layout.*` | `## Layout` (alias: `## Layout & Spacing`) |
| `shadow.*`, `blur.*` | `## Elevation & Depth` (alias: `## Elevation`) |
| `radius.*` | `## Shapes` |
| anything component-tier | `## Components` |

If a section is missing (heading absent) when its category is populated in `tokens.json`, fire info-level message. Info, not warning — a consumer who *only* needs tokens.json can suppress rationale.md entirely.

**Overview** and **Do's and Don'ts** sections are recommended but not checked by this rule — they're brand-voice-led, not category-driven.

---

## Rule 6 — `section-order` *(warning)*

**Applies to:** `rationale.md`.

**Check:** the `## ` (H2) headings appearing in the file must appear in canonical order:

1. Overview *(alias: Brand & Style)*
2. Colors
3. Typography
4. Layout *(alias: Layout & Spacing)*
5. Elevation & Depth *(alias: Elevation)*
6. Shapes
7. Components
8. Do's and Don'ts

Sections may be omitted (no warning fires for absence — that's Rule 5's job). Sections present but **out of order** fire warning: "Section `X` appears before `Y`, but `Y` is canonical-order-earlier."

Aliases are accepted (per `manifest.json` `rationale.aliases`).

**Rationale:** consumer tools (DESIGN.md emitter, claude.ai/design upload UI) rely on stable section order to extract relevant prose. Out-of-order sections silently corrupt that extraction.

---

## Rule 7 — `manifest-lossiness-complete` *(error)*

**Applies to:** `manifest.json`.

**Check:** the `lossiness[]` array must:
1. Exist as an array property (not absent, not null, not object).
2. Contain at least one entry for each of these declared-loss categories per v1-Export contract:
   - `combo-classes` (bundle-scope)
   - `block-hierarchy` (bundle-scope)
   - `adaptive-stacks` (bundle-scope)
   - `semantic-html-rules` (bundle-scope)
3. If any emitter is listed in `targets[]` that has emitter-specific lossiness (e.g. `design-md` → flattens modes, applies MD3 name mapping), the corresponding emitter-scope entries must also be present (`design-md-single-mode`, `design-md-md3-name-mapping`).

**Failure cases:**
- `lossiness` field omitted → error
- `targets[]` includes `design-md` but no `design-md-*` entries in `lossiness[]` → error
- `lossiness` is an empty array → error

**Rationale:** the lossiness field is the bundle's honesty contract (PLAN.md §5 "Declared lossiness"). Silently omitting it means consumers don't know what they're getting; lint enforcement makes the contract non-optional. Error severity matches: a Prism bundle that doesn't declare its losses is broken by definition, not by judgment.

---

## CLI shape (when implemented)

```
prism lint [bundle-dir]
  --strict          fail on warning (default: fail only on error)
  --skip <rule>     skip named rule (repeatable)
  --format <fmt>    json | text | github-actions (default: text)
```

Output to stdout, structured object to `lint/results.json` (regenerated on every `prism emit` per PLAN.md §5 directory layout).

Exit codes: 0 = clean / warnings; 1 = errors present; 2 = bundle structure invalid (missing manifest).

## Future rules (v1.x candidates)

Deferred from v1-Export, considered for v1.1:

- **`description-too-thin`** (warning) — `$description` < 40 chars on color/dimension tokens.
- **`description-missing-applied-guidance`** (info) — `$description` lacks "use" / "not" / "avoid" / boundary keywords.
- **`mode-overlay-completeness`** (warning) — overlay file (e.g. `tokens.dark.json`) doesn't cover every base token that has a brand-owned bg.
- **`brand-vs-system-conflict`** (error) — a token is marked both `$brand-owned: true` and `$system-owned: true`.

These don't ship in v1-Export to avoid blocking the first release on judgment-call rules. Add them in v1.1 once real bundles have surfaced which mistakes are worth catching automatically.

## Future rules (v2 candidates)

- **`exemplar-validates-against-structure`** — meta-test, requires structure layer.
- **`round-trip-byte-equivalent`** — Prism → df-input.css → Prism stable, requires bidirectional extractor.
- **`composition-references-resolvable`** — combo-class declarations reference real tokens, requires compositions layer.
