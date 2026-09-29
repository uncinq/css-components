---
isIndex: false
title: Badge
description: A small inline label, with ten solid colour variants and hover styles reserved for links.
weight: 2
icon: tag
---

A small inline label.

```html
<span class="badge badge-success">Published</span>
<a class="badge" href="/tag/css">css</a>
```

It is `inline-flex`, so it sits in a line of text and can hold an icon beside its label.

## Variants

The names mirror the [alert](../alert/) set: `.badge-primary`, `.badge-secondary`, `.badge-brand`, `.badge-neutral`, `.badge-dark`, `.badge-light`, `.badge-success`, `.badge-danger`, `.badge-warning`, `.badge-info`.

The colours do not. A badge is a **solid** chip, not a tinted block:

```css
.badge-success {
  --badge-color-background: var(--color-success);
  --badge-color-background-hover: var(--color-success-strong);
  --badge-color-text: var(--color-text-on-success);
}
```

Full colour for the background, and the matching `--color-text-on-*` token for the text, which is the pair that guarantees contrast when the underlying colour is overridden. Compare with `.alert-*`, which uses `-muted` and `-strong` because it has to remain readable behind a paragraph.

Note that a variant sets no border colour. `--badge-color-border` keeps whatever the base token resolves to, so a bordered badge needs that token set explicitly.

## Hover is reserved for links

```css
@media (hover: hover) and (pointer: fine) {
  a.badge:hover { ... }
}
```

Two guards, doing two different jobs. The element selector means a static `<span class="badge">` never looks interactive, which matters because badges are used for both states and actions. The media query means a tap on a touch device does not leave the badge stuck in its hover colour.

Both hover values fall back to the resting one, so a variant that defines no hover colour simply does not change on hover rather than resolving to nothing.

## Text decoration

The base rule sets `text-decoration: none`, so `a.badge` never carries the page's link underline. That is the one thing the class overrides on a link; everything else it leaves to the variant.
