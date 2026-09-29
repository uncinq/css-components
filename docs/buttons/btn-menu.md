---
isIndex: false
title: Menu button
description: The self-contained menu toggle, typically the burger in a site header.
weight: 3
icon: list
---

A menu toggle, typically the burger control in a header. Like [`.btn-close`](../btn-close/), it is **self-contained**: `.btn` is not required.

```html
<button class="btn-menu js-dialog-toggle" data-target="#menu" aria-label="Menu"></button>
```

It is almost always the trigger for a [drawer](../../overlays/drawer/), which is why it usually carries `.js-dialog-toggle` and a `data-target` pointing at the panel's `id`.

## Same construction as the close button

Every property falls back from `--btn-menu-*` to the matching `--btn-*` token, and the default palette is `--btn-control-*`. See [`.btn-close`](../btn-close/#why-it-stands-alone) for the full explanation, which applies here unchanged.

The icon comes from `--icon-menu`, masked on `::before` and sized by `--btn-menu-icon-size` falling back to `--icon-size`.

## What it does not declare

No `:focus-visible` rule and no disabled styling, exactly as with `.btn-close`. Add `.btn` alongside it when you need either.

## Hiding it once the menu is inline

A burger that opens a panel which has become a static region of the page is a button that does nothing. Pair the panel's `.panel-inline-*` class with the matching `.panel-trigger-*` on this button:

```html
<button class="btn-menu panel-trigger-md js-dialog-toggle" data-target="#menu" aria-label="Menu"></button>
```

See [Hiding the trigger](../../overlays/panel/#hiding-the-trigger).

## Accessibility

`aria-label` is required, since the button has no text. Keep `aria-expanded` in sync if your script toggles a menu in place rather than opening a panel in the top layer.
