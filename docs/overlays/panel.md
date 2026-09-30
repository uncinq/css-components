---
isIndex: false
title: Panel
description: The shared skeleton behind .modal and .drawer, the two opening strategies, the inline variants and the backdrop.
weight: 2
icon: layout-sidebar
---

`components/panel.css` is the largest file in the package and the only one with no class of its own. It owns every rule behind [`.modal`](../modal/) and [`.drawer`](../drawer/), which contribute nothing but `--panel-*` values.

There is **no `.panel` class.** The selectors read `:where(.modal, .drawer)`, so the skeleton arrives with either variant class.

## Structure

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

All three sections are optional, and all three are matched as **direct children** (`& > .header`).

| Section | Behaviour |
| --- | --- |
| `.header` | Fixed height, title and close button pushed apart, bottom border |
| `.content` | Takes the remaining height and scrolls, with `overscroll-behavior: contain` |
| `.footer` | Fixed height, top border, stacked on mobile and right-aligned from `--sm` up |

The panel itself carries **no padding**. It lives on the three sections instead, so that a click on the panel element is always a click on the backdrop rather than on a transparent margin.

## Two ways to open a panel

**The tag picks the strategy, not the class.** Either variant works both ways.

### `<dialog>` with `showModal()`

The browser handles Escape, the focus trap and making the rest of the page inert. You get `[open]`, the native `::backdrop` and the top layer for free.

This is the right choice when the panel is **always** a dialog.

### `popover="auto"` with `showPopover()`

Same top layer, same native `::backdrop`, but the element carries no implicit role.

Use this when the panel becomes a static region of the page above a breakpoint, which is what the [inline variants](#inline-variants) do. The reason is an accessibility one: a `<dialog>` is exposed as `role="dialog"` even when it is only styled as a sidebar, whereas a script can set that role solely while the panel is actually open.

## How open and closed are expressed

`display` is only ever declared on a state selector, never on the panel itself:

```css
:where(.modal, .drawer):not(:where([open], :popover-open)) { display: none; }
:where(.modal, .drawer):is([open], :popover-open)         { display: flex; opacity: 1; transform: none; }
```

Declaring `display` on the panel would beat the user agent rule that hides a closed `<dialog>`, and a closed dialog would render.

The two selectors differ deliberately. The closed rule uses `:where()` throughout and carries **zero specificity**. The open rule uses `:is()` on the state, giving it 0,1,0, which is what lets it beat the unstyled base rule that comes later in the file on specificity rather than on source order, while still leaving a host component such as `.toc .drawer` (0,2,0) room to override without escalating.

Everything else about the closed state lives in tokens: `--panel-opacity` and `--panel-translate` are the values the panel animates **from**, which is why a drawer only slides while a modal also fades.

The entry transition needs those values spelled out a second time, because opening is a discrete `display` change:

```css
@starting-style {
  :where(.modal, .drawer):is([open], :popover-open) {
    opacity: var(--panel-opacity, 1);
    transform: var(--panel-translate, none);
  }
}
```

`transition-behavior: allow-discrete` keeps the panel displayed while it transitions out, and keeps a `<dialog>` in the top layer until the transition ends.

## The backdrop

Both opening paths put the panel in the top layer, so both get the native `::backdrop`, which inherits from the panel and therefore reads the same `--panel-*` tokens. **No backdrop node is ever injected, and no `z-index` is involved.**

This is the reason the class-driven path uses `popover` rather than a plain element. A backdrop placed outside the panel sits in the root stacking context while the panel stays confined to its nearest stacking ancestor, a sticky sidebar wrapper or a sticky header, so the veil paints over the panel and no `z-index` can lift it out. Placing the backdrop next to the panel only moves the problem: it then inherits the same confinement, and anything painted later shows through it. The top layer removes the question entirely, which is exactly what it exists for.

Page scroll is locked from the panel's own state, with no class on `<body>` to manage:

```css
body:has(:where(.modal, .drawer):where([open], :popover-open)) { overflow: hidden; }
```

## Inline variants

`.panel-inline-*` turns a panel into an ordinary region of the page from a breakpoint upwards, while it stays an overlay below.

| Class | Becomes inline at |
| --- | --- |
| `.panel-inline-sm` | `--sm`, 768px |
| `.panel-inline-md` | `--md`, 1024px |
| `.panel-inline-lg` | `--lg`, 1440px |
| `.panel-inline-xl` | `--xl`, 1600px |

This is the case that calls for `popover="auto"` rather than `<dialog>`. A filters sidebar that is a drawer on mobile and a column on desktop should not announce itself as a dialog while it is a column. **The panel must not be a `<dialog>` when one of these is used.**

Above the breakpoint the variant neutralises the whole panel rule: position, insets, size, shadow, overflow, transition, and it hides `.header` and `.footer` while flattening `.content`.

### Colours go too, not just geometry

A region of the page carries none of the panel's chrome, so the variant hands five custom properties back to the parent:

```css
--color-link: inherit;
--color-link-active: inherit;
--color-link-hover: inherit;
--nav-color-text: inherit;
--nav-color-text-active: inherit;
```

`inherit` on a custom property is what hands it back, and it has to be done token by token. Resetting the `--panel-color-text-*` tokens they derive from would make them invalid at computed-value time rather than page-valued, and every link would fall back to the surrounding text colour instead of the site's link colour.

If you add a property to the panel rule, neutralise it in all four variants. That is the maintenance cost of the pattern, and the reason the block is written once per breakpoint rather than derived.

## Hiding the trigger

`.panel-trigger-*` is the companion, applied to the **trigger button** rather than to the panel. It hides the trigger at the breakpoint where the panel stops being an overlay.

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

One case CSS cannot cover: a panel carrying a `.panel-inline-*` class that is **open** when the viewport crosses the breakpoint. The script has to close it, because nothing in CSS releases the backdrop, the scroll lock and the inert background.

## Source order, not specificity

Most of this file is wrapped in `:where()` and carries no specificity at all, which is what lets `.modal`, `.drawer` and your own overrides win without escalating:

```css
.modal { border-radius: 0; }   /* enough, no !important needed */
```

The `.panel-inline-*` variants are the exception. They are plain class selectors that must beat `.modal` and `.drawer`, and they do so on **source order**. Alphabetical import order already places `panel.css` after `drawer.css` and `modal.css`; keep it that way if you import files individually. See [Cascade layers](../../cascade-layers/).
