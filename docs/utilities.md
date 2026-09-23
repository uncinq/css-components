---
isIndex: false
title: Utilities
description: The .scrollsnap utility, which turns a grid into a horizontal snap carousel below a breakpoint.
weight: 8
icon: tools
---

## `.scrollsnap`

Turns a grid into a horizontal carousel with snap points. Items keep their own width, the grid stops wrapping, and the row scrolls sideways, bleeding into the gutter so the next item peeks in.

```html
<div class="items scrollsnap-md">
  <article class="card">...</article>
  <article class="card">...</article>
  <article class="card">...</article>
</div>
```

It is opt-in, applied to the element that owns the grid, so the same component is a carousel in one context and a plain grid in another.

## The suffix names where the carousel stops

This is the one thing to get right, and it reads backwards from most responsive utilities.

| Class | Carousel below | Grid from |
| --- | --- | --- |
| `.scrollsnap` | every width | never |
| `.scrollsnap-sm` | `--sm`, 768px | 768px |
| `.scrollsnap-md` | `--md`, 1024px | 1024px |
| `.scrollsnap-lg` | `--lg`, 1440px | 1440px |
| `.scrollsnap-xl` | `--xl`, 1600px | 1600px |

**The suffix names the rung where the carousel STOPS and the grid comes back.** `.scrollsnap-md` is a carousel on mobile and tablet, and a grid from 1024px up.

## Why it is a utility

It has to win over the `grid-template-columns` that a component such as `.items` declares for itself in `@layer components`. `@layer utilities` comes later in the order, so it wins regardless of specificity. See [Cascade layers](../cascade-layers/).

## Item width

```css
grid-auto-columns: min(
  var(--items-min-width, var(--max-width-item)),
  var(--items-carousel-max-width, 75vw)
);
```

Items take `--items-min-width`, the same value that drives the grid, capped by `--items-carousel-max-width` (default `75vw`). The cap is what guarantees the next item always peeks in, which is the affordance that tells a reader the row scrolls at all. Without it, a wide item on a narrow screen would fill the viewport and the carousel would look like a static card.

## The bleed, and why margin and padding disagree

The row pulls out to the screen edge using `--container-bleed`, published by `.container` in [css-base](../../css-base/layouts/#the-container-bleed-contract). The property inherits, so it reaches the row from a `.container` however far up it sits.

Outside a container there is no published value, and the two `var()` fallbacks differ **on purpose**, because margin and padding answer different questions:

| | Question | Fallback |
| --- | --- | --- |
| `margin` | How far may I pull out of my parent? | `0`, since nothing published a bleed. The parent may be a card or a column with no room to give, and pulling a gutter out of it would overflow |
| `padding` | How far in from my own edge should the first item sit? | `var(--gutter)`, the breathing room every other box gets, so a full-bleed block does not let the cards touch the screen |

Inside a container the two collapse to one value: both read the published bleed, and the padding puts back inside the scroll box exactly what the negative margin took out. The first item lands on the container's content edge, aligned with everything else on the page.

`scroll-margin-inline` follows the **padding**, not the margin. It is what makes an item snap flush with the content edge rather than the padding edge, so it has to cancel the padding whatever the margin does.

This is the same reasoning as the `--container-bleed` contract itself, applied to a scroll container. If you write your own full-bleed component, copy the pair rather than picking one fallback for both.
