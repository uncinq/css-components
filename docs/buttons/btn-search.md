---
isIndex: false
title: Search button
description: The search toggle, a modifier on .btn carrying the search icon.
weight: 5
icon: search
---

A search control. **It requires `.btn`**: on its own it is nothing but a colour set and an icon.

```html
<button class="btn btn-search js-dialog-toggle" data-target="#search" aria-label="Search"></button>
```

## What it declares

Two things, and nothing else.

The `--btn-color-*` set is retargeted at the control palette, with a per-button escape hatch in front of it:

```css
--btn-color-background: var(--btn-search-color-background, var(--btn-control-color-background));
--btn-color-text: var(--btn-search-color-text, var(--btn-control-color-text));
```

Override `--btn-search-color-*` to restyle every search button, or `--btn-color-*` directly on one instance.

And the glyph, masked from `--icon-search` on `::before`, sized by `--btn-search-icon-size` falling back to `--icon-size`, which `.btn` sets to its own font size.

That is the whole file. Geometry, focus, hover, disabled and the transition all come from the base class, which is why omitting it leaves an unstyled button with an icon.

## Adding a label

The icon sits on `::before`, so any text content lands beside it with `--btn-gap` between the two. No extra class is needed:

```html
<button class="btn btn-search">Search</button>
```

Without visible text, `aria-label` is required.
