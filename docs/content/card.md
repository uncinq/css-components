---
isIndex: false
title: Card
description: A self-contained content card, the tokens it hands down to its media, and the whole-card link pattern.
weight: 5
icon: card-heading
---

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

| Child | Role |
| --- | --- |
| `.content` | The text block, grows to fill the card |
| `.title` | The heading line, styled from `--card-title-*` |
| `.description` | The body text, at `--card-font-size` |
| `.media` | The image or video, see [Media](../media/) |
| `.surtitle` | An optional eyebrow, see [Surtitle](../surtitle/) |

{{< alert-block state="info" >}}
The source comment in `card.css` describes `.card` as a backwards-compatible alias for `.item` and recommends using `.item` directly. That is misleading for anyone consuming this package on its own: **`.item` is not defined here**. It lives in the Hugolify design system theme. Within `@uncinq/css-components`, `.card` is a complete implementation and the class to use.
{{< /alert-block >}}

## Put the media last in the markup

The media block sits first **visually**, through `--card-media-order: -1`, regardless of where it appears in the source. Put it after the content so that screen readers and search engines reach the text first.

Set `--card-media-order` to anything else, or to `initial`, to put the image below the text on a given card.

## What the card hands down

Three reassignments at the top of the rule:

```css
--media-color-background: var(--card-media-color-background);
--media-ratio: var(--card-media-ratio);
--surtitle-margin-block-end: calc(var(--card-gap) * -1);
```

A [`.media`](../media/) inside a card therefore takes the card's ratio and background without any variant class, and a [`.surtitle`](../surtitle/) is pulled back by exactly one gap so that it reads as part of the title rather than as a separate line.

`.media-icon` gets a longer handover of its own, mapping eight `--card-media-icon-*` tokens onto the media namespace, which is what lets an icon card look nothing like a photo card while using the same two classes.

## The whole-card link

Wrapping the content in `a.content` makes the entire card clickable:

```html
<article class="card">
  <a class="content" href="/article">
    <p class="title">Card title</p>
    <p class="description">Short description.</p>
  </a>
  <div class="media">...</div>
</article>
```

The link lays a `::before` overlay across the card, and its own focus ring is suppressed so the focus outline can be drawn on the card instead:

```css
a.content::before { content: ''; inset: 0; position: absolute; }
a.content:focus-visible { outline: none; }

.card:has(a.content:focus-visible) {
  outline: var(--focus-outline-width) var(--focus-outline-style) var(--focus-color-outline);
}
```

Without the `:has()` rule the ring would outline an invisible box, which is the usual failure of this pattern.

Two things to know before using it. **Nested interactive elements become unreachable**, since the overlay covers them: a card with both a whole-card link and a button inside needs a different structure. And the overlay resolves against the nearest positioned ancestor, so the card must establish one; give it `position: relative` if your layout does not already.

## Sizing

`--card-max-width` and `--card-width` are unset by default, so a card fills its grid track. Prefer `ch` units when you do set them, so the width tracks the text measure rather than a fixed pixel count.

`overflow: hidden` on the card is what lets `--card-border-radius` clip a full-bleed media block at the corners.

## States

Hover swaps the shadow, `--card-shadow` taking `--card-shadow-hover`, behind `@media (hover: hover) and (pointer: fine)`. The transition is behind `prefers-reduced-motion`. There is no hover colour change, deliberately: a card that changes background on hover competes with the link inside it.
