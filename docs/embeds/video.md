---
isIndex: false
title: Video
description: A wrapper for a native video, positioning the optional pause control an autoplaying video needs.
weight: 3
icon: camera-video
---

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

## What it does

Three things:

```css
.video {
  position: relative;

  video { aspect-ratio: var(--video-ratio, var(--ratio-video)); object-fit: var(--video-fit, contain); width: 100%; }

  .btn-toggle-video { position: absolute; inset-block-end: 0; inset-inline-end: 0; z-index: 10; }
}
```

It establishes the positioning context, sizes the video to its container at a known ratio, and pins the control to the bottom inline-end corner. The ratio falls back to `--ratio-video` from the design tokens, so `--video-ratio` only needs setting for a portrait or square clip.

## The control is not decoration

A video that autoplays without native controls must be pausable to meet WCAG, which is exactly what [`.btn-toggle-video`](../../buttons/btn-toggle-video/) provides. If your video carries `controls`, you do not need it.

The button is the only child the wrapper positions. Anything else you put inside sits in the normal flow.

## Inside a media block

```css
.media .video { height: 100%; }
```

A [`.media`](../../content/media/) stretches the wrapper to fill it, so a video and an image are interchangeable within a [card](../../content/card/) without changing the card's structure. The media block's own `--media-ratio` then governs the box, and the video is cropped to it by `object-fit: cover`.

## Autoplay checklist

`muted` is required for autoplay to be allowed at all, and `playsinline` keeps iOS from taking the video fullscreen. `loop` is what makes a background video a background video. All three are attributes on the element, not something this component can supply.
