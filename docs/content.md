---
isIndex: false
title: Content
description: Alerts, badges, banners, cards, the items grid, labelled lists, media objects and surtitles.
weight: 4
---

## `.alert`

An inline notification block.

```html
<div class="alert alert-info">
  <p class="alert-heading">Heads up</p>
  <p>Your changes were saved.</p>
</div>
```

Variants: `.alert-primary`, `.alert-secondary`, `.alert-brand`, `.alert-neutral`, `.alert-dark`, `.alert-light`, `.alert-success`, `.alert-danger`, `.alert-warning`, `.alert-info`.

Each sets the background, border and text colour from the matching `-muted` and `-strong` semantic tokens, so the pairing stays legible if the underlying colour changes.

Two behaviours are worth knowing. `--color-link` is reassigned to the alert's own text colour, so links inside an alert do not fall back to the page link colour against a tinted background. And an alert whose first child is an `.icon`, with no `.alert-heading`, switches to a horizontal layout automatically.

`.container` inside an alert inherits the same flex layout, which is what lets a full-bleed alert keep its content aligned with the rest of the page.

## `.banner`

A thin variation on the alert, centred rather than start-aligned. It sets `--alert-align: center` and flattens paragraph margins, and nothing else.

```html
<div class="alert banner">
  <p>Site-wide announcement.</p>
</div>
```

It is a modifier on `.alert`, so use both classes.

## `.badge`

A small inline label.

```html
<span class="badge badge-success">Published</span>
<a class="badge" href="/tag/css">css</a>
```

Variants mirror the alert set: `.badge-primary`, `.badge-secondary`, `.badge-brand`, `.badge-neutral`, `.badge-dark`, `.badge-light`, `.badge-success`, `.badge-danger`, `.badge-warning`, `.badge-info`.

Hover styles apply only to `a.badge`, guarded by `@media (hover: hover) and (pointer: fine)`, so a static badge never looks interactive.

## `.card`

A self-contained content card.

```html
<article class="card">
  <div class="content">
    <p class="surtitle">Category</p>
    <p class="title">Card title</p>
    <p class="description">Short description.</p>
  </div>
  <div class="media">
    <picture>...</picture>
  </div>
</article>
```

The media block sits first visually, through `--card-media-order: -1`, regardless of its position in the markup. Put it after the content so that screen readers and search engines reach the text first.

Wrapping the content in `a.content` makes the whole card clickable, using a `::before` overlay that covers the card. Focus is then handled on the card itself with `:has(a.content:focus-visible)`, so the focus ring outlines the card rather than an invisible box. Nested interactive elements inside such a card will be unreachable, which is the known trade-off of this pattern.

Sizing with `--card-max-width` and `--card-width` is unset by default. Prefer `ch` units, so the width tracks the text measure.

{{< alert-block state="info" >}}
The source comment in `card.css` describes `.card` as a backwards-compatible alias for `.item` and recommends using `.item` directly. That is misleading for anyone consuming this package on its own: **`.item` is not defined here**. It lives in the Hugolify design system theme. Within `@uncinq/css-components`, `.card` is a complete implementation and the class to use.
{{< /alert-block >}}

## `.items`

A responsive grid, and the usual container for cards.

```html
<div class="items">
  <article class="card">...</article>
  <article class="card">...</article>
</div>
```

The column count is intrinsic rather than breakpoint-driven:

```css
grid-template-columns: repeat(
  var(--items-cols, auto-fill),
  minmax(min(var(--items-min-width, var(--max-width-item)), 100%), 1fr)
);
```

Items wrap when they no longer fit at `--items-min-width`, with no media query involved. Set `--items-cols` to a number to force a fixed count instead.

It also declares `container-type: inline-size` and `container-name: items`, so children can use container queries against the grid rather than the viewport.

Pair it with [`.scrollsnap`](../utilities/) to turn the grid into a horizontal carousel below a given breakpoint.

## `.list`

A compact labelled list with dash markers.

```html
<div class="list">
  <p>Optional label</p>
  <ul>
    <li>First</li>
    <li>Second</li>
  </ul>
</div>
```

The wrapper is a `div`, with the label as a sibling of the list rather than inside it. Either `ul` or `ol` works.

## `.media`

The structural base for media blocks, controlling aspect ratio, background and border.

| Class | Use |
| --- | --- |
| `.media` | Base, for a picture or video |
| `.media-logo` | Logo, typically contained rather than cropped |
| `.media-icon` | Icon, sized from `--icon-size` |

`.media-logo` and `.media-icon` build on `.media`. Inside a `.card`, the card reassigns `--media-color-background` and `--media-ratio` from its own `--card-media-*` tokens, so a media block adapts to its context without a variant class.

A `.video` inside a `.media` is stretched to fill it. See [Embeds](../embeds/).

## `.surtitle`

An eyebrow label above a heading.

```html
<p class="surtitle">Category</p>
<h2>Main title</h2>
```

Inside a `.card` it gets a negative `--surtitle-margin-block-end`, pulling it closer to the title so the pair reads as one unit.
