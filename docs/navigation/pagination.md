---
isIndex: false
title: Pagination
description: Page navigation, with masked control glyphs and a built-in responsive collapse.
weight: 5
icon: three-dots
---

A page navigation list.

```html
<nav aria-label="Pagination">
  <ul class="pagination">
    <li class="disabled"><a class="first">First</a></li>
    <li><a class="previous" href="?page=1">Previous</a></li>
    <li class="item active"><a href="?page=2">2</a></li>
    <li class="item item-adjacent"><a href="?page=3">3</a></li>
    <li><a class="next" href="?page=3">Next</a></li>
    <li><a class="last" href="?page=9">Last</a></li>
  </ul>
</nav>
```

| Class | On | Role |
| --- | --- | --- |
| `.first`, `.previous`, `.next`, `.last` | the `a` | Navigation controls |
| `.item` | the `li` | A page number |
| `.item-adjacent` | the `li` | A page number next to the current one |
| `.active` | the `li` | The current page |
| `.disabled` | the `li` | An unavailable control |

`.active` and `.disabled` are both read as ancestors of the link (`.active a`, `.disabled a`), so putting either on the `li` is what the rules expect.

## The glyphs are shipped

Each control draws a mask on `::after` from its own token:

```css
.next::after { mask: var(--pagination-icon-next) center / var(--pagination-icon-size) no-repeat; }
```

Four tokens, `--pagination-icon-first`, `-previous`, `-next` and `-last`, sized by `--pagination-icon-size`. The two backwards controls reuse their forwards glyph and rotate it:

```css
.first::after    { mask: var(--pagination-icon-first) ...;    rotate: 180deg; }
.previous::after { mask: var(--pagination-icon-previous) ...; rotate: 180deg; }
```

So a themed arrow only has to exist in one direction. Because the glyph is a mask over `currentColor`, it follows the link colour through hover, active and disabled without a second declaration.

The text inside the control (`First`, `Previous`) sits beside the glyph. Replace it with a `.visually-hidden` span if the design wants icons alone; do not remove it, or the control loses its accessible name.

## The responsive collapse is built in

```css
@media not (--sm) {
  .first, .last,
  li:has(.first), li:has(.last),
  .item:not(.item-adjacent):not(.active) { display: none; }
}
```

Below 768px the paginator drops to *previous, the current page, its neighbours, next*. You do not need to write that rule, and you do not need a second markup path for mobile: render the full paginator and let CSS thin it out.

This is what `.item-adjacent` is for. It is the only signal the CSS has about which page numbers are near the current one, so a paginator that omits it collapses to the current page alone on a narrow screen.

The `li:has(...)` selectors are there so the list item goes with the control it contains, rather than leaving an empty `li` contributing a gap.

## States

`.active a` and `.disabled a` both get `pointer-events: none`, so neither is clickable regardless of whether you render an `href`. The active page takes the `-active` colour pair, and a disabled control takes `--pagination-disabled-opacity`.

Note that the hover rule here is **not** behind `@media (hover: hover) and (pointer: fine)`, unlike the rest of the package. On a touch device a tapped page number can keep its hover background until the next paint.

## Accessibility

The `<nav>` and its `aria-label` are yours to write; the component styles the list only. Mark the current page with `aria-current="page"` on the link as well as `.active` on the item, so the state is announced and not merely coloured.
