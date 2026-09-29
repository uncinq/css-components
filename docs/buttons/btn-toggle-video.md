---
isIndex: false
title: Video toggle
description: The play and pause control overlaid on an autoplaying video, driven by the .is-playing state class.
weight: 8
icon: play-circle
---

A play and pause control overlaid on a video. **It requires `.btn`**, and it is the one specialised button that ships no icon of its own.

```html
<div class="video">
  <video autoplay muted loop playsinline></video>
  <button class="btn btn-toggle-video is-playing" aria-label="Pause">
    <span class="icon-pause"></span>
    <span class="icon-play"></span>
  </button>
</div>
```

## Why it exists

A video that autoplays without native controls must be pausable to meet WCAG. This button is that pause control. If your video carries `controls`, you do not need it.

It stays fully transparent, background and border alike, so the video shows through:

```css
--btn-color-background: transparent;
--btn-color-border: transparent;
```

The [`.video`](../../embeds/video/) wrapper is what positions it, pinned to the bottom inline-end corner. Used outside that wrapper it is an ordinary transparent button in the flow.

## Two children, one visible

The whole file beyond the colours is this:

```css
&:not(.is-playing) .icon-pause { display: none; }
&.is-playing .icon-play { display: none; }
```

So the button expects **two** child elements and shows exactly one. The glyphs themselves are not provided here: `.icon-pause` and `.icon-play` are whatever your theme's icon system renders.

## What your script owns

Two things, and getting either wrong breaks the control.

**Toggling `.is-playing`** in step with the video's actual state, including when playback stops for a reason the script did not initiate.

**Keeping the accessible name in sync.** The control means *pause* while playing and *play* while paused, so a fixed `aria-label` is wrong half the time.

```js
button.classList.toggle('is-playing', !video.paused);
button.setAttribute('aria-label', video.paused ? 'Play' : 'Pause');
```

Note that `.is-playing` describes the **video**, not the button, which is why the class stays on while the video runs rather than flipping on click.
