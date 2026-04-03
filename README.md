# @uncinq/css-components

> Framework-agnostic CSS component implementations for Un Cinq projects — actual styles built on top of design and component tokens.

## What is this?

`@uncinq/css-components` provides the CSS implementations of common UI components. It sits at the top of the Un Cinq CSS stack:

```text
@uncinq/design-tokens      ← primitive + semantic CSS custom properties
@uncinq/component-tokens   ← component-scoped CSS custom properties
@uncinq/css-base           ← base CSS rules
@uncinq/css-components     ← actual CSS rules (this package)
```

Each component file contains only CSS rules, using the tokens defined in the two packages above. No JavaScript, no markup opinions — just portable, layered CSS.

---

## CSS cascade layers

All component styles are declared inside `@layer components`:

```css
@layer config, base, layouts, vendors, components;
```

This means:

- Components never bleed into base or layout styles
- Project-level `@layer components` rules always win over these defaults
- Vendors (e.g. Splide) sit below project components but above base

---

## Prerequisites

This package consumes tokens from:

- [@uncinq/design-tokens](https://github.com/uncinq/design-tokens) — semantic CSS custom properties
- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens) — component-scoped CSS custom properties
- [@uncinq/css-base](https://github.com/uncinq/css-base) — base CSS rules

Import them before this package:

```css
@import '@uncinq/design-tokens';
@import '@uncinq/component-tokens';
@import '@uncinq/css-base';
@import '@uncinq/css-components';
```

---

## Installation

```bash
npm install @uncinq/css-components
# or
yarn add @uncinq/css-components
```

### Usage — full import

```css
@import '@uncinq/design-tokens';
@import '@uncinq/component-tokens';
@import '@uncinq/css-base';
@import '@uncinq/css-components';
```

### Usage — per component

```css
@import '@uncinq/design-tokens';
@import '@uncinq/component-tokens';
@import '@uncinq/css-base';
@import '@uncinq/css-components/css/component/nav.css';
@import '@uncinq/css-components/css/component/pagination.css';
```

### Usage — CDN (no build step)

```html
<link rel="stylesheet" href="https://unpkg.com/@uncinq/design-tokens/tokens/index.css">
<link rel="stylesheet" href="https://unpkg.com/@uncinq/component-tokens/tokens/index.css">
<link rel="stylesheet" href="https://unpkg.com/@uncinq/css-base/css/index.css">
<link rel="stylesheet" href="https://unpkg.com/@uncinq/css-components/css/index.css">
```

---

## File structure

```text
css/
  index.css               ← imports all component files
  component/
    alert.css             ← alert / notification banner
    badge.css             ← badge / pill / tag
    banner.css            ← full-width banner strip
    breadcrumb.css        ← breadcrumb navigation
    button.css            ← button (all variants)
    card.css              ← card container
    dropdown.css          ← dropdown menu
    embed.css             ← video / iframe embed wrapper
    list.css              ← styled list
    map.css               ← embedded map
    media.css             ← media object (image + text)
    nav.css               ← navigation bar
    nav-accessibility.css ← accessibility skip links / focus helpers
    offcanvas.css         ← off-canvas panel / drawer
    pagination.css        ← pagination control
```

---

## References

- [@uncinq/design-tokens](https://github.com/uncinq/design-tokens) — primitive + semantic layers
- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens) — component token layer
- [@uncinq/css-base](https://github.com/uncinq/css-base) — base CSS rules
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)
