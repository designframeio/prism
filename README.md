# Designframe Prism

A portable design-definition format. One canonical `PRISM.md` source enters the prism, multiple target-specific representations exit — DTCG JSON, Tailwind preset, CSS custom properties, Figma Tokens Studio JSON, Google DESIGN.md, and a self-contained design-system document.

> **Status:** Spec + canonical example Kit are public (Apache 2.0). **Tooling (parser, emitters, CLI) is forthcoming** in a later release.

This repo ships two things side-by-side:

1. **Prism — the format** ([`spec/`](./spec/)) — the `prism-spec/1.0` specification. Read it; author your own `PRISM.md` by hand.
2. **The Designframe Prism Kit** ([`examples/designframe/`](./examples/designframe/)) — Designframe v0.99 expressed as a complete Prism Kit (source + visual reference + brand assets + 7 export formats). Doubles as canonical reference and a working example.

A **Prism Kit** is a self-contained folder: the source, the visual reference, the assets, and the exports — everything a recipient needs to build from across any tool/export format.

## Three onramps

### 1. Use the Designframe Prism Kit directly

The kit is committed to this repo — no build needed. Pick the export that matches your stack:

| Your stack | File |
|---|---|
| Tailwind project | `examples/designframe/exports/tailwind-preset.js` |
| Any web project | `examples/designframe/exports/tokens.css` |
| Designframe runtime consumer | `examples/designframe/exports/df-input.css` |
| Figma + Tokens Studio plugin | `examples/designframe/exports/figma-tokens-studio.json` |
| AI design tooling (claude.ai/design) | `examples/designframe/exports/tokens.json` + `examples/designframe/exports/rationale.md` |
| Standalone design-system doc | `examples/designframe/design-system.html` (single file, open in browser) |
| Raw DTCG JSON | `examples/designframe/exports/tokens.json` (+ 4 mode overlays) |

See [`examples/designframe/QUICKSTART.md`](./examples/designframe/QUICKSTART.md) for per-format usage.

### 2. Author your own Prism Kit by hand

Read the spec ([`spec/prism-md-format.md`](./spec/prism-md-format.md), [`spec/designer-guide.md`](./spec/designer-guide.md)). Copy [`templates/starter.PRISM.md`](./templates/starter.PRISM.md) as a brand-neutral seed. Edit the placeholders with your brand's colors, fonts, and identity statement.

You won't get the tooling yet — Layer-1 lint, parsing, automatic emit are forthcoming. But the spec is self-contained: a `PRISM.md` you hand-author against `prism-spec/1.0` is forward-compatible with the eventual tooling.

### 3. Build your own parser / emitter

The spec is complete enough that you can implement your own `PRISM.md → DTCG bundle` parser. `spec/prism-md-format.md` covers grammar, type mapping, mode overlays, lint rules, and round-trip semantics. `spec/lint-rules.md` defines the bundle-validation contract.

## What's coming

`@designframeio/prism-kit` (the kit content republished as an npm package) lands within 1-2 weeks of this release for ergonomic `npm install` consumption.

`@designframeio/prism` (the tool — parser, emitters, lint engine, CLIs) lands later, gated on usage signals from the spec+kit release.

## License

[Apache 2.0](./LICENSE) covers the spec, kit content, and templates. The Designframe brand identity (wordmark, mark, key gradient, "Designframe" name) is © Mike Prasad — Apache 2.0 covers the code/data, not commercial brand-asset reuse outside this design-system context.

## Format spec

The `prism-spec/1.0` format is documented in [`spec/prism-md-format.md`](./spec/prism-md-format.md). It's a strict superset of Google DESIGN.md and a portable wrapper over W3C DTCG JSON for design tokens.

## Repo

- **Homepage:** [designframe.io](https://designframe.io)
- **This repo (spec + kit):** [github.com/designframeio/prism](https://github.com/designframeio/prism)
- **Tool release (forthcoming):** `@designframeio/prism` on npm + this same repo at `packages/prism/`
