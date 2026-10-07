---
isIndex: false
title: Push
description: A video or image with text laid over it, how the push places its text box, the position and darken modifiers, and the card variant.
weight: 9
icon: image
---

A video or an image with a surtitle, a title, some text and a call to action laid over it.

```html
<div class="push push-darken">
  <div class="content">
    <p class="surtitle">Collection</p>
    <h2 class="title">Push title</h2>
    <div class="text"><p>Short text.</p></div>
    <a class="btn" href="/collection">Discover</a>
  </div>
  <div class="media">
    <picture>...</picture>
  </div>
</div>
```

| Child | Role |
| --- | --- |
| `.content` | The text box, placed inside the push |
| `.media` | The image or video, stretched behind the content, see [Media](../media/) |
| `.surtitle` | An optional eyebrow, pulled back by one gap, see [Surtitle](../surtitle/) |
| `.btn` | The call to action, whose `::after` makes the whole push clickable |

The markup is the same for every modifier: switching a push to a card, or moving its text, is a class change, never a change of structure.

## The push carries the ratio

The push is a one-cell grid with the content padding. Two boxes share that cell: `.content`, placed by the position modifiers below, and an empty `::after` that carries the ratio, `--push-ratio-mobile` below `--sm` and `--push-ratio` from `--sm` up. Negative margins stretch that box over the padding, so the ratio is the push's own. `.media` is absolutely positioned and fills the push.

The push takes the taller of the two boxes: a text that needs more room than the ratio leaves grows the push instead of being cut, even when a layout gives the push a `min-height` to fill its row. The single track is fixed to `minmax(0, 1fr)`, so the ratio box never widens it. To change the shape of one push, override the two ratio tokens on it rather than setting a height.

`.push::after` is therefore taken; `::before` is the darken overlay.

`--push-ratio-double` is not read here. It is a second ratio for layouts that set two pushes side by side and want a taller shape than a full-width one.

## The media is a background

`.media` takes `pointer-events: none`: a click on the image or the video goes through to the push and its link, and a tap never starts or pauses a video. The pause control that a [`.video`](../../embeds/video/) lays over an autoplaying video is the exception, set back to `pointer-events: auto`, since WCAG requires a way to stop it. A video that needs its native `controls` does not belong in a push.

Safari draws a play button over a video it refuses to autoplay, in Low Power Mode for instance. The push hides it through `::-webkit-media-controls-start-playback-button`, with the one `!important` of the component: the button lives in the browser's shadow DOM, and nothing weaker overrides it there. The video then shows its first frame, or its poster.

## What the push hands down

Unless it has `.push-card`, a push sets the text, heading, link and surtitle colours from `--push-color`, and turns `.btn` into a light button that reads on a dark media. `--push-surtitle-color` overrides the surtitle colour alone; it is unset by default, and the surtitle falls back to `--push-color` at `--opacity-muted`.

It also reassigns two other namespaces:

```css
--media-color-background: var(--push-color-background);
--ratio-video: auto;
```

The background colour shows while the media loads, or behind an image with transparent areas. `--ratio-video: auto` lets a `.video` inside the push fill it instead of imposing its own ratio.

## Position and alignment

The content is centred vertically and sits at the start horizontally by default. Three groups of modifiers move it.

| Modifier | Effect |
| --- | --- |
| `.push-center`, `.push-end` | Horizontal position of the content, from `--sm` up |
| `.push-align-start`, `-center`, `-end` | Text alignment, from `--sm` up |
| `.push-vertical-start`, `.push-vertical-end` | Vertical position of the content |

Each one only sets a private variable (`--push-content-items`, `--push-content-align`, `--push-content-justify`), so they combine freely. The content is capped at `--push-content-max-inline-size`, which narrows at `--sm` and `--lg` so that a line of text never spans a full-width push. Those steps follow the viewport, not the push: a layout that sets pushes side by side should reset the token to `100%` on them.

The `.btn` keeps its own width rather than stretching across the content, and follows the text alignment from `--sm` up, like the rest of the text.

A push stretched taller than its ratio, by a grid row or a carousel track, keeps its content where the modifiers put it within the whole height.

## Darken

`.push-darken` paints a gradient between the media and the content, from the edge the content sits against: the start edge by default, the end edge with `.push-end`, the top or bottom with a vertical modifier. Directions flip under `:dir(rtl)`.

The gradient is painted by a `::before` on the push itself, edge to edge, since `.content` is only the text box.

With `.push-center` or `.push-card` there is no edge to darken from, so the whole media is dimmed instead, with `filter: brightness(var(--push-media-brightness))`. That token references `--hero-media-brightness` by default, so a push and a hero dim their media alike.

## Card

`.push-card` puts the text on a background of its own, for media too busy to read text over. Nothing changes in the markup: `.content` itself takes the look of a [card](../card/).

It reads the card tokens for its background, border, radius, shadow, text colour, padding, title typography and transition. Each one goes through an optional `--push-card-*` of the same name first, unset by default:

```css
border-radius: var(--push-card-border-radius, var(--card-border-radius));
padding-block: var(--push-card-padding-block, var(--card-padding-block));
```

Override `--push-card-*` to restyle every push card without touching the cards themselves, or `--card-*` to change both. Its shadow swaps to `--push-card-shadow-hover`, falling back to `--card-shadow-hover`, when the push is hovered. A push card does not inherit `--push-color`, so its text keeps the card colours.

## The whole-push link

`.btn::after` lays an overlay across the push, so a click anywhere on it follows the call to action.

`.content` stacks above the media and the darken overlay through its `z-index` as a grid item, without being positioned, so it is no containing block: the overlay resolves against the push and covers it entirely. Anything that does make `.content` a containing block, such as a `transform` or `translate` from an entrance animation, narrows the clickable area to the text box.

As with the [whole-card link](../card/#the-whole-card-link), a push with more than one interactive element needs a different structure, since the overlay covers the others.
