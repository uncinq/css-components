---
isIndex: false
title: Embeds
description: Aspect-ratio wrappers for third-party embeds, native video and maps.
weight: 6
icon: play-btn
---

## `.embed`

A responsive container for third-party embeds, holding an aspect ratio so the page does not reflow once the iframe loads.

```html
<div class="embed">
  <iframe src="https://..." title="Video title" allowfullscreen></iframe>
</div>
```

Accepts `iframe`, `video`, `embed` or `object` as its child. The ratio comes from `--embed-ratio`, which defaults to the video ratio token. Override it per instance for a square or portrait embed:

```html
<div class="embed" style="--embed-ratio: 1">...</div>
```

Always give the `iframe` a `title`. It is the only accessible name an embedded frame has.

## `.video`

A wrapper for a **native** `<video>`, as opposed to an embedded player.

```html
<div class="video">
  <video autoplay muted loop playsinline></video>
  <button class="btn btn-toggle-video is-playing" aria-label="Pause">
    <span class="icon-pause"></span>
    <span class="icon-play"></span>
  </button>
</div>
```

The wrapper positions the optional toggle control over the video. Inside a `.media`, the wrapper is stretched to fill it, so a video and an image are interchangeable within a card.

The control is there for a reason rather than for decoration. A video that autoplays without native controls must be pausable to meet WCAG, which is exactly what `.btn-toggle-video` provides. If your video carries `controls`, you do not need it. See [Buttons](../buttons/#btn-toggle-video) for the `.is-playing` contract.

## `.map`

A map container, driven by Leaflet through a `js-map` hook.

```html
<div class="map js-map"
     data-markers="[...]"
     data-zoom="12"
     data-tile="3"
     data-marker-hidden="false"></div>
```

| Attribute | Role |
| --- | --- |
| `data-markers` | The markers to plot |
| `data-zoom` | Initial zoom level |
| `data-tile` | Tile layer selection |
| `data-marker-hidden` | Whether markers start hidden |

This package provides the container only: sizing, background and border. Leaflet itself, its stylesheet and the script reading those attributes are yours to supply.

Leaflet's own CSS is a good example of why the layer order matters. Import it into `@layer libs` and any adjustments you write into `@layer vendors`, so that a lazily loaded Leaflet cannot outrank your own rules on source order. See [Cascade layers](../cascade-layers/).
