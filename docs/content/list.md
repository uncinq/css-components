---
isIndex: false
title: List
description: A compact labelled list, with the label as a sibling of the list rather than inside it.
weight: 7
icon: list-ul
---

A compact labelled list.

```html
<div class="list">
  <p>Optional label</p>
  <ul>
    <li>First</li>
    <li>Second</li>
  </ul>
</div>
```

**The wrapper is a `div`, and the label is a sibling of the list, not inside it.** That is the one structural thing to get right. Either `ul` or `ol` works.

## What it changes

Three small things, and nothing else:

| Selector | Token | Effect |
| --- | --- | --- |
| `p` | `--list-label-font-weight`, `--list-label-margin-block` | The label's weight and the space under it |
| `li` | `--list-item-font-size`, `--list-item-gap` | Item size, and the space between items as a block margin |
| `.list, ul, ol` | — | `margin-block-end: 0`, so the block ends flush |

Item spacing is a **margin**, not a `gap`, because the list is not a flex container. A `gap` here would do nothing.

## Markers come from css-base

The dash marker is not declared in this file. Bullets, numbering and the marker colour all come from the native list styling in [css-base](../../../css-base/base/), which is also why the component works on `ol` without a variant.

## Where it fits

It is the plain-text counterpart to [`.nav`](../../navigation/nav/), which resets the markers and lays its items out with flexbox because they are links. Use `.list` for content, `.nav` for navigation.
