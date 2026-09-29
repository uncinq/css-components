---
isIndex: false
title: Surtitle
description: An eyebrow label above a heading, tightened automatically when it sits inside a card.
weight: 8
icon: type-h2
---

An eyebrow label above a heading.

```html
<p class="surtitle">Category</p>
<h2>Main title</h2>
```

It is a leaf component: no children, no variants, no states. Every property reads a `--surtitle-*` token, including the border set, so it can be a plain label in one theme and a chip in another without a second class.

| Token group | Covers |
| --- | --- |
| `--surtitle-color-*` | Background and text |
| `--surtitle-border-*` | Colour, style, width and radius |
| `--surtitle-font-size`, `-font-weight`, `-letter-spacing`, `-line-height`, `-text-transform` | Typography |
| `--surtitle-padding-*`, `-margin-block-*` | Spacing |

## It pulls itself closer inside a card

A [card](../card/) reassigns one token:

```css
.card { --surtitle-margin-block-end: calc(var(--card-gap) * -1); }
```

The negative margin cancels exactly one grid gap, so the surtitle and the title read as a single unit rather than as two stacked lines. The card also zeroes `margin-block-start` on it, so the pair sits flush against the top of the content block.

That is the whole mechanism, and it is worth copying: a context that stacks its children with a `gap` can tighten any pair by handing back one negative gap, with no change to the component.

## Use a paragraph, not a heading

A surtitle is a label, not a level in the document outline. Marking it up as `<h3>` above an `<h2>` inverts the heading order and breaks navigation by headings. If the category needs to be part of the heading, put it inside one:

```html
<h2><span class="surtitle">Category</span> Main title</h2>
```
