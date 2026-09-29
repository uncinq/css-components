---
isIndex: false
title: Drawer
description: The edge-pinned panel configuration and its four position variants.
weight: 3
icon: layout-sidebar-reverse
---

A panel sliding in from an edge. Like [`.modal`](../modal/), the class is **pure configuration**: `drawer.css` declares nothing but `--panel-*` tokens, and [`components/panel.css`](../panel/) owns every rule.

```html
<div class="drawer drawer-end" id="menu" popover="auto" role="dialog" aria-labelledby="menuLabel">
  <div class="header">
    <p class="title" id="menuLabel">Menu</p>
    <button class="btn-close js-dialog-close" aria-label="Close"></button>
  </div>
  <div class="content">
    <ul class="nav">...</ul>
  </div>
</div>
```

**A position variant is required.** `.drawer` on its own pins nothing: `--drawer-start` and its siblings have no value until one of the four variants sets them.

## Position variants

| Class | Edge | Slides from |
| --- | --- | --- |
| `.drawer-start` | Inline start, left in a left-to-right document | `translateX(-100%)` |
| `.drawer-end` | Inline end | `translateX(100%)` |
| `.drawer-top` | Top | `translateY(-100%)` |
| `.drawer-bottom` | Bottom | `translateY(100%)` |

Each sets three things: which two insets are `0`, which two are `auto`, and the translate the panel animates from.

The start and end variants leave `--panel-inline-size: var(--drawer-width)` and let the block axis stretch. The top and bottom variants swap that, taking `--panel-block-size: var(--drawer-height)` and `--panel-inline-size: auto`.

## Why the margin stays at 0

```css
/* two edges pinned, the opposite ones auto */
--panel-inset-block:  var(--drawer-top)   var(--drawer-bottom);
--panel-inset-inline: var(--drawer-start) var(--drawer-end);
```

A `<dialog>` opened with `showModal()` is centred by user agent rules that only apply when the margins are `auto`. The drawer leaves `--panel-margin` at its `0` default precisely so those rules do not fire, which is what lets the same file work on a `<dialog>` and on a popover.

## It only slides, it does not fade

Unlike a modal, the drawer sets no `--panel-opacity`, so the token keeps its `1` default. The closed state is a pure translate, and the `@starting-style` block in `panel.css` replays it. That is the whole difference in the two animations.

## The case for `popover` over `<dialog>`

A drawer is the usual host for [`.panel-inline-*`](../panel/#inline-variants): a menu, a filters sidebar or a table of contents that is an overlay on mobile and a column on desktop. A `<dialog>` is exposed as `role="dialog"` even while it is styled as a column, so use `popover="auto"` and let your script set the role only while the panel is actually open.

## Sizing

`--drawer-width` and `--drawer-height` are the two dimensions. `--panel-max-inline-size` is pinned to `100%`, so a drawer wider than the viewport is clamped rather than pushed off screen.
