---
isIndex: false
title: Breadcrumb
description: The breadcrumb trail, with the class on the wrapper and the separator drawn from a token.
weight: 5
icon: chevron-double-right
---

The breadcrumb trail. **The class goes on the wrapper, not on the list.**

```html
<nav class="breadcrumb-wrapper" aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Docs</a></li>
    <li aria-current="page">Navigation</li>
  </ol>
</nav>
```

`.breadcrumb-wrapper` styles the `ol` and the `li` inside it by element, so there is no class to put on either. The name says wrapper for that reason.

## The separator

Drawn on `li::before` from a token, with the first item's suppressed:

```css
li::before { content: var(--breadcrumb-separator); color: var(--breadcrumb-color-separator); }
li:first-child::before { display: none; }
```

`--breadcrumb-separator` defaults to `"/"` and takes any `content` value, so a chevron or an arrow is a token change. It needs the quotes, since it is dropped into `content` as is.

Because the separator is generated content rather than text, it is not selected when a reader copies the trail, and screen readers skip it.

## Marking the current page

Use `aria-current="page"`, which is both the accessible marker and the styling hook:

```css
li[aria-current="page"] { color: var(--breadcrumb-color-text-active); }
```

There is no `.active` class here, deliberately. Relying on the attribute means the visual state and the announced state cannot disagree, and it rules out the other common approach, styling `:last-child`, which breaks the moment a trail ends on something other than the current page.

## Overflow on narrow screens

```css
overflow-x: auto;
li { flex-shrink: 0; white-space: nowrap; }
```

A long trail scrolls sideways rather than wrapping into several lines or truncating. Items keep their full width, so the deepest entries stay readable and the reader scrolls to reach the shallow ones.

The wrapper also carries a block border and its own background, which is what lets a breadcrumb sit as a full-width band under the header.

## Accessibility

`aria-label` on the `<nav>` is what distinguishes this landmark from the other navigation regions on the page. Use an `<ol>`, not a `<ul>`: the order of a breadcrumb is its meaning.
