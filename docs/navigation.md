---
isIndex: false
title: Navigation
description: Navigation lists, skip links, breadcrumbs and pagination.
weight: 5
icon: signpost
---

## `.nav`

The base navigation list, vertical by default.

```html
<ul class="nav">
  <li><a href="/">Home</a></li>
  <li><a href="/about">About</a></li>
</ul>
```

Works on `ul` or `ol`. **Direction is set by context, not by a modifier class**: a header lays its nav out horizontally by setting `flex-direction: row` on it. That keeps the component free of layout variants that would only ever be used in one place.

## `.nav-title`

A title within a navigation block, for labelling a group of links.

## `.nav-accessibility`

Skip links, for keyboard and assistive technology users.

```html
<ul class="nav nav-accessibility">
  <li><a href="#main">Skip to content</a></li>
  <li><a href="#navigation">Skip to navigation</a></li>
  <li><a href="#footer">Skip to footer</a></li>
</ul>
```

Three things matter here, and getting any of them wrong defeats the purpose:

**It must be the first focusable element in `<body>`.** A skip link that comes after the navigation it skips is useless.

**It is hidden off-screen, not hidden.** It slides into view when a child receives focus. Using `display: none` would remove it from the tab order entirely.

**It requires the `.nav` base class** alongside it.

The targets must exist and be focusable. An `id` on a non-interactive element needs `tabindex="-1"` for the jump to move focus rather than just scroll.

## `.breadcrumb-wrapper`

The breadcrumb trail. Note that the class goes on the **wrapper**, not on the list itself.

```html
<nav class="breadcrumb-wrapper" aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Docs</a></li>
    <li aria-current="page">Navigation</li>
  </ol>
</nav>
```

The separator is drawn from `--breadcrumb-separator`, which defaults to `"/"`. Mark the current page with `aria-current="page"` rather than relying on it being last.

## `.pagination`

A page navigation list.

```html
<nav role="navigation" aria-label="Pagination">
  <ul class="pagination">
    <li class="disabled"><a class="first">First</a></li>
    <li><a class="previous">Previous</a></li>
    <li class="item active"><a href="?page=1">1</a></li>
    <li class="item item-adjacent"><a href="?page=2">2</a></li>
    <li><a class="next">Next</a></li>
    <li><a class="last">Last</a></li>
  </ul>
</nav>
```

| Class | Role |
| --- | --- |
| `.first`, `.previous`, `.next`, `.last` | Navigation controls, on the `a` |
| `.item` | A page number, on the `li` |
| `.item-adjacent` | A page number next to the current one |
| `.active` | The current page |
| `.disabled` | An unavailable control, on the `li` |

`.item-adjacent` exists so that a responsive paginator can hide distant page numbers while keeping the immediate neighbours visible, which is the usual mobile treatment.

**The glyphs for `.first`, `.last`, `.previous` and `.next` are not provided.** This package positions and spaces them; the theme supplies the arrow through `a::before` or `a::after` content. A paginator with no theme styling will show the text labels only, which is a reasonable fallback but rarely what you want visually.

Note that `.item` here means a pagination entry, and is unrelated to the `.item` card unit mentioned in [Content](../content/).
