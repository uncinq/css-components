---
isIndex: false
title: Modal
description: The centred panel configuration, and how it centres and shrinks to its content at once.
weight: 2
icon: window
---

A centred panel. The class is **pure configuration**: `modal.css` declares nothing but `--panel-*` tokens, and [`components/panel.css`](../panel/) owns every rule.

```html
<dialog class="modal" id="contact" aria-labelledby="contactLabel">
  <div class="header">
    <p class="title" id="contactLabel">Contact</p>
    <button class="btn-close js-dialog-close" aria-label="Close"></button>
  </div>
  <div class="content">...</div>
  <div class="footer">
    <button class="btn btn-secondary js-dialog-close">Cancel</button>
    <button class="btn" type="submit">Send</button>
  </div>
</dialog>
```

Works on a `<dialog>` opened with `showModal()` and on any element carrying `popover="auto"`, opened with `showPopover()`. Reach for the second form only when the panel is not always modal, typically when [`.panel-inline-*`](../panel/#inline-variants) turns it into a region of the page above a breakpoint.

## Sizing

Override `--modal-max-width` on the element:

```html
<dialog class="modal" style="--modal-max-width: 40rem">...</dialog>
```

or globally, for every modal on the site:

```css
@layer components {
  .modal { --modal-max-width: 40rem; }
}
```

`--modal-max-height`, `--modal-min-height` and `--modal-margin` are the other three sizing tokens. All `--modal-*` names are the public API; `--panel-*` is the internal one.

## Why the height is `fit-content` and not `auto`

The centring is done with insets and auto margins:

```css
--panel-block-size: fit-content;
--panel-inset-block: 0;
--panel-inset-inline: 0;
--panel-margin: auto;
```

With both block insets set, an `auto` height resolves to fill the gap between them, and the auto margins then collapse to `0`. The panel would neither centre nor shrink to its content. `fit-content` is what keeps both behaviours, and it is the same reason the user agent dialog stylesheet uses it.

## Why there is no mobile breakpoint

The maximum is a `min()` rather than a media query:

```css
--panel-max-block-size: min(var(--modal-max-height), 100% - var(--modal-margin) * 2);
--panel-max-inline-size: min(var(--modal-max-width),  100% - var(--modal-margin) * 2);
```

So the panel always keeps `--modal-margin` clear of the viewport edges, whatever the maximum is set to. Raising `--modal-max-width` on a narrow screen changes nothing, which is why a modal needs no responsive variant.

## Opening animation

`--panel-opacity: 0` and `--panel-translate: var(--modal-translate)` describe the **closed** state, so a modal both fades and moves on the way in. A drawer leaves the opacity alone and only slides. Both are replayed by the `@starting-style` block in `panel.css`, and both are skipped under `prefers-reduced-motion: reduce`.

## Accessibility

Point `aria-labelledby` at the `id` of the title in the header. A `<dialog>` opened with `showModal()` gets the focus trap, Escape and inertness from the browser; the `popover` path gets light dismiss and Escape but no role, which your script has to supply.
