# Apples & Oranges — Quarto theme

A comparison-deck design system: two opposing sides (**apple** = dark,
**orange** = light) plus a neutral tone (**banana** = slate grey). Same
layout every time — only the palette flips. Type is **Space Grotesk**
(display/body) + **Space Mono** (eyebrows, indices, code).

## Files

| File | Use |
|------|-----|
| `apples-oranges-revealjs.scss` | reveal.js **presentation** theme |
| `apples-oranges.scss` | **HTML document** theme (Bootstrap-based) |
| `examples/slides.qmd` | sample deck (title-split, `.apple` / `.orange` / `.banana`, callouts, code) |
| `examples/document.qmd` | sample long-form HTML doc |
| `examples/_quarto.yml` | notes for wiring it into a project |

Both `.scss` files follow Quarto's Sass layering — a
`/*-- scss:defaults --*/` block (palette + Bootstrap/reveal variables)
and a `/*-- scss:rules --*/` block (component CSS).

## Palette (tokens)

| Token | Hex | Role |
|-------|-----|------|
| `$ao-ink` | `#0F1D38` | deepest navy — code cards |
| `$ao-navy` | `#16284A` | apple ground / primary ink |
| `$ao-panel-dark` | `#20355C` | callout panel on dark |
| `$ao-accent` | `#3B63B5` | links, accents |
| `$ao-soft` | `#6B8FC9` | soft blue |
| `$ao-eyebrow` | `#7FA6E0` | eyebrow labels on dark |
| `$ao-paper` | `#EDF3FC` | orange ground (light) |
| `$ao-panel-lt` | `#DEE9F8` | callout panel on light |
| `$ao-banana` | `#C3C9D2` | neutral ground (slate grey) |
| `$ao-card` | `#FEFFFE` | card surface |

## Quick start — slides

```yaml
---
title: "Apples [vs.]{.vs} Oranges"   # .vs paints the pivot soft-blue
subtitle: "A comparison deck template"
author: "Your Name"
date: "2026-06-01"
date-format: "MMMM YYYY"
title-slide-attributes:
  data-eyebrow: "Comparison Deck"    # top-left kicker on the title slide
format:
  revealjs:
    theme: apples-oranges-revealjs.scss
    width: 1920
    height: 1080
    margin: 0            # full-bleed grounds (required for the split / mirror)
    center: false        # top-aligned layout
    slide-number: "c/t"  # rendered top-right, mono
    code-line-numbers: false
    footer: "Apples & Oranges"
---
```

The **title slide** is automatically the navy/paper split — no per-slide
attribute needed. `data-eyebrow` sets the top-left kicker; `author` lands
bottom-left, `date` top-right.

Set a slide's tone by adding a class to its heading — Quarto promotes it
onto the `<section>`:

```markdown
## Why compare? {.apple}      <!-- dark navy ground, pale text -->
## Why compare? {.orange}     <!-- pale ground, navy text -->
## Neutral ground {.banana}   <!-- neutral slate ground -->
```

### Helper blocks

- `::: {.eyebrow}` — wide-tracked mono kicker; rides **above** the title.
- `::: {.points}` — auto-numbered list (`01 02 03`) with hairline rules.
  Wrap each item's headline in `[…]{.lead}` and an optional supporting
  line in `[…]{.desc}`. Add `.lg` (`{.points .lg}`) for a leads-only,
  larger variant.
- `:::: {.split}` — the standard content layout: a points column on the
  left and a `::: {.stack}` (callout + code card) on the right. Nest with
  more colons on the outer fence than the inner ones, e.g.

  ```markdown
  :::::: {.split}
  ::: {.points}
  - [Lead line]{.lead}<br>[Supporting detail.]{.desc}
  :::
  :::: {.stack}
  ::: {.callout-note title="Note"}
  A flat brand callout panel.
  :::
  ```python
  variety = "honeycrisp"
  ```
  ::::
  ::::::
  ```

- Fenced code blocks render as the dark "code card" (comments / strings
  are brand-tinted; everything else stays pale).
- `::: {.callout-note title="…"}` — flat brand panel, mono eyebrow title.
- `::: {.swatches}` — a row of labelled tone chips; mark each span
  `[apples]{.swatch .apple}` `[oranges]{.swatch .orange}`
  `[banana]{.swatch .banana}`.

### Mirror comparison slide

Add `.compare` to the heading (its text is hidden — used only for the
nav title) and lay out two `.side` halves. The slide goes full-bleed and
drops its footer / counter:

```markdown
## Strengths & trade-offs {.compare}

::::: {.versus}
:::: {.side .apple}
[Apples]{.eyebrow}

[Strengths]{.sub}

- Crisp texture
- Long shelf life

[Trade-offs]{.sub}

::: {.trade}
- Less juice per bite
:::
::::

:::: {.side .orange}
[Oranges]{.eyebrow}
…mirrored, right-aligned…
::::
:::::
```

## Quick start — HTML document

```yaml
format:
  html:
    theme: apples-oranges.scss
```

Utility blocks `.scheme-apple` / `.scheme-orange` / `.scheme-banana`
reproduce the three deck tones inside a document.

## Notes

- **Fonts** load via a Google Fonts `@import` inside each theme. To
  self-host, drop the `@import` line and set `mainfont` / `monofont` in
  YAML instead.
- **Code cards.** Both themes force a dark code card and re-tint the
  syntax tokens to the brand blues directly in CSS, so no `highlight-style`
  is required — the default tokenizer is enough.
- **Title split.** The reveal title slide is styled by its `#title-slide`
  id, so it is always the navy/paper split — no `data-state` needed. Set
  the top-left kicker with `data-eyebrow` and paint the pivot by writing
  the title as `Apples [vs.]{.vs} Oranges`.
- **Full bleed.** `margin: 0` is required so the apple / banana grounds
  and the `.compare` mirror reach the slide edges.
- **Slide counter.** Rendered top-right in mono via `slide-number: "c/t"`;
  reveal does not pad single digits, so it reads `2 / 6` rather than
  `02 / 06`.
