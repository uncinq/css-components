---
isIndex: false
title: Embed
description: A responsive container for third-party embeds, holding an aspect ratio so the page does not reflow.
weight: 2
icon: aspect-ratio
---

A responsive container for third-party embeds, holding an aspect ratio so the page does not reflow once the iframe loads.

```html
<div class="embed">
  <iframe src="https://..." title="Video title" allowfullscreen></iframe>
</div>
```

Accepts `iframe`, `video`, `embed` or `object` as its child, each stretched to fill the box.

## The ratio

`--embed-ratio` defaults to the video ratio token. Override it per instance for a square or portrait embed:

```html
<div class="embed" style="--embed-ratio: 1">...</div>
```

or per context, which is usually better:

```css
.podcast-player { --embed-ratio: 4 / 1; }
```

## Frame and shadow

`--embed-border`, `--embed-border-radius` and `--embed-box-shadow` are the three decoration tokens. `overflow: hidden` is what lets the radius clip the iframe at the corners, since an iframe cannot be given one of its own.

Note that `--embed-border` is a **shorthand** token taking width, style and colour together, unlike most components in this package which split the three. Set it as one value:

```css
.embed { --embed-border: 1px solid var(--color-border); }
```

## Always give the iframe a title

```html
<iframe src="..." title="2024 annual report, presented by the CFO"></iframe>
```

It is the only accessible name an embedded frame has. Without it a screen reader announces "frame" and the reader has to enter it to find out what it holds.

## Embed or video?

Use `.embed` for a **third-party player**, where the content is an iframe you do not control. Use [`.video`](../video/) for a native `<video>` element, which needs a positioning context for its pause control rather than a ratio wrapper.
