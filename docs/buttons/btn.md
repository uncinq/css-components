---
isIndex: false
title: Button
description: The .btn base class, its ten colour variants, five sizes and three style variants.
weight: 2
icon: hand-index-thumb
---

`.btn` is the base class. It works on `<button>`, `<a>` and `<input type="submit">`, and reads `--btn-*` from [@uncinq/component-tokens](../../../component-tokens/reference/).

```html
<button class="btn">Send</button>
<a class="btn btn-secondary" href="/contact">Contact</a>
```

Variants compose freely: `class="btn btn-danger btn-sm btn-ghost"`.

## Colour variants

| Class | Intent |
| --- | --- |
| `.btn-primary` | Primary action |
| `.btn-secondary` | Secondary action |
| `.btn-brand` | Brand-coloured |
| `.btn-neutral` | Neutral emphasis |
| `.btn-dark` | Dark surface |
| `.btn-light` | Light surface |
| `.btn-success` | Confirmation |
| `.btn-danger` | Destructive action |
| `.btn-warning` | Warning |
| `.btn-info` | Informational |

Each sets its background, border and text colour, with their hover values.

```css
.btn-danger {
  --btn-color-background: var(--color-danger);
  --btn-color-background-hover: var(--color-danger-hover);
  --btn-color-border: var(--color-danger);
  --btn-color-border-hover: var(--color-danger-hover);
  --btn-color-text: var(--color-text-on-danger);
  --btn-color-text-hover: var(--color-text-on-danger);
}
```

The text colour always comes from the matching `--color-text-on-*` semantic token, so contrast survives a change to the underlying colour. That pairing is the reason to rebrand by redefining `--color-brand` rather than `--btn-color-background`.

The hover text colour has to be set alongside the text colour, even when the two are the same. The `--btn-color-text-hover` token defaults to `var(--btn-color-text)`, but on `:root`, where it resolves to the default text colour once and for all: a variant or a context that only changes `--btn-color-text`, as a push over a dark media does, would see its text turn back to `--color-text-on-brand` on hover.

`.btn-primary` and `.btn-brand` resolve to exactly the same values, as do `.btn-secondary` and `.btn-neutral`. The intent names exist so a page can say *primary action* without deciding which colour that is.

## Sizes

| Class | Use |
| --- | --- |
| `.btn-xs` | Dense UI, inline controls |
| `.btn-sm` | Compact |
| `.btn-md` | Default, can be omitted |
| `.btn-lg` | Prominent |
| `.btn-xl` | Hero call to action |

A size class sets `--btn-font-size` along with the block and inline padding, all three from the same rung of the scale. Since `.btn` publishes `--icon-size: var(--btn-font-size)`, any icon inside scales with it.

## Style variants

| Class | Effect |
| --- | --- |
| `.btn-ghost` | Fully transparent, background and border alike, text in `--color-text` |
| `.btn-link` | Renders as an underlined link |
| `.btn-control` | Form control styling, for buttons sitting next to inputs |

`.btn-control` maps the `--btn-color-*` set onto the `--btn-control-*` tokens, which is the same palette the [close](../btn-close/) and [menu](../btn-menu/) buttons use by default. It is what makes a button read as part of a form rather than as a call to action.

### How `.btn-link` gets its underline

Every button already carries one. The base class declares `text-decoration-line: var(--btn-text-decoration-line)`, which the tokens set to `underline`, and paints it with `--btn-color-text-decoration`, which defaults to `transparent`.

`.btn-link` only changes the colour:

```css
.btn-link {
  --btn-color-text-decoration: initial;   /* revealed, takes currentColor */

  @media (hover: hover) and (pointer: fine) {
    &:hover {
      --btn-color-text-decoration: transparent;  /* hidden again */
    }
  }
}
```

The underline is therefore always in the layout and never shifts the text, which is why hovering one does not nudge the label. If you write your own link-like variant, change the decoration colour rather than the decoration line, for the same reason.

## States

`:focus-visible` draws the outline from the `--focus-*` tokens. `:disabled` and `[aria-disabled='true']` are treated alike, both getting `cursor: not-allowed` and `--opacity-disabled`.

Hover is guarded by `@media (hover: hover) and (pointer: fine)`, so a touch device never leaves a button stuck in its hover colours after a tap. The transition is guarded by `prefers-reduced-motion`.
