---
isIndex: false
title: Filter button
description: The filters toggle, a modifier on .btn carrying the filter icon.
weight: 6
icon: funnel
---

The control that opens a filters panel. **It requires `.btn`.**

```html
<button class="btn btn-filter panel-trigger-md js-dialog-toggle"
        data-target="#filters"
        aria-label="Filters"></button>
```

Like [`.btn-search`](../btn-search/), it declares only a `--btn-color-*` set pointed at the control palette, with `--btn-filter-color-*` in front of it as the override hook, and an icon masked from `--icon-filter`. Everything else comes from `.btn`.

## It usually needs a `.panel-trigger-*`

A filters panel is the textbook case for [`.panel-inline-*`](../../overlays/panel/#inline-variants): a drawer on mobile, a sidebar column on desktop. Once the panel is a column there is nothing left to open, so the button has to go with it.

The two classes are separate and must be paired by hand:

| Panel carries | Button carries |
| --- | --- |
| `.panel-inline-sm` | `.panel-trigger-sm` |
| `.panel-inline-md` | `.panel-trigger-md` |
| `.panel-inline-lg` | `.panel-trigger-lg` |
| `.panel-inline-xl` | `.panel-trigger-xl` |

A mismatch leaves a button on screen that opens nothing. The reasoning behind the two-class design is on the [panel page](../../overlays/panel/#hiding-the-trigger).

## Placement

The filter button usually lives in the page heading rather than beside the panel it opens, which is worth remembering when writing layout rules around it. It is an ordinary inline-flex button and takes whatever the surrounding layout gives it.
