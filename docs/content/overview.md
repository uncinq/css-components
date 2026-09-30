---
isIndex: false
title: Overview
description: The eight content components, the three that are meant to nest, and the two colour patterns they follow.
weight: 1
icon: book
---

Eight components for laying out page content.

| Page | Class | Purpose |
| --- | --- | --- |
| [Alert](../alert/) | `.alert` | An inline notification block |
| [Badge](../badge/) | `.badge` | A small inline label |
| [Banner](../banner/) | `.banner` | A centred modifier on `.alert` |
| [Card](../card/) | `.card` | A self-contained content card |
| [Items](../items/) | `.items` | The responsive grid that holds cards |
| [List](../list/) | `.list` | A compact labelled list |
| [Media](../media/) | `.media` | The structural base for images and video |
| [Surtitle](../surtitle/) | `.surtitle` | An eyebrow label above a heading |

## Three of them are meant to nest

`.items` holds `.card`, a card holds a `.media`, and a `.surtitle` sits above the card's title. Each one reassigns the next one's tokens from its own namespace, so a media block or a surtitle adapts to its context without a variant class. The [card page](../card/) covers what it hands down.

{{< alert-block state="info" >}}
Several source comments describe `.card` as a backwards-compatible alias for `.item` and recommend using `.item` directly. That is misleading for anyone consuming this package on its own: **`.item` is not defined here**. It lives in the Hugolify design system theme. Within `@uncinq/css-components`, `.card` is a complete implementation and the class to use.
{{< /alert-block >}}

## Colour variants follow one of two patterns

`.alert` and `.badge` both ship the same ten intent names, and they resolve them differently.

| | Background | Text |
| --- | --- | --- |
| `.alert-*` | `--color-*-muted` | `--color-*-strong` |
| `.badge-*` | `--color-*` | `--color-text-on-*` |

An alert is a tinted block meant to hold a paragraph, so it uses the muted pair. A badge is a solid chip meant to be read at a glance, so it uses the full colour with its guaranteed-contrast text token. Neither ever hardcodes a colour, which is what makes a rebrand a token change rather than a search and replace.
