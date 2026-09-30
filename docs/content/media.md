---
isIndex: false
title: Media
description: The structural base for images and video, holding the aspect ratio, plus the logo and icon variants.
weight: 8
icon: image
---

The structural base for media blocks. It holds an aspect ratio and crops whatever is inside it.

```html
<div class="media">
  <picture>
    <img src="..." alt="...">
  </picture>
</div>
```

| Class | Use |
| --- | --- |
| `.media` | Base, for a picture or a video |
| `.media-logo` | Logo, contained rather than cropped |
| `.media-icon` | Icon, no ratio and no background |

`.media-logo` and `.media-icon` are **modifiers**: use them alongside `.media`.

## What the base does

```css
.media {
  aspect-ratio: var(--media-ratio);
  background-color: var(--media-color-background);
  overflow: hidden;
}
```

and then stretches `img`, `picture` and `video` to fill it with `object-fit: cover`. The background shows through while an image loads, which is what keeps a grid of cards from flashing white.

Note that the base declares **no border and no radius**. A bordered media block is a card's doing, not the component's: see below.

## `.media-logo`

Swaps the crop for a contain:

```css
.media-logo {
  picture { display: flex; align-items: center; justify-content: center; }
  img { object-fit: contain; max-height: var(--media-logo-max-height); max-width: var(--media-logo-max-width); }
}
```

A logo must never be cropped, and it must not fill the box either, hence the two maxima and the centring. Everything else, ratio and background, still comes from `.media`.

## `.media-icon`

Removes the two things that make a media block a media block:

```css
.media-icon {
  --media-ratio: none;
  --media-color-background: transparent;
}
```

So it takes the size of its content. Inside a [card](../card/) it gets far more than that: the card maps eight `--card-media-icon-*` tokens onto the media namespace, including `--icon-size` and `--icon-color`, which is what gives icon cards their own look without a third class.

## Context over variants

A media block inside a card reads the card's tokens rather than its own:

```css
.card {
  --media-color-background: var(--card-media-color-background);
  --media-ratio: var(--card-media-ratio);
}
```

and the card adds the border, radius and margins on `.media` directly. So the same `<div class="media">` is a full-bleed card header in one place and a standalone framed image in another, with no class changing.

That is the pattern to copy when you need a media block to look different in a new context: set the `--media-*` tokens on the context, not a variant on the block.

## Video inside a media block

```css
.media .video { height: 100%; }
```

A [`.video`](../../embeds/video/) wrapper is stretched to fill the media box, so a video and an image are interchangeable inside a card.
