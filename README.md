# @uncinq/css-components

> Framework-agnostic CSS component implementations.

<img width="1280" height="640" alt="share-css-components" src="https://github.com/user-attachments/assets/3d74df8a-e7bf-4ba5-8529-99cfef0bc176" />

30 components and 1 utility, written as plain CSS, reading their values from [@uncinq/component-tokens](https://github.com/uncinq/component-tokens) and adding none of their own.

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
@layer reset, tokens, libs, base, vendors, layouts, components, pages, utilities;

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

### A CDN link is not enough

`@uncinq/design-tokens` and `@uncinq/component-tokens` can be linked straight from a CDN, because they emit nothing but custom properties. **This package cannot**, and neither can `@uncinq/css-base`.

Both ship `@media (--sm)` queries that rely on the `@custom-media` rules declared in `css-base/css/mediaqueries.css`, and no browser implements `@custom-media`. Loaded without [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media), 13 responsive blocks in this package are dropped: `.panel-inline-*`, `.panel-trigger-*` and every `.scrollsnap-*` variant.

The failure is silent. No error is raised, the page simply stays at its mobile values on every screen. A build step is required.

## What's included

| Group | Components |
| --- | --- |
| Buttons | `.btn` with colour, size and style variants, plus `btn-close`, `btn-menu`, `btn-search`, `btn-filter`, `btn-share`, `btn-toc`, `btn-toggle-video` |
| Overlays | `modal`, `drawer`, `dropdown`, and the shared panel skeleton |
| Content | `alert`, `badge`, `banner`, `card`, `items`, `list`, `media`, `push`, `surtitle` |
| Navigation | `nav`, `nav-title`, `nav-accessibility`, `breadcrumb`, `pagination` |
| Embeds | `embed`, `video`, `map` |
| Forms | `form`, `form-check` and the layout helpers |
| Utilities | `scrollsnap` |

## Two import-order rules

`components/panel.css` must come **after** `modal.css` and `drawer.css`: its `.panel-inline-*` variants beat them on source order, because most of the file is `:where()`-wrapped and carries no specificity. Alphabetical order already gives that in `css/index.css`.

`utilities/scrollsnap.css` comes after everything, in `@layer utilities`, because it has to win over the grid a component declares for itself.

If you import file by file rather than using the barrel, preserve both positions.

## Not included

- **No JavaScript.** `modal`, `drawer` and `dropdown` expect a script; each documents the classes and attributes it must toggle.
- **No `.item`.** This package ships `.card`, a complete implementation. `.item` lives in the Hugolify design system theme.
- **No `.icon` glyphs.** Buttons and `pagination` mask their own from icon tokens, but an `.icon` element you place inside an `alert` or a video toggle is the theme's to render.

## Documentation

Full documentation: **[socle.uncinq.dev/docs/css-components/](https://socle.uncinq.dev/docs/css-components/)**

It is also versioned with the code in [`docs/`](docs/), one page per component, and ships inside the npm package, so it is readable offline and from `node_modules`:

- [Overview](docs/overview.md) — installation, import order, what the package leaves out
- [Cascade layers](docs/cascade-layers.md) — the two layers, and the `:where()` convention
- [Buttons](docs/buttons/) — [`.btn`](docs/buttons/btn.md) and the seven specialised buttons
- [Overlays](docs/overlays/) — [the panel skeleton](docs/overlays/panel.md), [`.modal`](docs/overlays/modal.md), [`.drawer`](docs/overlays/drawer.md), [`.dropdown`](docs/overlays/dropdown.md)
- [Content](docs/content/) — [`.alert`](docs/content/alert.md), [`.badge`](docs/content/badge.md), [`.banner`](docs/content/banner.md), [`.card`](docs/content/card.md), [`.items`](docs/content/items.md), [`.list`](docs/content/list.md), [`.media`](docs/content/media.md), [`.surtitle`](docs/content/surtitle.md)
- [Navigation](docs/navigation/) — [`.nav`](docs/navigation/nav.md), [`.nav-title`](docs/navigation/nav-title.md), [skip links](docs/navigation/nav-accessibility.md), [breadcrumb](docs/navigation/breadcrumb.md), [pagination](docs/navigation/pagination.md)
- [Embeds](docs/embeds/) — [`.embed`](docs/embeds/embed.md), [`.video`](docs/embeds/video.md), [`.map`](docs/embeds/map.md)
- [Forms](docs/forms/) — [`.form`](docs/forms/form.md), [`.form-check`](docs/forms/form-check.md)
- [Utilities](docs/utilities/) — [`.scrollsnap`](docs/utilities/scrollsnap.md) and the bleed contract

## References

- [`@uncinq/component-tokens`](https://github.com/uncinq/component-tokens) — the values these components read
- [`@uncinq/css-base`](https://github.com/uncinq/css-base) — the foundation underneath
- [MDN: CSS cascade layers](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers)

## License

MIT © [Un Cinq](https://uncinq.dev/)
