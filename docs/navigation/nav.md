---
isIndex: false
title: Nav
description: The base navigation list, vertical by default, whose direction is set by context rather than by a modifier class.
weight: 1
icon: compass
---

The base navigation list, vertical by default.

```html
<ul class="nav">
  <li><a href="/">Home</a></li>
  <li><a href="/about">About</a></li>
</ul>
```

Works on `ul` or `ol`.

## Direction is set by context, not by a modifier

There is no `.nav-horizontal`. The direction comes from a token:

```css
flex-direction: var(--nav-direction, column);
```

so a header lays its own navigation out horizontally by setting `--nav-direction: row` on it, or `flex-direction` directly. That keeps the component free of layout variants that would only ever be used in one place, and it means the same list is a column in a drawer and a row in a header without a class change.

## Nested lists inherit the layout

```css
&,
:is(ul, ol) { display: flex; flex-direction: var(--nav-direction, column); ... }
```

The rule matches the nav **and** every list inside it, so a submenu needs no second `.nav` class. Note that a nested list inherits `--nav-direction` too, so a horizontal header menu gets horizontal submenus unless the submenu resets it.

## Links

`a:where(:not(.btn))` and `li > span` are styled together, so a non-link entry, the current page rendered as text, lines up with the links around it. `.btn` is excluded through `:where()`, which adds no specificity, so a call to action inside a navigation keeps its own styling.

The current entry is marked with a class on the link:

```html
<li><a href="/about" class="active" aria-current="page">About</a></li>
```

```css
&.active { --nav-color-text: var(--nav-color-text-active); }
```

The class drives the colour; `aria-current` is what a screen reader announces. Write both.

## Highlighted items

`.nav-item-highlighted` tightens the line height and pulls the item out to the nav's own inline padding, for an entry rendered as a block rather than as a line of text.

## Dropdowns inside a nav

`.dropdown-toggle` is restyled to take `--nav-color-text`, so a [dropdown](../../overlays/dropdown/) in a header menu matches the links beside it. Nothing else about the dropdown changes.

## Inside a panel

A `.modal` or `.drawer` publishes `--nav-color-text` from its own text colour, so a navigation inside a panel follows the panel's palette. Going inline with [`.panel-inline-*`](../../overlays/panel/#inline-variants) hands that token back to the page, which is why a drawer menu that becomes a sidebar column changes colour at the breakpoint.
