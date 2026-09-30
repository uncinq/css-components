---
isIndex: false
title: Share button
description: The share control, a modifier on .btn carrying the share icon.
weight: 7
icon: share
---

A share control. **It requires `.btn`.**

```html
<button class="btn btn-share" aria-label="Share">Share</button>
```

Construction is identical to [`.btn-search`](../btn-search/): a `--btn-color-*` set pointed at the control palette, with `--btn-share-color-*` as the per-variant override, and the glyph masked from `--icon-share`.

## It ships no behaviour

The class is presentation only. Whether the button calls `navigator.share()`, copies a URL to the clipboard or opens a [dropdown](../../overlays/dropdown/) of destinations is entirely up to your script.

If it opens a menu, the dropdown contract applies: the trigger needs `.dropdown-toggle` and `.js-dropdown-toggle`, and its next element sibling must be the `.dropdown-menu`.

```html
<div class="dropdown">
  <button class="btn btn-share dropdown-toggle js-dropdown-toggle" aria-expanded="false">Share</button>
  <ul class="dropdown-menu">
    <li><a href="...">Bluesky</a></li>
    <li><a href="...">LinkedIn</a></li>
  </ul>
</div>
```

Note that `.dropdown-toggle` draws its own chevron on `::after` while `.btn-share` draws the share glyph on `::before`, so the combination gives you both, one on each side of the label.
