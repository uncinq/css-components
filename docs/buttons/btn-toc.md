---
isIndex: false
title: Table of contents button
description: The table of contents toggle, a modifier on .btn carrying the toc icon.
weight: 8
icon: list-nested
---

The control that opens a table of contents. **It requires `.btn`.**

```html
<button class="btn btn-toc panel-trigger-lg js-dialog-toggle"
        data-target="#toc"
        aria-label="Table of contents"></button>
```

Construction is identical to [`.btn-search`](../btn-search/): a `--btn-color-*` set pointed at the control palette, with `--btn-toc-color-*` as the per-variant override, and the glyph masked from `--icon-toc`.

## It behaves like the filter button

A table of contents follows the same responsive pattern as a filters panel: a drawer on small screens, a sticky column on large ones. Pair the panel's [`.panel-inline-*`](../../overlays/panel/#inline-variants) with the matching `.panel-trigger-*` on this button, or you keep a button that opens nothing at desktop width.

Because a table of contents is typically a [`.nav`](../../navigation/nav/) inside the panel's `.content`, the panel's colour handover matters here: going inline hands `--nav-color-text` back to the page, so the links stop using the panel's text colour and take the site's link colour instead.
