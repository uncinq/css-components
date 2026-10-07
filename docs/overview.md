---
isIndex: false
title: Overview
description: The 30 components and 1 utility, how to install them, the import order inside the package, and what it deliberately leaves out.
weight: 1
icon: book
---

`@uncinq/css-components` is the top of the stack: 30 components and 1 utility, written as plain CSS, reading their values from [@uncinq/component-tokens](../../component-tokens/) and adding no values of their own.

```
@uncinq/design-tokens      primitive + semantic values
@uncinq/component-tokens   component-scoped values
@uncinq/css-base           reset, native elements, layouts
@uncinq/css-components     ← this package
```

Every rule lives in `@layer components`, except the scroll-snap utility which lives in `@layer utilities`. There is no build step, no preprocessor, and no JavaScript shipped in the package. Three components do expect a script you provide, and each one documents the contract it needs.

## The components

One page per component, grouped into six sections.

| Section | Components |
| --- | --- |
| [Buttons](../buttons/) | [`.btn`](../buttons/btn/), [`.btn-close`](../buttons/btn-close/), [`.btn-menu`](../buttons/btn-menu/), [`.btn-search`](../buttons/btn-search/), [`.btn-filter`](../buttons/btn-filter/), [`.btn-share`](../buttons/btn-share/), [`.btn-toc`](../buttons/btn-toc/), [`.btn-toggle-video`](../buttons/btn-toggle-video/) |
| [Overlays](../overlays/) | [panel](../overlays/panel/), [`.modal`](../overlays/modal/), [`.drawer`](../overlays/drawer/), [`.dropdown`](../overlays/dropdown/) |
| [Content](../content/) | [`.alert`](../content/alert/), [`.badge`](../content/badge/), [`.banner`](../content/banner/), [`.card`](../content/card/), [`.items`](../content/items/), [`.list`](../content/list/), [`.media`](../content/media/), [`.push`](../content/push/), [`.surtitle`](../content/surtitle/) |
| [Navigation](../navigation/) | [`.nav`](../navigation/nav/), [`.nav-title`](../navigation/nav-title/), [`.nav-accessibility`](../navigation/nav-accessibility/), [`.breadcrumb-wrapper`](../navigation/breadcrumb/), [`.pagination`](../navigation/pagination/) |
| [Embeds](../embeds/) | [`.embed`](../embeds/embed/), [`.video`](../embeds/video/), [`.map`](../embeds/map/) |
| [Forms](../forms/) | [`.form`](../forms/form/), [`.form-check`](../forms/form-check/) |
| [Utilities](../utilities/) | [`.scrollsnap`](../utilities/scrollsnap/) |

Read [Cascade layers](../cascade-layers/) first if you are wiring this into a project for the first time.

## Installation

All three packages below are required. The tokens resolve the custom properties, and css-base provides the reset and the element styles the components build on.

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens @uncinq/css-base @uncinq/css-components
```

```css
@layer reset, tokens, libs, base, vendors, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components, utilities */
```

Per component, when you only need a few:

```css
@import '@uncinq/css-components/css/components/button.css';
@import '@uncinq/css-components/css/components/alert.css';
@import '@uncinq/css-components/css/utilities/scrollsnap.css';
```

### A CDN link is not enough

`@uncinq/design-tokens` and `@uncinq/component-tokens` can be linked straight from a CDN, because they emit nothing but custom properties. **This package cannot**, and neither can `@uncinq/css-base`.

Both ship `@media (--sm)` queries that rely on the `@custom-media` rules declared in `css-base/css/mediaqueries.css`, and no browser implements `@custom-media`. Loaded without [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media), 13 responsive blocks in this package are dropped: `.panel-inline-*`, `.panel-trigger-*` and every `.scrollsnap-*` variant.

The failure is silent. No error is raised, the page simply stays at its mobile values on every screen. A build step is required.

Note that `@uncinq/css-base` is a genuine prerequisite but is **not** declared in `peerDependencies`. Install it explicitly.

## Import order inside the package

`css/index.css` imports the components alphabetically, with two deliberate exceptions.

`components/button.css` comes **before** the seven `btn-*` files rather than after them, because five of those are modifiers on `.btn` and read better in that order.

`utilities/scrollsnap.css` comes after everything, in `@layer utilities`, because it has to win over the grid a component declares for itself in `@layer components`.

One constraint is easy to break when importing file by file: `panel.css` must come **after** `modal.css` and `drawer.css`. Alphabetical order already gives that, which is why nothing in the barrel looks unusual, but the `.panel-inline-*` variants beat the two host components on source order rather than on specificity. See [Cascade layers](../cascade-layers/).

## What this package does not include

**No JavaScript.** `.modal`, `.drawer` and `.dropdown` expect a script. The CSS defines the classes and attributes that script must toggle, and each page documents the contract.

**No `.item`.** Despite what a couple of source comments still suggest, `.item` is not defined here. It lives in the Hugolify design system theme. This package ships `.card`, a complete implementation in its own right, and `.items`, a grid whose children you provide. See [Content](../content/).

**No icons for `.icon`.** Components that draw a glyph, the [buttons](../buttons/) and [`.pagination`](../navigation/pagination/), mask it from an icon token. An `.icon` element you place inside an [alert](../content/alert/) or a [video toggle](../buttons/btn-toggle-video/) is the theme's to render.

## References

- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens), the values these components read
- [@uncinq/css-base](https://github.com/uncinq/css-base), the foundation underneath
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
