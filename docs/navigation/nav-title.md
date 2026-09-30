---
isIndex: false
title: Nav title
description: A label above a group of navigation links.
weight: 3
icon: type
---

A title within a navigation block, for labelling a group of links.

```html
<nav aria-labelledby="docs-nav">
  <p class="nav-title" id="docs-nav">Documentation</p>
  <ul class="nav">
    <li><a href="/docs/install">Installation</a></li>
    <li><a href="/docs/usage">Usage</a></li>
  </ul>
</nav>
```

It is a leaf component with no children and no variants:

```css
.nav-title {
  font-size: var(--nav-title-font-size);
  font-weight: var(--nav-title-font-weight);
  margin-block: var(--nav-title-margin-block, 0);
  padding-block: var(--nav-title-padding-block);
  padding-inline: var(--nav-title-padding-inline);
}
```

## It is a sibling of the list, not an item in it

Putting it inside the `<ul>` as an `<li>` would make it a list item that screen readers count and announce as one, and [`.nav`](../nav/) would style it as a navigation entry. Keep it outside.

## Why it carries inline padding

`--nav-title-padding-inline` exists so the label lines up with the link text below it, which is itself inset by `--nav-link-padding-inline`. Keep the two in step when you change either, or the title sits proud of the list it labels.

## Use it as the accessible name

A group of links deserves a label in the accessibility tree as well as on screen. Point the surrounding `<nav>` at it with `aria-labelledby` rather than repeating the text in an `aria-label`, so the two cannot drift apart.
