---
isIndex: false
title: Overview
description: Three containers that reserve their aspect ratio before the content arrives, and stop at the container.
weight: 1
icon: book
---

Three containers, all built on the same idea: hold the space before the content arrives.

| Page | Class | For |
| --- | --- | --- |
| [Embed](../embed/) | `.embed` | Third-party iframes, players, social cards |
| [Video](../video/) | `.video` | A native `<video>`, with an optional pause control |
| [Map](../map/) | `.map` | A Leaflet map container |

## They all reserve an aspect ratio

An iframe, a video or a map that sizes itself once its content loads pushes the rest of the page down at an unpredictable moment. Each of these components declares `aspect-ratio` from a token, so the box exists at its final size from the first paint.

That is also why all three set `overflow: hidden`: the content is stretched to fill a box whose proportions it did not choose.

## They stop at the container

None of the three ships the thing it wraps. `.embed` gives you no player, `.video` gives you no controls beyond the [toggle button](../../buttons/btn-toggle-video/), and `.map` gives you neither Leaflet nor its stylesheet. Loading those, and keeping them out of your own layers, is the consuming project's job. See [Cascade layers](../../cascade-layers/) for where third-party CSS belongs.
