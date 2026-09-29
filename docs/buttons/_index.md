---
isIndex: false
title: Buttons
description: The .btn base class with its colour, size and style variants, and the seven specialised buttons built on it.
weight: 2
icon: hand-index
---

Eight files: one base class, and seven single-purpose buttons.

| Page | Class | Needs `.btn` |
| --- | --- | --- |
| [Button](btn/) | `.btn` | — |
| [Close button](btn-close/) | `.btn-close` | no |
| [Menu button](btn-menu/) | `.btn-menu` | no |
| [Search button](btn-search/) | `.btn-search` | yes |
| [Filter button](btn-filter/) | `.btn-filter` | yes |
| [Share button](btn-share/) | `.btn-share` | yes |
| [Table of contents button](btn-toc/) | `.btn-toc` | yes |
| [Video toggle](btn-toggle-video/) | `.btn-toggle-video` | yes |

## Two kinds of specialised button

Read the third column before using one, because the two kinds behave differently when the base class is missing.

`.btn-close` and `.btn-menu` **redeclare everything they need**. Each `--btn-close-*` and `--btn-menu-*` token falls back to the matching `--btn-*` one, so they look consistent with `.btn` without depending on it. Use them on their own.

The other five are **modifiers**. They declare nothing but `--btn-color-*` reassignments and an icon, so omitting `.btn` leaves them almost entirely unstyled.

```html
<!-- self-contained -->
<button class="btn-close" aria-label="Close"></button>

<!-- modifier on .btn -->
<button class="btn btn-search" aria-label="Search"></button>
```

## The icons are masks, not images

Six of the seven ship their glyph, drawn on `::before` from the icon tokens in [@uncinq/design-tokens](../../design-tokens/reference/#icon):

```css
&::before {
  background-color: currentColor;
  mask: var(--icon-search) center / var(--btn-search-icon-size, var(--icon-size)) no-repeat;
}
```

Because it is a mask over a background colour rather than a background image, the glyph takes `currentColor` and follows the button's text colour through every state, including hover and disabled. Size it globally with `--icon-size`, or per button with `--btn-close-icon-size` and its siblings.

`.btn` publishes `--icon-size: var(--btn-font-size)`, so a modifier button scales with its size class for free. A bare `.btn-close` or `.btn-menu` has no such declaration above it and takes whatever `--icon-size` is in scope.

[`.btn-toggle-video`](btn-toggle-video/) is the exception: it ships no icon and expects two child elements instead.

## None of them names itself

When a button has no visible text, give it an accessible name with `aria-label`, or a `.visually-hidden` span from [css-base](../../css-base/base/#accessibility-helpers). No button in this package provides one.

## Focus and disabled states

`.btn` declares both:

```css
&:focus-visible {
  outline: var(--focus-outline-width) var(--focus-outline-style) var(--focus-color-outline);
}

&:disabled,
&[aria-disabled='true'] {
  cursor: not-allowed;
  opacity: var(--opacity-disabled);
}
```

The five modifiers inherit that through the base class. **`.btn-close` and `.btn-menu` declare neither**, so they fall back to the browser's own focus ring rather than the design system's, and a disabled one looks no different from an enabled one. Add `.btn` alongside them when either matters.
