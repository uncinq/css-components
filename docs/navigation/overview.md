---
isIndex: false
title: Overview
description: The five navigation components, the two that take their class on the wrapper, and the landmarks none of them supplies.
weight: 1
icon: book
---

Five components for moving around a site.

| Page | Class | Purpose |
| --- | --- | --- |
| [Nav](../nav/) | `.nav` | The base navigation list |
| [Nav title](../nav-title/) | `.nav-title` | A label above a group of links |
| [Skip links](../nav-accessibility/) | `.nav-accessibility` | Skip navigation for keyboard and assistive technology |
| [Breadcrumb](../breadcrumb/) | `.breadcrumb-wrapper` | The breadcrumb trail |
| [Pagination](../pagination/) | `.pagination` | Page navigation |

## Two of them take the class on the wrapper

`.breadcrumb-wrapper` goes on the `<nav>`, not on the `<ol>` inside it. `.nav-accessibility` goes on the list itself, but only alongside `.nav`. Neither is guessable from the name, so check the page before writing the markup.

## None of them supplies the landmark

`<nav>`, `aria-label` and `aria-current` are yours to write. A page with three navigation regions and no labels on them is harder to use with a screen reader than a page with one, which is the usual reason to add them.

## `.item` means two different things

In [pagination](../pagination/), `.item` is a page number. It is unrelated to the `.item` card unit mentioned throughout the [Content](../../content/) section, which is not defined in this package at all.
