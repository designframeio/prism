# Designframe Prism Kit

> **For step-by-step usage,** read [`QUICKSTART.md`](./QUICKSTART.md). This file is the conceptual reference — what a Prism Kit is, what's in this one, and where to read the spec.

A **Prism Kit** is a complete brand-specific design-system bundle — a single zip-portable folder containing the canonical source (`PRISM.md`), a visual reference (`design-system.html`), brand assets (`assets/`), and machine-consumable exports in 7 target formats (`exports/`). One folder; everything downstream tools need.

This particular kit is **Designframe v0.99** expressed as a Prism Kit — the canonical example shipped alongside the Prism format/tool itself.

## Kit contents

```
examples/designframe/
├── README.md                ← this file (conceptual intro)
├── QUICKSTART.md            ← how to use this kit (action-oriented)
├── PRISM.md                 ← canonical source: 12 token groups + 4 modes + 3 assets + prose
├── design-system.html       ← marquee visual: 11-section design-system doc + Atlas
├── assets/                  ← brand SVGs (5 logo variants)
│   └── logos/
└── exports/                 ← 7 target-format consumables
    ├── tokens.json + tokens.{dark,alt,invert,key}.json   ← DTCG JSON + 4 mode overlays
    ├── tailwind-preset.js                                  ← Tailwind v3+ preset
    ├── tokens.css                                          ← CSS custom properties
    ├── df-input.css                                        ← Designframe runtime input
    ├── figma-tokens-studio.json                            ← Figma Tokens Studio plugin format
    ├── design-md.md                                        ← Google DESIGN.md (validated 0/0/0)
    ├── manifest.json                                       ← bundle metadata + brandOwnedRoots contract
    └── rationale.md                                        ← prose (paired with tokens.json for AI tooling)
```

## What this kit IS

- **A complete reference implementation of Designframe at v0.99.** Every brand color, theme context, typography step, spacing context, radius, shadow, and brand asset Designframe defines is in `PRISM.md`. The exports are derived by `prism parse` + `prism emit`.
- **Zip-portable.** Every internal reference resolves from the kit root. Zip this folder, extract anywhere, open `design-system.html` — everything works without a build step or filesystem context above the kit.
- **A working example of the Prism format.** Other brands wanting to ship a similar artifact can use this as a template, swap `brandOwnedRoots` for their own values, and emit their own kit.

## What this kit is NOT

- **Not the full Designframe framework.** The atomic tier (tokens) is in scope; combo classes, block hierarchy rules, adaptive layout math, and the Tailwind utility surface are part of [`designframe-ui`](https://github.com/mikeprasad/designframe-ui), not this kit. v2 of the Prism format may bring those layers in scope (see [`../../spec/v2-reserved/README.md`](../../spec/v2-reserved/README.md)).
- **Not a brand-asset license.** The wordmark, mark, key gradient, and "Designframe" name remain © Mike Prasad. Apache 2.0 covers the code and token definitions; commercial reuse of the brand identity outside this design-system reference falls outside the license.

## Status

**v1.0-export functionally complete** as of 2026-05-18. All emitters operational, both lint layers clean (Layer 1: 0/0/1; Layer 2: 0/0/0; Google DESIGN.md lint: 0/0/0). Build is idempotent — `npm run build` twice produces zero diff.

## License

Apache 2.0 — see [`../../LICENSE`](../../LICENSE).

## References

| Document | Purpose |
|---|---|
| [`QUICKSTART.md`](./QUICKSTART.md) | How to use this kit (read first if consuming) |
| [`../../spec/prism-md-format.md`](../../spec/prism-md-format.md) | The Prism format specification (`prism-spec/1.0`) |
| [`../../spec/designer-guide.md`](../../spec/designer-guide.md) | How to author a PRISM.md by hand |
| [`../../spec/lint-rules.md`](../../spec/lint-rules.md) | Layer 2 (bundle) lint rule definitions |
| [`../../spec/description-contract.md`](../../spec/description-contract.md) | `$description` authoring style + exemplars |
| [`../../PRODUCT.md`](../../PRODUCT.md) | Prism product identity + execution roadmap |
| [`../../PLAN.md`](../../PLAN.md) | Implementation plan + decision log + session log |
| [`../../README.md`](../../README.md) | The repo-level intro — Prism + this kit as canonical example |

## Conventions

- **Pivot format:** W3C DTCG JSON, two tiers (primitives + semantic)
- **Naming:** DF-native first (`space.shoulder`, `color.theme.alt.fg`); generic aliases (`space.md`) emitted as `$ref` references
- **Mode overlays:** base `tokens.json` + named partials (`tokens.dark.json`, `tokens.alt.json`, `tokens.invert.json`, `tokens.key.json`)
- **Brand contract:** every token marked `$brand-owned` (replaceable on fork) or `$system-owned` (stable). Drives `prism fork` ergonomics.
- **fg/bg pairing:** every `color.*.bg` MUST have a paired `color.*.fg` (enforced by lint Rule 8)
