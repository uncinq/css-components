---
isIndex: false
title: Alert
description: An inline notification block, with ten colour variants and an automatic horizontal layout when it leads with an icon.
weight: 2
icon: exclamation-triangle
---

An inline notification block.

```html
<div class="alert alert-info">
  <p class="alert-heading">Heads up</p>
  <p>Your changes were saved.</p>
</div>
```

## Variants

`.alert-primary`, `.alert-secondary`, `.alert-brand`, `.alert-neutral`, `.alert-dark`, `.alert-light`, `.alert-success`, `.alert-danger`, `.alert-warning`, `.alert-info`.

Each sets background, border and text colour from the matching `-muted` and `-strong` semantic tokens, so the pairing stays legible if the underlying colour changes.

```css
.alert-success {
  --alert-color-background: var(--color-success-muted);
  --alert-color-border: var(--color-success-muted);
  --alert-color-text: var(--color-success-strong);
}
```

`.alert-primary` and `.alert-brand` resolve identically, as do `.alert-secondary` and `.alert-neutral`. `.alert-dark` is the one exception to the muted pattern: it takes `--color-text-on-dark` rather than `--color-dark-strong`.

The base `.alert` declares no colours of its own, so an alert with no variant class shows whatever `--alert-color-*` resolves to in your token set.

## Links inside an alert

`--color-link` is reassigned to the alert's own text colour:

```css
.alert { --color-link: var(--alert-color-text); }
```

Without that, a link inside a tinted block would fall back to the page link colour, which is chosen for contrast against the page background rather than against a muted tint.

## Leading with an icon

An alert whose first child is an `.icon`, and which has no `.alert-heading`, switches to a horizontal layout on its own:

```css
&:has(.icon:first-child):not(:has(.alert-heading)) {
  flex-direction: row;
  align-items: flex-start;
}
```

```html
<div class="alert alert-warning">
  <span class="icon icon-triangle-alert"></span>
  <p>This action cannot be undone.</p>
</div>
```

With a heading present the alert stays stacked, because the icon then belongs beside the heading rather than beside the whole block. `.alert-heading` is itself a flex row with `--spacing-xs` between its children, so an icon inside the heading lines up with the text.

## `.container` inside an alert

`.container` inherits the same flex layout as the alert itself:

```css
&,
.container {
  display: flex;
  flex-direction: var(--alert-direction, column);
  gap: var(--alert-gap);
}
```

That is what lets a full-bleed alert keep its content aligned with the rest of the page: the alert spans the viewport, the container inside it caps the measure, and both lay their children out the same way.

## Hiding an alert

`.alert` declares `display: flex`, which would otherwise beat the user agent rule for `[hidden]`. The file restores it:

```css
.alert[hidden] { display: none; }
```

So a dismissible alert can be hidden with the attribute alone, with no state class to manage:

```js
alert.hidden = true;
```

## Margins

`.alert *` sets `margin-block: 0` on every descendant, so paragraphs inside an alert are spaced by the flex `gap` rather than by their own margins. Vertical rhythm around the alert itself comes from `--alert-margin`.
