---
isIndex: false
title: Overlays
description: The shared panel skeleton behind .modal and .drawer, the two opening strategies, and the .dropdown contract.
weight: 3
icon: window-stack
---

## The shared panel skeleton

`.modal` and `.drawer` are the same panel. Same structure (header, content, footer), same open and closed contract, same backdrop. They differ only in where they sit: modal centres them, drawer sticks them to an edge.

`components/panel.css` owns every rule. The two variants are **pure configuration**: `modal.css` and `drawer.css` declare nothing but `--panel-*` tokens.

There is **no `.panel` class to apply.** The file targets `:where(.modal, .drawer)` directly, so the skeleton arrives with either variant class. `panel` names the shared concept and the token namespace, not a class.

```html
<dialog class="modal" id="contact" aria-labelledby="contactLabel">
  <div class="header">
    <p class="title" id="contactLabel">Contact</p>
    <button class="btn-close js-dialog-close" aria-label="Close"></button>
  </div>
  <div class="content">...</div>
  <div class="footer">...</div>
</dialog>
```

Each variant aliases `--panel-*` from its own namespace, so `--modal-*` and `--drawer-*` remain the public API. Override those, not `--panel-*`.

## Two ways to open a panel

**The tag picks the strategy, not the class.** Either variant works both ways.

### `<dialog>` with `showModal()`

The browser handles Escape, the focus trap and making the rest of the page inert. You get `[open]`, the native `::backdrop` and the top layer for free.

This is the right choice when the panel is **always** a dialog.

### `popover="auto"` with `showPopover()`

Same top layer, same native `::backdrop`, but the element carries no implicit role.

Use this when the panel becomes a static region of the page above a breakpoint, which is what the `.panel-inline-*` variants do. The reason is an accessibility one: a `<dialog>` is exposed as `role="dialog"` even when it is only styled as a sidebar, whereas a script can set that role solely while the panel is actually open.

## `.modal`

A centred panel. Size a single instance by overriding `--modal-max-width` on the element itself.

```html
<dialog class="modal" style="--modal-max-width: 40rem">...</dialog>
```

## `.drawer`

A panel sliding in from an edge.

| Class | Edge |
| --- | --- |
| `.drawer-start` | Inline start, left in a left-to-right document |
| `.drawer-end` | Inline end |
| `.drawer-top` | Top |
| `.drawer-bottom` | Bottom |

## `.panel-inline-*`

Turns a panel into an ordinary region of the page from a breakpoint upwards, while it stays an overlay below.

| Class | Becomes inline at |
| --- | --- |
| `.panel-inline-sm` | `--sm`, 768px |
| `.panel-inline-md` | `--md`, 1024px |
| `.panel-inline-lg` | `--lg`, 1440px |
| `.panel-inline-xl` | `--xl`, 1600px |

This is the case that calls for `popover="auto"` rather than `<dialog>`. A filters sidebar that is a drawer on mobile and a column on desktop should not announce itself as a dialog while it is a column.

These variants beat `.modal` and `.drawer` on **source order**, not specificity, which is why `panel.css` is imported last. See [Cascade layers](../cascade-layers/).

## `.panel-trigger-*`

The companion to `.panel-inline-*`, applied to the **trigger button** rather than to the panel. It hides the trigger at the breakpoint where the panel stops being an overlay.

| Class | Trigger hidden from |
| --- | --- |
| `.panel-trigger-sm` | `--sm`, 768px |
| `.panel-trigger-md` | `--md`, 1024px |
| `.panel-trigger-lg` | `--lg`, 1440px |
| `.panel-trigger-xl` | `--xl`, 1600px |

Pair the suffixes: a panel carrying `.panel-inline-md` needs its button carrying `.panel-trigger-md`, otherwise a button that opens nothing remains on screen at desktop width.

The reason this is a second class rather than something CSS derives on its own is worth knowing, because it looks like a redundancy. Nothing structurally connects a trigger to its panel: the only link is `data-target` matching an `id`, and no selector can compare two attribute values. A positional rule would work only until somebody moved the markup, and a filter button typically lives in the page heading rather than beside the panel it opens. In practice both classes come from the same configuration value, so the pair cannot disagree.

## The JavaScript contract

This package ships no JavaScript. `.modal` and `.drawer` expect a script that provides:

| Hook | Role |
| --- | --- |
| `.js-dialog-toggle[data-target="#id"]` | Opens the panel it points at |
| `.js-dialog-close` | Any element inside the panel that closes it |

Closing must also work on Escape and on a click outside. With `<dialog>` and `showModal()` the browser already does both. With `popover="auto"` the platform handles light dismiss and Escape too, so in practice the script mainly manages the role and the open call.

## `.dropdown`

A contextual menu, with a tighter JavaScript contract because it animates.

```html
<div class="dropdown">
  <button class="js-dropdown-toggle" aria-expanded="false">Label</button>
  <ul class="dropdown-menu" role="menu">
    <li><a class="dropdown-item" href="#">Item</a></li>
    <li><hr class="dropdown-divider"></li>
    <li><a class="dropdown-item" href="#">Item</a></li>
  </ul>
</div>
```

The trigger is `.js-dropdown-toggle`, and its **next element sibling must be** `.dropdown-menu`.

The script toggles three classes on the menu, in this sequence:

| Class | Meaning |
| --- | --- |
| `.showing` | Opening transition has started |
| `.show` | Fully open |
| `.hiding` | Closing transition has started |

It must also keep `aria-expanded` on the toggle in sync. The three-class sequence exists so the menu can animate out before being removed from the flow; a single `.show` class would make the closing transition impossible.

`.dropdown-menu-end` aligns the menu to the inline end of its trigger rather than the start.
