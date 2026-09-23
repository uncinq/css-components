# @uncinq/css-components

> Framework-agnostic CSS component implementations.

<img width="1280" height="640" alt="share-css-components" src="https://github.com/user-attachments/assets/3d74df8a-e7bf-4ba5-8529-99cfef0bc176" />

29 components and 1 utility, written as plain CSS, reading their values from [@uncinq/component-tokens](https://github.com/uncinq/component-tokens) and adding none of their own.

## Where this sits

```
@uncinq/design-tokens      primitive + semantic values
@uncinq/component-tokens   component-scoped values
@uncinq/css-base           reset, native elements, layouts
@uncinq/css-components     ← this package
```

## Installation

All four packages are required. `@uncinq/css-base` is a genuine prerequisite but is **not** declared in `peerDependencies`, so install it explicitly.

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens @uncinq/css-base @uncinq/css-components
```

Declare the cascade layer order once at the top of your entry stylesheet, **before any import**:

```css
@layer reset, tokens, libs, vendors, base, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components, utilities */
```

Per component:

```css
@import '@uncinq/css-components/css/components/button.css';
@import '@uncinq/css-components/css/utilities/scrollsnap.css';
```

Without a build step:

```html
<link rel="stylesheet" href="https://unpkg.com/@uncinq/css-components">
```

## What's included

| Group | Components |
| --- | --- |
| Buttons | `.btn` with colour, size and style variants, plus `btn-close`, `btn-menu`, `btn-search`, `btn-filter`, `btn-share`, `btn-toc`, `btn-toggle-video` |
| Overlays | `modal`, `drawer`, `dropdown`, and the shared panel skeleton |
| Content | `alert`, `badge`, `banner`, `card`, `items`, `list`, `media`, `surtitle` |
| Navigation | `nav`, `nav-title`, `nav-accessibility`, `breadcrumb`, `pagination` |
| Embeds | `embed`, `video`, `map` |
| Forms | `form`, `form-check` and the layout helpers |
| Utilities | `scrollsnap` |

## Two import-order rules

`components/panel.css` is imported **last** among the components: its `.panel-inline-*` variants beat `.modal` and `.drawer` on source order, because most of the file is `:where()`-wrapped and carries no specificity.

`utilities/scrollsnap.css` comes after everything, in `@layer utilities`, because it has to win over the grid a component declares for itself.

If you import file by file rather than using the barrel, preserve both positions.

## Not included

- **No JavaScript.** `modal`, `drawer` and `dropdown` expect a script; each documents the classes and attributes it must toggle.
- **No `.item`.** This package ships `.card`, a complete implementation. `.item` lives in the Hugolify design system theme.
- **No pagination glyphs.** The theme supplies them through `::before` or `::after` content.

## Documentation

Full documentation: **[socle.uncinq.dev/docs/css-components/](https://socle.uncinq.dev/docs/css-components/)**

It is also versioned with the code in [`docs/`](docs/), and ships inside the npm package, so it is readable offline and from `node_modules`:

- [Cascade layers](docs/cascade-layers.md) — the two layers, and the `:where()` convention
- [Buttons](docs/buttons.md)
- [Overlays](docs/overlays.md) — the panel skeleton and the JS contracts
- [Content](docs/content.md)
- [Navigation](docs/navigation.md)
- [Embeds](docs/embeds.md)
- [Forms](docs/forms.md)
- [Utilities](docs/utilities.md) — `.scrollsnap` and the bleed contract

## References

- [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) — the values these components read
- [`@uncinq/css-base`](https://github.com/uncinq/css-base) — the foundation underneath
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)

## License

MIT © [Un Cinq](https://uncinq.dev/)
