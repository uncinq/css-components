---
isIndex: false
title: Cascade layers
description: The two layers this package owns, why panel.css is imported last, and how to override a component cleanly.
weight: 1
icon: stack
---

This package writes to two layers.

| Layer | Contents |
| --- | --- |
| `@layer components` | All 29 components |
| `@layer utilities` | `.scrollsnap`, the only utility |

The full order, including layers owned by the sibling packages and by the project itself, is the **consuming project's** responsibility. Declare it once, at the very top of your entry stylesheet, before any `@import`:

```css
@layer reset, tokens, libs, base, vendors, layouts, components, pages, utilities;
```

CSS fixes a layer's position the first time its name is seen, and later re-declarations do not reorder it. If that line comes after an import, it is already too late. See [the css-base page](../../css-base/cascade-layers/) for the full explanation, including why `libs` and `vendors` are a pair.

## Why `utilities` is last

`.scrollsnap` changes the `grid-template-columns` that a component such as `.items` declares for itself in `@layer components`. A later layer beats an earlier one regardless of specificity, so putting it in `utilities` is what lets a single class opt an existing component into carousel behaviour without rewriting the component.

This is also why it is a utility rather than a component rule: it is a modifier applied to something else, not a thing of its own.

## Why `panel.css` is imported last

Inside `@layer components`, source order breaks ties between rules of equal specificity. Two facts combine here:

1. Most of `panel.css` is wrapped in `:where()`, so it carries **zero specificity**. That is deliberate: it lets `.modal`, `.drawer` and your own overrides win without escalating.
2. The `.panel-inline-*` variants must beat `.modal` and `.drawer`, and they are plain class selectors.

Because those variants have no specificity advantage over the host components, they rely on arriving later in the file. Hence `@import 'components/panel.css';` sits last among the component imports in `css/index.css`.

If you import files individually, keep `panel.css` after `modal.css` and `drawer.css`.

## Overriding a component

Write to the same layer after the imports:

```css
@layer components {
  .alert {
    --alert-border-radius: 0;
  }
}
```

Prefer redefining the token over rewriting the declaration. Almost every value in this package comes from a `--component-*` custom property, so an override usually needs no selector work at all.

Better still, when the change is contextual rather than global, set the property on an ancestor and let it inherit:

```css
.sidebar {
  --alert-padding-inline: var(--spacing-sm);
}
```

That is a normal declaration on a normal selector, competing on specificity like any other rule, not on layer order. It restyles a region without adding a variant class to the system.

## Overriding a `:where()` rule

Because `panel.css` uses `:where()` throughout, its rules have zero specificity and a single class selector of yours already outranks them:

```css
.modal {
  border-radius: 0;
}
```

No `!important`, no repeated class, no `:is()` trick. This is the main practical benefit of the `:where()` convention, and it is worth preserving if you contribute to the package.
