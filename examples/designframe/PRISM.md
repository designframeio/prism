---
format: prism-spec/1.0
name: designframe
version: 0.99.0
prismMdVersion: 1.0
description: |
  Designframe — minimalist adaptive design system by Mike Prasad.
  Built as a Tailwind CSS preset/extension; pure CSS output.
license: Apache-2.0
homepage: https://designframe.io
designframeVersion: 0.99
designMdCompatible: true

brandOwnedRoots:
  - color.brand
  - color.brand-alt
  - color.theme.header
  - color.theme.footer
  - typography.family
  - radius.brand

systemOwnedRoots:
  - screen
  - layout
  - space
  - size
  - shadow
  - shader
  - blur
  - border
  - transition
  - typography.scale
  - color.alert

modes:
  - name: default
    description: Light mode + main brand palette (cascade root)
    base: true
  - name: dark
    description: Dark mode — bg flips to primary (near-black), fg to invert (white)
    aliases: [ invert ]
  - name: alt
    description: Alt palette — secondary brand identity (e.g. campaign mode)
  - name: key
    description: Key brand color as canvas — saturated section bg with component key↔invert swap

source:
  dfInputCss: ../../../df-ui/df-working/src/assets/designframe/df-input.css
  tailwindConfig: ../../../df-ui/df-working/tailwind.config.js
  dfPreset: ../../../df-ui/df-working/src/assets/designframe/df-preset.js
  rationaleSources:
    - ../../../df-ui/df-working/docs/df-rules.md
    - ../../../df-ui/df-working/docs/df-knowledge.md
    - ../../../df-ui/df-working/docs/taxonomy.md
    - ../../../df-ui/df-working/src/assets/designframe/configuration.md
    - ../../../df-ui/df-working/src/assets/designframe/theming.md

color:
  brand:
    key: { $type: color, $value: "#222222", $brand-owned: true, $df-source: --qs-color-key, $description: "Brand key — flat brand-dark. Default theme is monochrome; key-end shares this value (degenerate gradient)." }
    key-end: { $type: color, $value: "#222222", $brand-owned: true, $df-source: --qs-color-key-end, $description: "Brand key gradient end-stop. Matches key in monochrome default theme — no operative gradient." }
    primary: { $type: color, $value: "#0c0c0c", $brand-owned: true, $df-source: --qs-color-primary, $description: "Primary text on light surfaces. Near-black; slightly darker than brand-identity #222222 to compensate for anti-aliasing weight loss at body-type sizes (the optical-weight discipline — see Color rationale)." }
    secondary: { $type: color, $value: "#757575", $brand-owned: true, $df-source: --qs-color-secondary, $description: "Secondary text — passes WCAG AA on white (4.61:1). Industry parallel: Material Design medium-emphasis text." }
    tertiary: { $type: color, $value: "#cccccc", $brand-owned: true, $df-source: --qs-color-tertiary, $description: "Tertiary tier — borders, dividers, low-emphasis chrome. Decorative; not for readable text. Fails WCAG by design — intentional low-attention." }
    invert: { $type: color, $value: "#ffffff", $brand-owned: true, $df-source: --qs-color-invert, $description: "Inverse foreground — pure white. Used on dark surfaces. Matches brand-asset SVG fill values (the mark's white circles, inverted wordmark)." }
    disabled: { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Disabled-state UI. Shares value with tertiary by intent — both target low-emphasis visual weight. WCAG carves out 'inactive UI components' from contrast requirements.", $df-source: --qs-color-disabled }
    background: { $type: color, $value: "#ffffff", $brand-owned: true, $df-source: --qs-color-background, $description: "Default page canvas — pure white. Matches brand-asset reality (mark on-dark variant + favicon outer ring)." }
    background-start: { $type: color, $value: "#dddddd", $brand-owned: true, $description: "Default-theme gradient bg start (lower-left corner). The lightness-matched inverse of #222222 (L* 14 ↔ L* 86) — atmospheric only, not for text contrast. Brand-math: gradient bridges from brand-dark's optical mirror to pure white.", $df-source: --qs-color-background-start }
    background-end: { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Default-theme gradient bg end (upper-right corner). Pure white. Gradient flows lower-left → upper-right (lift convention).", $df-source: --qs-color-background-end }
    background-direction: { $type: string, $value: "right top", $brand-owned: true, $df-source: --qs-color-background-direction, $description: "CSS gradient direction — 'right top' means gradient flows TOWARD upper-right (start at lower-left, end at upper-right)." }

  brand-alt:
    key: { $type: color, $value: "#000000", $brand-owned: true, $description: "Alt-theme key (lower-left of alt-key gradient). Absolute black — the most dramatic dark stop in the system. Pairs with key-end (#222222) for the alt-key gradient. Activated in mode:alt.", $df-source: --qs-color-key-alt }
    key-end: { $type: color, $value: "#222222", $brand-owned: true, $description: "Alt-theme key end (upper-right of alt-key gradient). Brand-dark — the gradient resolves to the brand-canonical rest position.", $df-source: --qs-color-key-end-alt }
    primary: { $type: color, $value: "#0c0c0c", $brand-owned: true, $description: "Alt-theme primary text. Mirrors default primary — alt-mode keeps the same light canvas + dark-text pattern as default; only key/key-end and background-start/end actually differ.", $df-source: --qs-color-primary-alt }
    secondary: { $type: color, $value: "#757575", $brand-owned: true, $description: "Alt-theme secondary text. Mirrors default secondary (WCAG AA).", $df-source: --qs-color-secondary-alt }
    tertiary: { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Alt-theme tertiary. Mirrors default tertiary (decorative tier).", $df-source: --qs-color-tertiary-alt }
    invert: { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Alt-theme invert. Mirrors default invert (pure white).", $df-source: --qs-color-invert-alt }
    disabled: { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Alt-theme disabled. Mirrors default disabled.", $df-source: --qs-color-disabled-alt }
    background: { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Alt-theme page canvas. Mirrors default background — same light canvas; alt theme varies via gradient stops + key colors, not via canvas inversion.", $df-source: --qs-color-background-alt }
    background-start: { $type: color, $value: "#cccccc", $brand-owned: true, $description: "Alt-theme gradient bg start (lower-left). Brand tertiary — visibly more atmospheric than default-theme gradient (which starts at #DDDDDD). Recognizable 'alt' feel without leaving monochrome.", $df-source: --qs-color-background-start-alt }
    background-end: { $type: color, $value: "#ffffff", $brand-owned: true, $description: "Alt-theme gradient bg end (upper-right). Pure white — alt and default gradients both resolve to white at upper-right (lift convention shared).", $df-source: --qs-color-background-end-alt }
    background-direction: { $type: string, $value: "right top", $brand-owned: true, $df-source: --qs-color-background-alt-direction, $description: "Alt-theme gradient direction. Matches default — lift toward upper-right." }

  alert:
    notify:
      base:       { $type: color, $value: "#60a5fa", $system-owned: true, $df-source: --color-alert-notify,            $description: "Notify alert identity color — saturated blue. Use for dots, icons, focus rings, badge fills." }
      heading:    { $type: color, $value: "#1d4ed8", $system-owned: true, $df-source: --color-alert-notify-heading,    $description: "Notify alert heading text. WCAG AA on alert.notify.background." }
      text:       { $type: color, $value: "#2563eb", $system-owned: true, $df-source: --color-alert-notify-text,       $description: "Notify alert body text. Slightly lighter than heading; WCAG AA on alert.notify.background." }
      background: { $type: color, $value: "#dbeafe", $system-owned: true, $df-source: --color-alert-notify-background, $description: "Notify alert container background — soft blue tint. The `.alert.notify` container surface." }
    warning:
      base:       { $type: color, $value: "#facc15", $system-owned: true, $df-source: --color-alert-warning,            $description: "Warning alert identity color — saturated yellow. Use for dots, icons, focus rings, badge fills." }
      heading:    { $type: color, $value: "#a16207", $system-owned: true, $df-source: --color-alert-warning-heading,    $description: "Warning alert heading text. Dark amber for WCAG AA on alert.warning.background." }
      text:       { $type: color, $value: "#ca8a04", $system-owned: true, $df-source: --color-alert-warning-text,       $description: "Warning alert body text. WCAG AA on alert.warning.background." }
      background: { $type: color, $value: "#fef9c3", $system-owned: true, $df-source: --color-alert-warning-background, $description: "Warning alert container background — soft yellow tint. The `.alert.warning` container surface." }
    error:
      base:       { $type: color, $value: "#f87171", $system-owned: true, $df-source: --color-alert-error,            $description: "Error alert identity color — saturated red. Use for dots, icons, focus rings, badge fills, destructive-action signaling." }
      heading:    { $type: color, $value: "#b91c1c", $system-owned: true, $df-source: --color-alert-error-heading,    $description: "Error alert heading text. Dark red for WCAG AA on alert.error.background." }
      text:       { $type: color, $value: "#dc2626", $system-owned: true, $df-source: --color-alert-error-text,       $description: "Error alert body text. WCAG AA on alert.error.background." }
      background: { $type: color, $value: "#fee2e2", $system-owned: true, $df-source: --color-alert-error-background, $description: "Error alert container background — soft red tint. The `.alert.error` container surface." }
    success:
      base:       { $type: color, $value: "#4ade80", $system-owned: true, $df-source: --color-alert-success,            $description: "Success alert identity color — saturated green. Use for dots, icons, focus rings, badge fills, pass-state signaling." }
      heading:    { $type: color, $value: "#15803d", $system-owned: true, $df-source: --color-alert-success-heading,    $description: "Success alert heading text. Dark green for WCAG AA on alert.success.background." }
      text:       { $type: color, $value: "#16a34a", $system-owned: true, $df-source: --color-alert-success-text,       $description: "Success alert body text. WCAG AA on alert.success.background." }
      background: { $type: color, $value: "#dcfce7", $system-owned: true, $df-source: --color-alert-success-background, $description: "Success alert container background — soft green tint. The `.alert.success` container surface." }

  _aliases:
    primary: { $ref: "{color.brand.key}" }
    primary-end: { $ref: "{color.brand.key-end}" }
    fg-default: { $ref: "{color.brand.primary}" }
    fg-muted: { $ref: "{color.brand.secondary}" }
    fg-subtle: { $ref: "{color.brand.tertiary}" }
    fg-on-dark: { $ref: "{color.brand.invert}" }
    bg-canvas: { $ref: "{color.brand.background}" }

  fg:
    default: { $ref: "{color.theme.default.fg}" }
    muted: { $ref: "{color.brand.secondary}" }
    subtle: { $ref: "{color.brand.tertiary}" }
    on-key: { $ref: "{color.theme.key.fg}" }
    on-dark: { $ref: "{color.theme.invert.fg}" }
    disabled: { $ref: "{color.brand.disabled}" }

  bg:
    canvas: { $ref: "{color.theme.default.bg}" }
    key: { $ref: "{color.theme.key.bg}" }
    key-start: { $ref: "{color.theme.key-gradient.bg-start}" }
    key-end: { $ref: "{color.theme.key-gradient.bg-end}" }
    invert: { $ref: "{color.theme.invert.bg}" }

shader:
  light: { $type: number, $value: 0.15, $system-owned: true, $description: Lightest shader alpha., $df-source: --shader-light }
  base: { $type: number, $value: 0.60, $system-owned: true, $df-source: --shader-base, $description: "Bare 'shader' = this value (naming asymmetry — see Do's and Don'ts)." }
  heavy: { $type: number, $value: 0.85, $system-owned: true, $description: Heavy shader alpha., $df-source: --shader-heavy }

typography:
  family:
    heading: { $type: fontFamily, $value: [ "Satoshi", "sans-serif", "system-ui" ], $brand-owned: true, $description: "Designframe canonical heading face — Satoshi (Indian Type Foundry, SIL OFL). Distinctive geometric grotesque that echoes the three-circle mark's construction. Heading + display share Satoshi; body uses General Sans (ITF's restrained sibling) per the foundry-house mono-family pattern. Forking brands replace 'Satoshi' here + the matching @font-face src in df-input.css.", $df-source: --font-heading, $font-asset: "{font.satoshi}" }
    body: { $type: fontFamily, $value: [ "General Sans", "sans-serif", "system-ui" ], $brand-owned: true, $description: "Designframe canonical body face — General Sans (Indian Type Foundry, SIL OFL). ITF's foundry-house mono-family sibling to Satoshi: same design language, role-calibrated as a restrained grid-derived neutral grotesque tuned for body-text legibility. The pairing reads as a deliberate foundry-house system (Apple SF Pro Display + SF Pro Text pattern). Forking brands can swap to another body face by changing this $value + the matching @font-face binding.", $df-source: --font-body, $font-asset: "{font.general-sans}" }
    display: { $type: fontFamily, $value: [ "Satoshi", "sans-serif", "system-ui" ], $brand-owned: true, $description: "Designframe canonical display face — same Satoshi as heading. Display + heading share the distinctive geometric grotesque; body diverges to General Sans (the restrained ITF sibling). The 2:1 split within the foundry-house mono-family system creates hierarchy between display-tier and body-tier without importing a second foundry tradition. Brands wanting a distinct display face carve out by changing this $value without affecting heading/body.", $df-source: --font-display, $font-asset: "{font.satoshi}" }
    mono: { $type: fontFamily, $value: [ "Commit Mono", "ui-monospace", "monospace" ], $brand-owned: true, $description: "Designframe canonical mono face — Commit Mono (Eigil Nikolajsen, SIL OFL). For code surfaces (PRISM.md fences, design-system.html token paths, inline <code>, IDE-flavored docs). Geometric construction + ligature-free design (deliberate authorial choice by Eigil) aligns with Designframe's 'math + structural defaults' philosophy.", $df-source: --font-mono, $font-asset: "{font.commit-mono}" }
  scale:
    base: { size: { $type: dimension, $value: "16px", $system-owned: true, $description: Base type size. Equivalent to 1rem and to text-p3., $df-source: --text-base-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $description: Base line height. Anchor for all DF type rhythm. See df-rules.md §lineHeight-anchor., $df-source: --text-base-lh } }
    d1: { size: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --text-d1-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d1-lh } }
    d2: { size: { $type: dimension, $value: "72px", $system-owned: true, $df-source: --text-d2-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d2-lh } }
    d3: { size: { $type: dimension, $value: "60px", $system-owned: true, $df-source: --text-d3-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-d3-lh } }
    h1: { size: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --text-h1-size }, lh: { $type: number, $value: 1, $system-owned: true, $df-source: --text-h1-lh } }
    h2: { size: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --text-h2-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-h2-lh } }
    h3: { size: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-h3-size }, lh: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --text-h3-lh } }
    h4: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-h4-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-h4-lh } }
    h5: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-h5-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-h5-lh } }
    h6: { size: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-h6-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-h6-lh } }
    p1: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-p1-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-p1-lh } }
    p2: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-p2-size }, lh: { $type: dimension, $value: "28px", $system-owned: true, $df-source: --text-p2-lh } }
    p3: { size: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-p3-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-p3-lh } }
    p4: { size: { $type: dimension, $value: "14px", $system-owned: true, $df-source: --text-p4-size }, lh: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-p4-lh } }
    p5: { size: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --text-p5-size }, lh: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --text-p5-lh } }
    nav: { size: { $type: dimension, $value: "20px", $system-owned: true, $df-source: --text-nav-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-nav-lh } }
    button: { size: { $type: dimension, $value: "18px", $system-owned: true, $df-source: --text-button-size }, lh: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --text-button-lh, $description: "Inherits text-nav-lh." } }
    chip: { size: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --text-chip-size }, lh: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --text-chip-lh } }

space:
  unit: { $type: dimension, $value: "4px", $system-owned: true, $df-source: --layout-unit, $description: "Base layout unit. All spacing multiples derive from this." }
  gutter:
    base: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --spacing-gutter-base }
    sm: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥640px., $df-source: --spacing-gutter-sm }
    md: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥768px., $df-source: --spacing-gutter-md }
    lg: { $type: dimension, $value: "24px", $system-owned: true, $description: ≥960px., $df-source: --spacing-gutter-lg }
    xl: { $type: dimension, $value: "32px", $system-owned: true, $description: ≥1440px., $df-source: --spacing-gutter-xl }
    2xl: { $type: dimension, $value: "32px", $system-owned: true, $description: ≥1600px., $df-source: --spacing-gutter-2xl }
  shoulder:
    base: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --spacing-shoulder-base }
    sm: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --spacing-shoulder-sm }
    md: { $type: dimension, $value: "36px", $system-owned: true, $df-source: --spacing-shoulder-md }
    lg: { $type: dimension, $value: "36px", $system-owned: true, $df-source: --spacing-shoulder-lg }
    xl: { $type: dimension, $value: "40px", $system-owned: true, $df-source: --spacing-shoulder-xl }
    2xl: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --spacing-shoulder-2xl }
  article: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-article-base }, xl: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --pad-article-xl } }
  element: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-element-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-element-xl } }
  section: { base: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --pad-section-base }, xl: { $type: dimension, $value: "64px", $system-owned: true, $df-source: --pad-section-xl } }
  header: { base: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --pad-header-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --pad-header-xl } }
  stack: { base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-stack-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-stack-xl } }
  form: { base: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --gap-form-base }, xl: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-form-xl } }
  sub: { base: { $type: dimension, $value: "8px", $system-owned: true, $df-source: --spacing-sub-base }, xl: { $type: dimension, $value: "12px", $system-owned: true, $df-source: --spacing-sub-xl } }
  hgroup:
    display: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --gap-hgroup-display, $description: "HGROUP gap when display-level." }
    heading: { $type: dimension, $value: "8px", $system-owned: true, $df-source: --gap-hgroup-heading }
    title: { $type: dimension, $value: "4px", $system-owned: true, $df-source: --gap-hgroup-title }
  _aliases:
    md: { $ref: "{space.gutter.base}" }
    sm: { $ref: "{space.article.base}" }
    xs: { $ref: "{space.sub.base}" }
    2xs: { $ref: "{space.unit}" }

size:
  element:
    hairline: { $type: dimension, $value: "1px", $system-owned: true, $description: Hairline border/divider thickness., $df-source: --element-hairline-size }
    min: { $type: dimension, $value: "24px", $system-owned: true, $description: Smallest element height. Matches text-base-lh., $df-source: --element-min-size }
    sub: { $type: dimension, $value: "16px", $system-owned: true, $description: Sub-element height., $df-source: --element-sub-size }
    compact: { $type: dimension, $value: "32px", $system-owned: true, $description: Compact interactive height., $df-source: --element-compact-size }
    base: { $type: dimension, $value: "40px", $system-owned: true, $description: "Standard interactive height (button, input).", $df-source: --element-base-size }

radius:
  brand:
    min: { $type: dimension, $value: "4px", $brand-owned: true, $description: "Subtle softening for small surfaces — chips, badges, .rounded checkboxes, skeleton text rows. The middle tier between fully square (.base 0px) and pill (.full 9999px). Preserves the .rounded checkbox variant as semantically distinct from plain-square.", $df-source: --qs-rounded-min }
    base: { $type: dimension, $value: "0px", $brand-owned: true, $description: "Base element rounding — buttons, inputs, avatars. Fully square: geometric crispness for the most-visible interactive surfaces. Matches the brand mark's true-square geometry.", $df-source: --qs-rounded-base }
    corner: { $type: dimension, $value: "4px", $brand-owned: true, $description: "Container rounding — LANDMARK/SECTION/BLOCK frames, cards. Subtle softening (4px) for content containers while controls (base) stay fully square. The DF radius story in one sentence: controls square, containers softened.", $df-source: --qs-rounded-corner }
    field: { $type: dimension, $value: "4px", $brand-owned: true, $description: "Form-input rounding — text fields, selects, textareas. Matches container rounding (.corner) at 4px. The whole framework now reads as 'one subtle softening pass' across all non-control surfaces.", $df-source: --qs-rounded-field }
  full: { $type: dimension, $value: "9999px", $system-owned: true, $description: Pill/circle radius., $df-source: --rounded-full }
  chip: { $ref: "{radius.brand.min}" }
  article:
    base: { $type: dimension, $value: "16px", $system-owned: true, $df-source: --rounded-article-base, $description: "Article (card) rounding. Matches pad-article per breakpoint." }
    xl: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --rounded-article-xl }

shadow:
  alpha:
    light: { $type: number, $value: 0.125, $system-owned: true, $description: Light shadow alpha., $df-source: --shadow-alpha-light }
    base: { $type: number, $value: 0.25, $system-owned: true, $description: Base shadow alpha., $df-source: --shadow-alpha-base }
    heavy: { $type: number, $value: 0.25, $system-owned: true, $df-source: --shadow-alpha-heavy, $description: "Same numeric value as base; offset/blur do the differentiation." }

blur:
  light: { $type: dimension, $value: "24px", $system-owned: true, $df-source: --blur-light }
  base: { $type: dimension, $value: "48px", $system-owned: true, $df-source: --blur-base }
  heavy: { $type: dimension, $value: "64px", $system-owned: true, $df-source: --blur-heavy }
  max: { $type: dimension, $value: "128px", $system-owned: true, $df-source: --blur-max }

border:
  base: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-base, $description: "Utility-only; not auto-applied." }
  button: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-button }
  form: { $type: dimension, $value: "1px", $system-owned: true, $df-source: --border-form }

transition:
  duration: { $type: duration, $value: "300ms", $system-owned: true, $description: Base transition duration., $df-source: --transition-duration-base }
  timing: { $type: cubicBezier, $value: [ 0.4, 0, 0.2, 1 ], $system-owned: true, $description: Base transition easing., $df-source: --transition-timing-base }
  property: { $type: string, $value: "all", $system-owned: true, $description: Base transition property., $df-source: --transition-property-base }

layout:
  cols:
    base: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-base }
    sm: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-sm }
    md: { $type: number, $value: 6, $system-owned: true, $df-source: --layout-cols-md }
    lg: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-lg }
    xl: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-xl }
    2xl: { $type: number, $value: 12, $system-owned: true, $df-source: --layout-cols-2xl }
  col-width:
    base: { $type: dimension, $value: "32px", $system-owned: true, $df-source: --layout-col-width-base }
    sm: { $type: dimension, $value: "76px", $system-owned: true, $df-source: --layout-col-width-sm }
    md: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --layout-col-width-md }
    lg: { $type: dimension, $value: "52px", $system-owned: true, $df-source: --layout-col-width-lg }
    xl: { $type: dimension, $value: "84px", $system-owned: true, $df-source: --layout-col-width-xl }
    2xl: { $type: dimension, $value: "96px", $system-owned: true, $df-source: --layout-col-width-2xl }

screen:
  sm: { $type: dimension, $value: "640px", $system-owned: true, $df-source: --screen-sm }
  md: { $type: dimension, $value: "768px", $system-owned: true, $df-source: --screen-md }
  lg: { $type: dimension, $value: "960px", $system-owned: true, $df-source: --screen-lg }
  xl: { $type: dimension, $value: "1440px", $system-owned: true, $df-source: --screen-xl }
  2xl: { $type: dimension, $value: "1600px", $system-owned: true, $df-source: --screen-2xl }
---

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

The brand identity is **a two-color monochrome system** — `color.brand.key` and `color.brand.key-end` both resolve to `#222222` (the brand-dark), and `color.brand.invert` is pure white. The `key`/`key-end` token pair is preserved for DF token-surface compatibility; in the default theme they share a value (degenerate gradient — effectively flat brand-dark). The `mode:key` overlay then activates this brand-dark as a saturated canvas, and the `color.brand-alt.key` / `color.brand-alt.key-end` pair (`#000000` → `#222222`) provides the actual gradient experience when `mode:alt` is active.

```token
color.brand.key:
  $type: color
  $value: "#222222"
  $brand-owned: true
  $df-source: --qs-color-key
  $description: Brand key — flat brand-dark. Default theme is monochrome; key-end shares this value.
  $applied-guidance: |
    Use as the brand-identity accent — button-key fills, key-tinted backgrounds, link-hover via `.text-key`. In the default theme, key and key-end share the value #222222, so any "gradient" rendering collapses to a flat brand-dark surface. For an actual gradient brand experience, switch to mode:alt where the alt-key gradient (#000000 → #222222) is operative.
  $ai-hint: |
    When generating brand-key UI elements, treat key as a flat color in default theme — gradient utilities like bg-gradient-key resolve to a degenerate (flat) gradient that's visually identical to bg-key. Only in mode:alt does the gradient experience activate; design with that mode-switch in mind if a gradient is the desired brand feel.
```

```token
color.brand.key-end:
  $type: color
  $value: "#222222"
  $brand-owned: true
  $df-source: --qs-color-key-end
  $description: Brand key gradient end-stop. Matches key in monochrome default theme (no operative gradient).
  $applied-guidance: |
    Paired with color.brand.key for the default-theme key gradient. Both stops are #222222 in the canonical Designframe brand, producing a flat (degenerate) gradient. Direction is controlled by color.brand.background-direction at theme level (default: right top) but has no visual effect when both stops match. The mode:alt overlay provides the actual gradient experience via color.brand-alt.key (#000000) → color.brand-alt.key-end (#222222).
```

```token
color.brand.primary:
  $type: color
  $value: "#0c0c0c"
  $brand-owned: true
  $df-source: --qs-color-primary
  $description: Primary text — darkest monotone.
  $applied-guidance: |
    Default body-text color in the cascade. Apply via `text-primary` for headings, body, captions, and labels — never as a hardcoded hex (`text-black`, `#000`) or raw color. Inside `.theme.invert`, `text-primary` auto-resolves to color.brand.invert via the cascade; hardcoded values stay dark and break against dark backgrounds. Per df-rules.md §10.1 the contract is "primary = the foreground for the current theme," not "primary = always black."
  $ai-hint: |
    When generating prose, headings, or labels, do NOT set color via inline styles or utilities — the default cascade resolves to primary automatically. Only use `text-primary` explicitly when overriding a previously-set foreground (e.g., re-asserting body color inside a card on a key-themed section).
```

```token
color.brand.secondary:
  $type: color
  $value: "#757575"
  $brand-owned: true
  $df-source: --qs-color-secondary
  $description: Secondary text — mid-grey passing WCAG AA on white (4.61:1). Industry parallel — Material Design medium-emphasis text.
  $applied-guidance: |
    Use via `text-secondary` for de-emphasized but still-readable content — help text, captions, metadata, form hints, footer subtext. Carries less attention weight than `text-primary` while remaining body-text accessible (4.61:1 contrast on white, passes WCAG AA normal). Inside `.theme.invert` / `.theme.key`, the cascade swaps to an appropriately lighter equivalent. Don't reach for `text-gray-500` or hardcoded grays — those bypass the cascade and may fail accessibility.
```

```token
color.brand.tertiary:
  $type: color
  $value: "#cccccc"
  $brand-owned: true
  $df-source: --qs-color-tertiary
  $description: Tertiary text — light gray, still legible.
  $applied-guidance: |
    Lowest text tier still intended to be read. Use via `text-tertiary` for short metadata — timestamps, dividers between inline items, lowest-priority labels. Below WCAG AA body contrast on raw white; reserve for short labels or pair with a shader-tinted background that lifts effective contrast. Don't use for body paragraphs or anywhere users must read a sustained line of text.
```

```token
color.brand.invert:
  $type: color
  $value: "#ffffff"
  $brand-owned: true
  $df-source: --qs-color-invert
  $description: Invert color — white. Used on dark backgrounds.
  $applied-guidance: |
    Foreground for dark themes (`.theme.invert`, `.theme.key`, `.theme.key-gradient`). Pairs with `bg-primary` in invert mode and `bg-key` in key mode. Don't apply `text-invert` directly to elements in light themes — it locks white-on-white. The cascade resolves `text-primary` to invert automatically when the parent section carries an inverting theme class; hand-applied `text-invert` defeats the cascade and won't re-flip on theme change.
  $ai-hint: |
    When generating UI inside a `.theme.invert` or `.theme.key` section, do NOT explicitly set `text-invert` on individual elements — the cascade does it. The only legitimate explicit use is in a fixed dark surface that should stay dark regardless of containing theme (rare; document the exception).
```

### Theme contexts

Theme contexts are semantic-tier tokens that compose primitives into role-based `{bg, fg}` pairs. The same semantic token (e.g. `color.theme.default.fg`) resolves to different values across modes — the role stays constant, the value changes.

```token
color.theme.default.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Default theme background. Light mode = white.
  $applied-guidance: |
    Default page-level background. Pairs with color.theme.default.fg per the fg/bg pairing rule. Don't override per-section; theme context is a section-level decision via `.theme` class on a `<section>`.
```

```token
color.theme.default.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Default theme foreground (primary text on canvas).
  $applied-guidance: |
    Default body-text color, paired with color.theme.default.bg. Inherited by `<p>`, `<h1>`–`<h6>`, `<a>` defaults — don't set color manually on these elements unless overriding for a deliberate exception.
  $ai-hint: |
    When generating prose paragraphs or headings, do NOT set color via inline styles or utilities. The default cascade handles it; setting `text-primary` is redundant and `text-black` is wrong (it bypasses the cascade per df-rules.md §10.1).
```

```token
color.theme.invert.bg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Inverted theme background (dark mode).
  $applied-guidance: |
    Dark-mode section bg = primary (near-black). Apply via `.theme.invert` on a section element, not by setting bg-primary directly — the class triggers full atomic invert (card auto-revert, button gradient-key-invert behavior, shader recalibration). Manual color flipping misses these per df-rules.md §10.5.
```

```token
color.theme.invert.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Inverted theme foreground (dark mode).
  $applied-guidance: |
    Foreground role for inverted (dark) theme sections. Resolves to color.brand.invert via cascade when `.theme.invert` is applied to a section. Don't reference this semantic token directly in markup — write `text-primary` and let the cascade resolve to invert automatically inside `.theme.invert`. Lint rule 3 (contrast-ratio) verifies WCAG AA pairing with color.theme.invert.bg.
```

```token
color.theme.key.bg:
  $type: color
  $ref: "{color.brand.key}"
  $contrastTarget: large
  $description: Key theme background — saturated brand color as section bg.
  $applied-guidance: |
    Use for hero bands, CTA strips, brand-emphasized sections. Per df-rules.md §10.4, components inside `.theme.key` auto-invert (key↔invert swap on buttons/chips/form rings) so they remain visible on the saturated bg. Don't manually flip component colors in key sections — the cascade handles it. $contrastTarget=large signals the WCAG-AA threshold is 3:1 (display/hero), not 4.5:1 (body); body text inside a key section auto-inverts to higher-contrast invert per the cascade.
```

```token
color.theme.key.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Key theme foreground — invert (white) on saturated bg.
  $applied-guidance: |
    Foreground role for key-themed sections — resolves to invert (white) over the saturated key brand bg. Active inside `.theme.key` and `.theme.key-gradient` sections. The semantic-tier `$contrastTarget: large` on color.theme.key.bg signals the WCAG-AA threshold for *bg* is 3:1 (display-scale text); for body-text contrast use `text-primary` in a card or alt-styled child element so the cascade lifts contrast to 4.5:1. Per df-rules.md §10.4, components inside `.theme.key` auto-swap key↔invert; manual color flipping defeats the cascade.
```

The 14 theme contexts compose into four orthogonal axes: **palette** (default vs alt-brand), **mode** (light vs invert), **saturation** (transparent overlay vs solid surface), and **landmark** (header/footer landmarks have their own brand-owned color slots). Per df-rules.md §10.2, applying a `.theme.*` class on a `<section>` reassigns the `--color-*` cascade for everything inside; per-component overrides defeat the cascade.

```token
color.theme.default-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Default theme with transparent bg — defers to parent surface. DF-native — .theme.transparent.
```

```token
color.theme.default-transparent.fg:
  $type: color
  $ref: "{color.theme.default.fg}"
  $description: Default theme transparent foreground (fg unchanged, bg surrendered to parent).
```

```token
color.theme.alt.bg:
  $type: color
  $ref: "{color.brand-alt.background}"
  $description: Alt theme background — light mode + alt brand palette. DF-native — .theme.alt.
  $applied-guidance: |
    Background role for sections themed with the alternate brand palette (`.theme.alt`). Use the class on a `<section>`, not the semantic token directly — applying `.theme.alt` reassigns the entire `--color-*` cascade to alt counterparts (color.brand-alt.*) for everything inside. Per df-rules.md §10.2, direct `bg-background-alt` usage locks the element to alt regardless of surrounding theme; generic `bg-background` inside `.theme.alt` resolves here automatically.
```

```token
color.theme.alt.fg:
  $type: color
  $ref: "{color.brand-alt.primary}"
  $description: Alt theme foreground.
  $applied-guidance: |
    Foreground role paired with color.theme.alt.bg inside `.theme.alt` sections. Same cascade rule as the bg: use generic `text-primary` and let the cascade resolve to alt automatically — direct `text-primary-alt` usage locks the element to alt regardless of surrounding theme (df-rules.md §10.2 anti-pattern). Lint rule 3 (contrast-ratio) enforces WCAG AA against color.theme.alt.bg.
```

```token
color.theme.alt-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Alt theme with transparent bg.
```

```token
color.theme.alt-transparent.fg:
  $type: color
  $ref: "{color.theme.alt.fg}"
  $description: Alt theme transparent foreground.
```

```token
color.theme.invert-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Invert theme with transparent bg.
```

```token
color.theme.invert-transparent.fg:
  $type: color
  $ref: "{color.theme.invert.fg}"
  $description: Invert theme transparent foreground.
```

```token
color.theme.alt-invert.bg:
  $type: color
  $ref: "{color.brand-alt.primary}"
  $description: Alt + inverted theme — dark mode using alt palette. DF-native — .theme.alt-invert. Composes alt cascade on top of invert cascade.
```

```token
color.theme.alt-invert.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt-invert foreground.
```

```token
color.theme.alt-invert-transparent.bg:
  $type: color
  $value: transparent
  $system-owned: true
  $contrastTarget: decorative
  $description: Alt-invert theme with transparent bg.
```

```token
color.theme.alt-invert-transparent.fg:
  $type: color
  $ref: "{color.theme.alt-invert.fg}"
  $description: Alt-invert transparent foreground.
```

The key-gradient context decomposes the gradient into bg/bg-start/bg-end/bg-direction so emitters can reconstruct the `linear-gradient(direction, start, end)` declaration; the bare `bg` field carries a solid fallback (defaults to `bg-start`) for consumers that don't support gradient bg.

```token
color.theme.key-gradient.bg:
  $type: color
  $ref: "{color.brand.key}"
  $contrastTarget: large
  $description: Key-gradient solid fallback for non-gradient consumers.
```

```token
color.theme.key-gradient.bg-start:
  $type: color
  $ref: "{color.brand.key}"
  $description: Key-gradient start stop.
```

```token
color.theme.key-gradient.bg-end:
  $type: color
  $ref: "{color.brand.key-end}"
  $description: Key-gradient end stop.
```

```token
color.theme.key-gradient.bg-direction:
  $type: string
  $ref: "{color.brand.background-direction}"
  $description: Key-gradient direction (CSS keyword form).
```

```token
color.theme.key-gradient.fg:
  $type: color
  $ref: "{color.brand.invert}"
  $description: Key-gradient foreground (invert on saturated bg).
```

```token
color.theme.key-alt.bg:
  $type: color
  $ref: "{color.brand-alt.key}"
  $contrastTarget: large
  $description: Alt key theme — solid alt-key brand color as section bg.
```

```token
color.theme.key-alt.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt key foreground.
```

```token
color.theme.key-alt-gradient.bg:
  $type: color
  $ref: "{color.brand-alt.key}"
  $contrastTarget: large
  $description: Alt key-gradient solid fallback.
```

```token
color.theme.key-alt-gradient.bg-start:
  $type: color
  $ref: "{color.brand-alt.key}"
  $description: Alt key-gradient start stop.
```

```token
color.theme.key-alt-gradient.bg-end:
  $type: color
  $ref: "{color.brand-alt.key-end}"
  $description: Alt key-gradient end stop.
```

```token
color.theme.key-alt-gradient.fg:
  $type: color
  $ref: "{color.brand-alt.invert}"
  $description: Alt key-gradient foreground.
```

Landmark contexts (header + footer) have brand-owned color slots distinct from main theme so brands can color their chrome (top nav, footer) independent of the article surface.

```token
color.theme.header.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Header landmark background. Source — --qs-color-header-background.
```

```token
color.theme.header.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Header landmark foreground. Source — --qs-color-header-primary.
```

```token
color.theme.header.nav-link:
  $type: color
  $ref: "{color.brand.secondary}"
  $description: Header nav link color. Source — --qs-color-header-nav-link.
```

```token
color.theme.footer.bg:
  $type: color
  $ref: "{color.brand.background}"
  $description: Footer landmark background. Source — --qs-color-footer-background.
```

```token
color.theme.footer.fg:
  $type: color
  $ref: "{color.brand.primary}"
  $description: Footer landmark foreground.
```

```token
color.theme.footer.nav-link:
  $type: color
  $ref: "{color.brand.secondary}"
  $description: Footer nav link color.
```

### Alert colors

System-owned colors for status-signaling. Per df-rules.md §10.7, alert tokens are namespaced (`bg-alert-notify`, NOT `bg-notify`) to prevent collision with semantic vocabulary.

Each alert level carries **four surfaces** — `base` (saturated identity color), `heading` (dark heading text), `text` (body text), `background` (soft-tint container). This 4-surface model matches the DF runtime composition (`df-input.css:3169-3245`): an `.alert.{level}` container uses `background` + `heading` together, while `.dot.{level}` / `.badge.{level}` indicators use `base` for the saturated fill. Modeling 4 surfaces (vs collapsing to a `{bg, fg}` pair) lets consumers reproduce both alert containers *and* the indicator family without inventing intermediate values.

```token
color.alert.error.background:
  $type: color
  $value: "#fee2e2"
  $system-owned: true
  $description: Error alert container background — soft red tint.
  $applied-guidance: |
    Apply to alert containers signaling error state — form validation, destructive-action confirmations, system failures. Pair with color.alert.error.heading (#b91c1c) for body-heading text and color.alert.error.text (#dc2626) for paragraph text; both pass WCAG AA against this soft-red surface. Don't apply error.base (#f87171) as a container background — that's the saturated identity color reserved for dots, focus rings, and badge fills, and its WCAG contrast against typical heading text is below AA.
```

```token
color.alert.error.base:
  $type: color
  $value: "#f87171"
  $system-owned: true
  $description: Error alert identity color — saturated red.
  $applied-guidance: |
    Apply to status indicators that need attention-level saturation: `.dot.error` indicator dots, `.badge.dot.error` chips, focus rings on form fields in error state, and the left-stripe accent on the alert container. Don't use as a container background (use color.alert.error.background instead) and don't use as body text (use color.alert.error.text instead — it's calibrated for WCAG AA on the soft-tint container).
```

### Shader system

DF's shader tokens are alpha multipliers used to derive subtle-tint backgrounds from any brand color. The three values map to `light` (0.15), `base` (0.60), `heavy` (0.85).

> **NOTE:** Bare `--shader` (no suffix) resolves to `--shader-base` (0.60), NOT `--shader-light` (0.15). This is a known footgun documented in df-knowledge.md "Shader utility naming asymmetry" — when authoring custom CSS, prefer the explicit suffix.

## Typography

DF's type scale is anchored on a **16px base font-size + 24px line-height** rhythm. Every scale step (d1-d3 display, h1-h6 heading, p1-p5 body, i1-i6 icon) is a multiple or fraction of these anchors.

### Font roles

Four font-family roles (`heading`, `body`, `display`, `mono`) populated via `@font-face` declarations in `df-input.css` CONFIG Fonts. Canonical Designframe setup is an **ITF foundry-house mono-family system**: **Satoshi** (Indian Type Foundry, SIL OFL, variable axis weight 300-900, upright + italic) for `heading` + `display` (the distinctive geometric grotesque echoing the three-circle mark); **General Sans** (Indian Type Foundry, SIL OFL, variable axis weight 200-700, upright + italic) for `body` (ITF's restrained sibling, grid-derived neutral grotesque tuned for body-text legibility — the Apple SF Pro Display + SF Pro Text mono-family pattern applied across two ITF roles); **Commit Mono** (Eigil Nikolajsen, SIL OFL, variable axis weight 300-700, ligature-free by design) for `mono` code surfaces. All three faces shipped in the kit at `assets/fonts/` (no external CDN dependency). Forking brands replace `typography.family.*` $value arrays + the matching @font-face declarations in df-input.css.

```token
typography.scale.p3.size:
  $type: dimension
  $value: "16px"
  $system-owned: true
  $description: Body text P3 size — equivalent to base. The anchor for the type scale.
  $applied-guidance: |
    Default body-text size. All other scale steps are ratios of this value. If you change it, recompute every paired line-height — DF's 24px line-height rhythm breaks at off-scale sizes. The p1-p5 ladder is a 1.25× ratio at the heavy end (p1=20px → p3=16px) and 0.875×/0.75× at the light end (p3=16px → p4=14px → p5=12px).
```

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

```token
space.gutter.base:
  $type: dimension
  $value: "24px"
  $system-owned: true
  $df-source: --spacing-gutter-base
  $description: Standard gutter spacing — mobile-first.
  $applied-guidance: |
    Use for section-level rhythm (between blocks in section-grid, between sections via m-section). Don't override per-component; gutter is a system-level decision and per-component overrides break the adaptive grid math.
```

## Elevation & Depth

Three shadow tiers (`light`, `base`, `heavy`) carry the elevation system. Plus a separate `shadow.alpha.*` sub-token scale for compositional shadows (inset, side-only, directional composites) where the full `shadow.*` tokens can't be reused.

Per df-rules.md §14.1, `box-shadow` is a single property — setting it replaces any previous value. Adding a raw custom shadow (e.g. focus ring) on an element with `shadow-light` wipes out the depth shadow entirely. Use shadow tokens (`shadow-light` / `shadow-base` / `shadow-heavy`) instead of hand-authored `box-shadow: 0 1px 3px rgb(0 0 0 / 0.1)`.

Composite shadows live in body blocks (not frontmatter) per spec §6.5 rule 5 — `$type: shadow` is a composite type whose `$value` is an object describing offset/blur/spread/color.

```token
shadow.light:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "0px"
    blur: "4px"
    spread: "0px"
    color: "rgba(0,0,0,0.125)"
  $system-owned: true
  $df-source: --shadow-light
  $description: Lightest elevation tier — surface separation without committing to depth. Alpha matches shadow.alpha.light.
```

```token
shadow.base:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "2px"
    blur: "4px"
    spread: "0px"
    color: "rgba(0,0,0,0.25)"
  $system-owned: true
  $df-source: --shadow-base
  $description: Standard elevation — default card / popover shadow. Alpha matches shadow.alpha.base.
```

```token
shadow.heavy:
  $type: shadow
  $value:
    offsetX: "0px"
    offsetY: "4px"
    blur: "8px"
    spread: "2px"
    color: "rgba(0,0,0,0.25)"
  $system-owned: true
  $df-source: --shadow-heavy
  $description: Heaviest elevation — modal / drawer / overlay shadow. Offset + blur do the differentiation from base; alpha is identical to base.
```

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

The Designframe identity is **a two-color monochrome system**: brand-dark `#222222` and pure white `#FFFFFF`. The mark is a *structural diagram* of Designframe's architecture — three circles in √2 ratios encoding the three control tiers (**LANDMARK** / **SECTION** / **BLOCK**) where Designframe applies its theming and adaptive sizing via CSS cascade. The wordmark is "DESIGNFRAME" set in geometric uppercase, drawn in solid `#222222`. There is no gradient lockup — the brand is intentionally monochromatic; on-dark variants invert the fill rather than introducing color.

### Logo

```asset
logo.primary:
  $type: image
  $path: assets/logos/df-wordmark.svg
  $description: Primary Designframe wordmark — "DESIGNFRAME" in solid #222222, geometric uppercase, 784×72 proportions. Monochrome only; no gradient.
  $applied-guidance: |
    Default lockup for light surfaces. Maintain clear-space equal to space.gutter.base on all sides. Minimum height 24px for legibility. Don't recolor — the wordmark is intentionally monochromatic; color treatments dilute the identity. Don't outline, drop-shadow, or apply effects. Don't stretch non-uniformly. For dark surfaces, switch to the on-dark variant (white letterforms, same geometry).
  $clear-space:
    $ref: "{space.gutter.base}"
  $min-size:
    $ref: "{size.element.base}"
  $variants:
    on-dark: assets/logos/df-wordmark-on-dark.svg
```

```asset
logo.mark:
  $type: image
  $path: assets/logos/df-mark.svg
  $description: Designframe mark — three solid circles of decreasing size, aligned on the horizontal centerline of a 1024×1024 square canvas. The default variant places white circles on a #222222 canvas; the on-dark variant inverts to #222222 circles on a white canvas. Square geometry — no corner radius. The three circles, sized in √2 ratios, encode Designframe's three control tiers — LANDMARK (the page-level landmarks header/footer/main), SECTION (containers within), and BLOCK (content within sections) — the three points where Designframe applies its theming and adaptive sizing via CSS cascade.
  $applied-guidance: |
    Use when wordmark length is constrained (app tile, social avatar, square thumbnail, top-nav lockup). The mark canvas is a true square — don't apply corner radius; the flat geometry is a deliberate identity choice (the brand-mark squareness is asset-side, independent of radius.brand.corner which applies to containers). Default variant: white circles on #222222 canvas. On-dark variant: #222222 circles on white canvas (use against dark surfaces). Minimum size 32px square — below that the three-circle scale hierarchy becomes visually indistinct; use logo.favicon instead.
  $clear-space:
    $ref: "{space.article.base}"
  $min-size:
    $ref: "{size.element.compact}"
  $variants:
    on-dark: assets/logos/df-mark-on-dark.svg
    favicon: assets/logos/favicon.svg
```

```asset
logo.favicon:
  $type: image
  $path: assets/logos/favicon.svg
  $description: Designframe favicon — the three-circle mark inscribed in a full-bleed white circle. Geometrically the same motif as logo.mark but bounded by a circular outline rather than the square canvas, sized for 16-32px browser-tab and PWA-icon contexts where a circular mask is common.
  $applied-guidance: |
    Reserved for contexts where the square logo.mark canvas doesn't fit a target surface's icon mask (browser tabs, OS app dock, system notification badges that crop to a circular mask). At any size where logo.mark renders cleanly within its container, prefer logo.mark — the favicon is a mask-shape adaptation, not a scale-down alternative.
  $min-size:
    $ref: "{size.element.sub}"
```

### Fonts

Designframe's canonical typography is an **ITF foundry-house mono-family system**: Satoshi for `heading` + `display` tiers (the distinctive geometric grotesque), General Sans for `body` tier (the restrained sibling grid-derived for body legibility), Commit Mono for `mono` code surfaces. The three faces are designed within the same modernist-restraint philosophy — two from ITF (Satoshi + General Sans share design DNA, role-calibrated for display vs body) and one cross-foundry mono designed with aligned considered-typography philosophy. All variable-axis woff2 files shipped in the kit at `assets/fonts/` — no external CDN dependencies. The mono-family pattern mirrors Apple's SF Pro Display + SF Pro Text approach: one design language, multiple role-calibrated voices. Differentiation between heading/display (Satoshi) and body (General Sans) comes from face character difference; differentiation within heading/display tier comes from scale + weight.

```asset
font.satoshi:
  $type: font-file
  $path: assets/fonts/Satoshi-Variable.woff2
  $format: woff2-variations
  $description: Designframe canonical heading/body/display face. Satoshi by Indian Type Foundry — variable axis weight 300-900, upright. Geometric grotesque with circular round-letters that echo the three-circle mark and hard squared terminals that echo the "DESIGNFRAME" wordmark.
  $applied-guidance: |
    The primary typeface for all Designframe text surfaces — every heading, paragraph, label, button, and display headline resolves to Satoshi via typography.family.{heading,body,display}. The variable weight axis (300-900) provides continuous interpolation, allowing the design system to express tier-differentiation through weight without shipping multiple static files. Match font-weight CSS to design intent: 300-400 for body text and lightweight UI, 500-600 for emphasis, 700-900 for display headlines. Forking brands replace this asset with their own primary face and update typography.family.* $value arrays to match.
  $license: SIL OFL 1.1
  $designer: Indian Type Foundry
  $homepage: https://www.fontshare.com/fonts/satoshi
```

```asset
font.satoshi-italic:
  $type: font-file
  $path: assets/fonts/Satoshi-VariableItalic.woff2
  $format: woff2-variations
  $description: Satoshi italic companion — variable axis weight 300-900, italic style. Separate woff2 file (not a slnt/ital axis on the upright variable) per Indian Type Foundry's release convention. Pairs with font.satoshi under the same 'Satoshi' family name; CSS resolves to this file when font-style:italic is requested.
  $applied-guidance: |
    Used implicitly when CSS calls for italic Satoshi (em, cite, em.text-secondary, etc.). Don't reference directly in markup — italic resolution happens through the cascade via font-style:italic. Shipping this file alongside font.satoshi ensures italic text renders in the canonical face rather than falling back to system italic. Forks that don't need italics can omit this file and the corresponding @font-face block in df-input.css.
  $license: SIL OFL 1.1
  $designer: Indian Type Foundry
  $homepage: https://www.fontshare.com/fonts/satoshi
```

```asset
font.general-sans:
  $type: font-file
  $path: assets/fonts/GeneralSans-Variable.woff2
  $format: woff2-variations
  $description: "Designframe canonical body face. General Sans by Indian Type Foundry — variable axis weight 200-700, upright. ITF's foundry-house mono-family sibling to Satoshi: grid-derived neutral grotesque, restrained character tuned for body-text legibility. Designed by the same team within the same modernist-restraint philosophy as Satoshi + Erode."
  $applied-guidance: |
    The body face for prose tiers. Used by typography.family.body. Variable axis (200-700) provides continuous interpolation; body default at 400 weight, emphasis (font-bold) at 600-700. Pairs with Satoshi via foundry-house mono-family pattern — same ITF design language, role-calibrated for display vs body. Forking brands can swap to another body face without disrupting the heading/display tier.
  $license: SIL OFL 1.1
  $designer: Indian Type Foundry
  $homepage: https://www.fontshare.com/fonts/general-sans
```

```asset
font.general-sans-italic:
  $type: font-file
  $path: assets/fonts/GeneralSans-VariableItalic.woff2
  $format: woff2-variations
  $description: General Sans italic companion — variable axis weight 200-700, italic style. True cursive italic (different letterforms from upright, not slant axis) per ITF convention. Shares the 'General Sans' family name with the upright file; CSS resolves to this file when font-style:italic is requested.
  $applied-guidance: |
    Used implicitly when CSS calls for italic body text (em, cite, em.text-secondary, etc.). Don't reference directly in markup — italic resolution happens through the cascade. Shipping this file ensures italic emphasis renders in canonical General Sans rather than falling back to system italic.
  $license: SIL OFL 1.1
  $designer: Indian Type Foundry
  $homepage: https://www.fontshare.com/fonts/general-sans
```

```asset
font.commit-mono:
  $type: font-file
  $path: assets/fonts/CommitMono-Variable.woff2
  $format: woff2-variations
  $description: Designframe canonical mono face. Commit Mono by Eigil Nikolajsen — variable axis weight 300-700. Geometric mono construction with intentionally ligature-free design ("Ligatures fundamentally alter the perception of code by visually merging multiple characters into one, which can make it harder to parse what's actually written" — Eigil). Used by typography.family.mono for code surfaces.
  $applied-guidance: |
    The mono face for all code-flavored text — code fences, inline <code>, token paths in design-system.html, terminal-style demos, IDE-flavored documentation. Resolves to this file via typography.family.mono → --font-mono → Tailwind utility .font-mono. The ligature-free design is deliberate — programming ligatures (=>, !=, ===) merge syntactically-distinct characters into single glyphs which can mislead readers and AI agents parsing the code. Designframe inherits this no-ligature stance as canonical. Forks wanting ligatures can swap to another mono face (JetBrains Mono, Fira Code, etc.) without changing the typography.family.mono structure.
  $license: SIL OFL 1.1
  $designer: Eigil Nikolajsen
  $homepage: https://commitmono.com
```

### Do's and Don'ts (asset-specific)

- **Do** use the wordmark as the headline brand surface — homepage, sign-in screens, marketing-page headers, OG images.
- **Do** swap to the on-dark variants when surface bg is darker than `color.theme.invert.bg` (e.g., in mode:dark or mode:key contexts) — the fill inverts from #222222 to white while geometry stays identical.
- **Don't** recolor any asset variant. The Designframe identity is intentionally monochromatic — `#222222` + white is the entire brand palette. If you fork this kit to define your own brand, replace the SVGs with your own marks; the brand-tier color tokens (`color.brand.key`, `color.brand.key-end`, etc.) are utility colors retained for DF token-surface compatibility, not the brand identity itself.
- **Don't** apply effects (drop shadow, glow, outline, emboss). Designframe's identity is the geometry — three circles in √2 ratio encoding the LANDMARK/SECTION/BLOCK cascade, the wordmark's uppercase proportions — not chrome treatments. If the asset needs visual weight, increase its size; don't decorate it.
- **Don't** use logo.favicon outside its 16-32px size band — at larger sizes the circular outline reads as decorative rather than functional. Use logo.mark for any non-icon-masked context.

## Modes

### dark

In dark mode, the default theme context redirects to the invert context. Brand primitives don't change; only the semantic-tier `$ref` targets reassign.

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

```token
color.bg.canvas:
  $ref: "{color.theme.invert.bg}"
  $mode: dark
```

```token
color.fg.default:
  $ref: "{color.theme.invert.fg}"
  $mode: dark
```

### alt

Alt-palette mode — every default-theme context redirects to its alt-palette equivalent. Brand-alt primitives carry the alternate identity. For canonical Designframe, that's the dramatic-key gradient (`#000000` → `#222222`) and a wider-range atmospheric backdrop (`#cccccc` → `#ffffff`) — both still monochrome, but visibly more contrast-driven than the flat default theme. Other brands forking Designframe carry their own alt-palette character via `color.brand-alt.*` overrides.

```token
color.theme.default.bg:
  $ref: "{color.brand-alt.background}"
  $mode: alt
```

```token
color.theme.default.fg:
  $ref: "{color.brand-alt.primary}"
  $mode: alt
```

### key

Key mode — the full page treats every default-theme section as if `.theme.key` were applied. Saturated key brand color as canvas; components auto-invert per df-rules.md §10.4 (structural — not represented at token level).

```token
color.theme.default.bg:
  $ref: "{color.theme.key.bg}"
  $mode: key
```

```token
color.theme.default.fg:
  $ref: "{color.theme.key.fg}"
  $mode: key
```

## Components — Premium

(Reserved for `df-*` premium components — see workspace `df-ui/CLAUDE.md` "Free vs Premium Boundary". Current premium components live in `df-input.css` SECTION CONFIG Custom CSS Classes. Whether they get ported to PRISM.md tokens or stay runtime-only is an open decision (TBD); the section is held as a reserved namespace so consumers know to expect either nothing or token-tier entries here in a future release.)

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
