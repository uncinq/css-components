---
isIndex: false
title: css-components
description: Framework-agnostic CSS component implementations, driven entirely by component tokens and scoped to @layer components.
weight: 5
icon: grid
---

`@uncinq/css-components` is the top of the stack: 29 components and 1 utility, written as plain CSS, reading their values from [@uncinq/component-tokens](../component-tokens/) and adding no values of their own.

```
@uncinq/design-tokens      primitive + semantic values
@uncinq/component-tokens   component-scoped values
@uncinq/css-base           reset, native elements, layouts
@uncinq/css-components     ← this package
```

Every rule lives in `@layer components`, except the scroll-snap utility which lives in `@layer utilities`. There is no build step, no preprocessor, and no JavaScript shipped in the package. Three components do expect a script you provide, and each one documents the contract it needs.

## The components

| Page | Covers |
| --- | --- |
| [Buttons](buttons/) | `.btn` with its colour, size and style variants, plus 7 specialised buttons |
| [Overlays](overlays/) | `.panel`, `.modal`, `.drawer`, `.dropdown` |
| [Content](content/) | `.alert`, `.badge`, `.banner`, `.card`, `.items`, `.list`, `.media`, `.surtitle` |
| [Navigation](navigation/) | `.nav`, `.nav-title`, `.nav-accessibility`, `.breadcrumb-wrapper`, `.pagination` |
| [Embeds](embeds/) | `.embed`, `.video`, `.map` |
| [Forms](forms/) | `.form`, `.form-check` and the form layout helpers |
| [Utilities](utilities/) | `.scrollsnap`, turning a grid into a snap carousel |

Read [Cascade layers](cascade-layers/) first if you are wiring this into a project for the first time.

## Installation

All three packages below are required. The tokens resolve the custom properties, and css-base provides the reset and the element styles the components build on.

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens @uncinq/css-base @uncinq/css-components
```

```css
@layer reset, tokens, libs, vendors, base, layouts, components, pages, utilities;

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

`components/panel.css` is imported **last** among the components. Its `.panel-inline-*` variants have to beat `.modal` and `.drawer`, and they do so on source order rather than on specificity, because most of the file is wrapped in `:where()` and carries no specificity at all.

`utilities/scrollsnap.css` comes after everything, in `@layer utilities`, because it has to win over the grid a component declares for itself in `@layer components`.

If you import file by file rather than using the barrel, preserve those two positions.

## What this package does not include

**No JavaScript.** `.modal`, `.drawer` and `.dropdown` expect a script. The CSS defines the classes and attributes that script must toggle, and each page documents the contract.

**No `.item`.** Despite what a couple of source comments still suggest, `.item` is not defined here. It lives in the Hugolify design system theme. This package ships `.card`, a complete implementation in its own right, and `.items`, a grid whose children you provide. See [Content](content/).

**No icons.** Components that show a glyph, such as `.pagination` controls, expect the theme to supply it through `::before` or `::after` content.

## References

- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens), the values these components read
- [@uncinq/css-base](https://github.com/uncinq/css-base), the foundation underneath
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
