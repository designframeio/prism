# v2 RESERVED — Future Prism Layers

Reserved scope for Prism v2. Currently empty placeholders; v1 ships the **tokens layer** only. v2 adds four additional layers when real-world usage signals justify each. See `../../PLAN.md` §6 for the full v2 spec.

> **Do not place v1 artifacts in these layers.** Bundle consumers detect populated layers via `manifest.json`'s `prismVersion` field; mixing scopes corrupts that signal.

This file consolidates four previous `prism-v1/{compositions,structure,exemplars,schema}/RESERVED.md` files (one per reserved layer). Consolidated 2026-05-18 (S52 Prism Kit reorganization, PLAN.md §13).

## Compositions layer

DF combo classes expressed as first-class declarations with token references — composition primitives instead of class-naming conventions.

## Structure layer

DF grammar rules expressed as machine-readable JSON:

- **Hierarchy:** MAIN > SECTION > BLOCKS > Content
- **Semantics:** allowed elements per role
- **Adaptivity:** breakpoint + stack behavior

Formalizes `df-rules.md` (currently 228KB of prose ruleset) into artifacts consumers can validate against. Strong co-evolution: v2's structure layer and df-rules.md advance together.

## Exemplars layer

Canonical HTML fixtures drawn from:

- 178 cs samples at `/cs/commonspace-designframe-ui/cs-working/src/pages/samples/` (read-only from df-prism)
- 3 hand-authored reference pages at `/df/df-ui/df-working/old-docs/`
- 48 docs pages at `/df/df-ui/df-working/docs/`

Each exemplar is a minimal self-contained snippet tagged with which compositions and structure rules it demonstrates. Purposes: regression fixtures, AI training data, ground truth for round-trip tests.

Tag taxonomy under consideration (from PLAN.md §6 + df Session 23 finding): `page:content` (clean sequential headings) vs `page:reference` (visual-categorization heading levels).

## Schema layer

JSON Schema files formalizing the contract for each populated layer:

- `tokens.schema.json` — DTCG token tree shape (DF-native naming, fg/bg pairing, $brand/$system-owned markers, $ref alias contract)
- `compositions.schema.json` — combo-class declarations
- `structure.schema.json` — DF grammar rules
- `exemplars.schema.json` — exemplar manifest format

v1 ships **lint rules** (`../lint-rules.md`) but not formal JSON Schemas — schemas are a v2 scope-up. Lint operates declaratively in v1; v2 adds schema validation as a second layer.
