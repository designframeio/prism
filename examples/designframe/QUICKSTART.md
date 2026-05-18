# Designframe Prism Kit — Quickstart

You have a complete **Prism Kit** for the Designframe design system — every token primitive, every mode overlay, every brand asset, and every export format you might need, in one self-contained folder. Open it. Use it. Fork it.

If you only do one thing first: **open `design-system.html` in a browser**.

## What's in this kit

| File | What it is | When you'd use it |
|---|---|---|
| **`PRISM.md`** | The canonical source — every token, every mode, every brand asset declaration, with prose rationale | Editing the design system; forking for your own brand; reading the "why" |
| **`design-system.html`** | The visual reference — 11-section design-system doc + Atlas (every token rendered) | First read, visual orientation, sharing with stakeholders, on-device verification |
| **`assets/`** | Brand SVGs — wordmark, mark, favicon (+ on-dark variants) | Embedding the brand identity in product chrome |
| **`exports/`** | Six export formats + DTCG JSON + manifest + rationale (see below) | Consuming the design system in your tool of choice |

## Choose your export

The kit ships seven consumable formats in `exports/`. Pick whichever fits your tooling.

| If you use… | Open this file | How to consume it |
|---|---|---|
| **Tailwind CSS** | `exports/tailwind-preset.js` | Add to `tailwind.config.js`: `presets: [require('./exports/tailwind-preset.js')]` |
| **Plain CSS** | `exports/tokens.css` | Drop in a `<link>` or `@import`. Provides `--qs-*` and `--*` custom properties + four `.theme.*` mode overlays |
| **Designframe runtime** | `exports/df-input.css` | Replaces the `:root` section of your existing df-input.css. Pairs with the DF Tailwind preset for the full framework |
| **Figma (Tokens Studio plugin)** | `exports/figma-tokens-studio.json` | Tokens Studio → Import → "Use Token Studio JSON". Sets-based structure with theme toggles |
| **Google DESIGN.md / `@google/design.md` CLI** | `exports/design-md.md` | Validated against `@google/design.md@0.1.1` (0/0/0 lint). Includes MD3 color name crosswalk |
| **Raw DTCG JSON** | `exports/tokens.json` (+ `tokens.{dark,alt,invert,key}.json` mode overlays) | Any DTCG-compatible tool. 193 leaf tokens with `$brand-owned` / `$system-owned` markers + `$applied-guidance` prose |
| **AI tooling (claude.ai/design, etc.)** | `exports/tokens.json` + `exports/rationale.md` | Upload the pair: tokens JSON + 230 lines of prose rationale. The pair is designed to give AI agents enough context to reason about the system, not just consume the tokens mechanically |

## Fork it for your own brand

Designframe's brand identity lives in `PRISM.md` frontmatter under `brandOwnedRoots`:

```yaml
brandOwnedRoots:
  - color.brand
  - color.brand-alt
  - color.theme.header
  - color.theme.footer
  - typography.family
  - radius.brand
```

To rebrand: edit the values under these roots in PRISM.md (and `assets/logos/*.svg` for visual marks), then regenerate the kit:

```bash
# From a clone of designframe-prism
npm run build
```

`systemOwnedRoots` (alerts, spacing, screens, shadows, etc.) are stable across forks — they encode Designframe's design-system *contract*, not its brand identity. You can override them but you don't have to.

To start a fresh Prism Kit for an unrelated brand, use the generic starter template:

```bash
npx designframe-prism init --out ./my-kit/PRISM.md --name my-brand
```

## Verify locally

After unzipping, no build step is needed to view the kit:

```bash
# macOS
open design-system.html

# Linux
xdg-open design-system.html
```

All assets resolve as siblings or children — the kit is fully self-contained.

## What's not in this kit

By design, the kit ships the **tokens layer** of Designframe. The full Designframe framework (utility classes, combo classes, block hierarchy rules, adaptive layout math) is **not** ported — those layers are out of scope for Prism v1 (see [`../../spec/v2-reserved/README.md`](../../spec/v2-reserved/README.md)). If you want the full framework, use [`designframe-ui`](https://github.com/mikeprasad/designframe-ui) directly.

## License

The kit content (PRISM.md, exports, assets) is licensed under **Apache 2.0**. See [`../../LICENSE`](../../LICENSE).

The Designframe brand identity (wordmark, mark, key gradient, "Designframe" name) is © Mike Prasad — Apache 2.0 covers the *code and tokens*, not commercial brand-asset reuse outside this design-system context.

## Where to go next

- **The format spec:** [`../../spec/prism-md-format.md`](../../spec/prism-md-format.md) — the `prism-spec/1.0` grammar, lint rules, round-trip semantics
- **The tool:** [`../../README.md`](../../README.md) — what Prism is, how to bootstrap your own kit
- **Report issues / contribute:** [github.com/mikeprasad/designframe-prism](https://github.com/mikeprasad/designframe-prism)
