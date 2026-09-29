---
isIndex: false
title: Items
description: A responsive grid whose column count is intrinsic rather than breakpoint-driven.
weight: 5
icon: grid-3x2-gap
---

A responsive grid, and the usual container for [cards](../card/).

```html
<div class="items">
  <article class="card">...</article>
  <article class="card">...</article>
  <article class="card">...</article>
</div>
```

The children are yours to provide. Within this package that means `.card`; `.item` is defined by the Hugolify design system theme, not here.

## The column count is intrinsic

```css
grid-template-columns: repeat(
  var(--items-cols, auto-fill),
  minmax(min(var(--items-min-width, var(--max-width-item)), 100%), 1fr)
);
```

Items wrap once they no longer fit at `--items-min-width`, **with no media query involved**. The grid answers to the width of its container rather than to the width of the viewport, so the same markup works in a page, in a sidebar and inside another grid.

The inner `min(..., 100%)` is what keeps a single item from overflowing a container narrower than `--items-min-width`. Without it, a 30ch minimum in a 20ch column would blow out the layout.

Two ways to change the result:

| Token | Effect |
| --- | --- |
| `--items-min-width` | The width below which items wrap. Defaults to `--max-width-item` |
| `--items-cols` | A fixed column count, replacing `auto-fill` |

```css
.related { --items-cols: 2; }
```

## Gaps

`--items-gap` sets both axes. `--items-column-gap` and `--items-row-gap` override one at a time, each falling back to `--items-gap`.

## It is a container query context

```css
container-type: inline-size;
container-name: items;
```

So children can query the grid rather than the viewport:

```css
@container items (min-width: 60ch) {
  .card { --card-media-order: initial; }
}
```

That is the companion to the intrinsic columns: the grid decides how many tracks fit, and the children adapt to the space they actually got.

Be aware that `container-type: inline-size` makes the grid its own containment context, which affects how absolutely positioned descendants resolve.

## Turning it into a carousel

Add a [`.scrollsnap-*`](../../utilities/scrollsnap/) class to the same element. The utility replaces the grid template with a single scrolling row below its breakpoint and hands everything back above it.

```html
<div class="items scrollsnap-md">...</div>
```

The carousel reads the same `--items-min-width`, so item width stays consistent between the two modes.
