---
isIndex: false
title: Map
description: A Leaflet map container, and where Leaflet's own stylesheet belongs in the layer order.
weight: 4
icon: map
---

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

This package provides the container only: ratio, background, radius and margins. **Leaflet itself, its stylesheet and the script reading those attributes are yours to supply.**

## It breaks out on mobile

```css
@media not (--sm) {
  --map-ratio: var(--map-ratio-mobile);

  .container & { margin-inline: calc(-1 * var(--gutter)); }
  .row { display: block; }
}
```

Three changes below 768px. The ratio switches to `--map-ratio-mobile`, usually taller, because a wide map on a narrow screen shows almost nothing useful. Inside a `.container` the map pulls out to the screen edges by one gutter. And a `.row` inside it stops laying out in columns.

The bleed is deliberately scoped to `.container &`, so a map in a card or a sidebar stays inside its parent instead of overflowing something with no room to give.

## Where Leaflet's CSS goes

This is the clearest case in the package for why the layer order matters. Leaflet ships its own stylesheet, and a map is often loaded lazily.

```css
@layer reset, tokens, libs, base, vendors, layouts, components, pages, utilities;
```

Import Leaflet's stylesheet into `@layer libs`, and any adjustments you write into `@layer vendors`. A layer's position is fixed the first time its name appears, so a stylesheet injected at runtime lands in the slot its layer already occupies rather than at the end of the cascade. Without that, a lazily loaded Leaflet outranks your own rules purely on source order, and the map's marker styling starts winning arguments it should lose.

See [Cascade layers](../../cascade-layers/).

## Accessibility

A map with no text alternative is unusable without sight. Whatever the map shows, addresses, opening hours, a route, should also exist as text near it. The container has no role and announces nothing on its own.
