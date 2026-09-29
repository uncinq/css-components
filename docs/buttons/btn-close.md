---
isIndex: false
title: Close button
description: The self-contained close control used inside panels, alerts and dismissible blocks.
weight: 2
icon: x-lg
---

A close control. It is **self-contained**: `.btn` is not required.

```html
<button class="btn-close" aria-label="Close"></button>
```

Its main use is the header of a [panel](../../overlays/panel/), where it usually carries the `.js-dialog-close` hook as well:

```html
<div class="header">
  <p class="title" id="contactLabel">Contact</p>
  <button class="btn-close js-dialog-close" aria-label="Close"></button>
</div>
```

## Why it stands alone

Every property is declared twice over, with `--btn-close-*` falling back to the matching `--btn-*` token:

```css
background-color: var(--btn-close-color-background, var(--btn-control-color-background));
border-radius: var(--btn-close-border-radius, var(--btn-border-radius));
font-size: var(--btn-close-font-size, var(--btn-font-size));
```

So it looks like the rest of the button family without inheriting from it, and a single `--btn-close-*` override retargets it alone. Its default palette is `--btn-control-*`, the same quiet colours as [`.btn-control`](../btn/#style-variants), which is what keeps it from competing with the panel's real actions.

## The icon

Drawn on `::before` as a mask from `--icon-close`, sized by `--btn-close-icon-size` falling back to `--icon-size`. Since nothing above a bare `.btn-close` publishes `--icon-size`, set one explicitly if the ambient value is not what you want:

```css
.btn-close {
  --btn-close-icon-size: var(--font-size-lg);
}
```

## What it does not declare

No `:focus-visible` rule and no disabled styling. The focus ring is therefore the browser's own rather than the one from the `--focus-*` tokens, and `disabled` changes nothing visually.

Add `.btn` alongside it when either matters:

```html
<button class="btn btn-close" aria-label="Close"></button>
```

## Accessibility

The button has no text content, so `aria-label` is not optional. A close button with no accessible name is announced as "button" and nothing more.
