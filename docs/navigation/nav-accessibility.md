---
isIndex: false
title: Skip links
description: Skip navigation for keyboard and assistive technology users, hidden off-screen rather than hidden.
weight: 4
icon: universal-access
---

Skip links, for keyboard and assistive technology users.

```html
<ul class="nav nav-accessibility">
  <li><a href="#main">Skip to content</a></li>
  <li><a href="#navigation">Skip to navigation</a></li>
  <li><a href="#footer">Skip to footer</a></li>
</ul>
```

Three things matter here, and getting any of them wrong defeats the purpose.

## It must be the first focusable element in `<body>`

A skip link that comes after the navigation it skips is useless. It goes before the header, before any cookie banner, before anything else that can take focus.

## It is hidden off-screen, not hidden

```css
.nav-accessibility {
  position: absolute;
  transform: translateY(-100%);

  &:focus-within { transform: translateY(0); }
}
```

`display: none` or `visibility: hidden` would remove it from the tab order entirely, which is the one thing a skip link cannot afford. It is pushed out of view with a transform and slides back on `:focus-within`, so the whole group appears as soon as any link inside it is reached.

The slide is behind `prefers-reduced-motion`; with motion disabled the group simply appears.

## It requires the `.nav` base class

`.nav-accessibility` sets `flex-direction: row` and a small font size, but the flex layout, the list reset and the link padding all come from [`.nav`](../nav/). On its own it is an unstyled list positioned off-screen.

## The focus treatment is deliberate

```css
a {
  background: var(--color-background);
  &:focus {
    background: var(--color-text);
    color: var(--color-background);
    outline: none;
  }
}
```

The resting background is what keeps the links readable once the group slides over the page content. On focus the colours invert, which is why the usual focus ring is dropped: the inversion is the focus indicator, and it meets the contrast requirement on its own.

Note this uses `:focus`, not `:focus-visible`, so the indicator appears however focus arrived.

## The targets have to be focusable

`z-index: 300` puts the group above sticky headers, but a working skip link also needs somewhere to land. An `id` on a non-interactive element needs `tabindex="-1"`, otherwise the jump scrolls the page without moving focus, and the next Tab press continues from the top of the document:

```html
<main id="main" tabindex="-1">
```
