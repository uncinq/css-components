---
isIndex: false
title: Banner
description: A centred modifier on .alert, for site-wide announcements.
weight: 4
icon: megaphone
---

A thin variation on the [alert](../alert/), centred rather than start-aligned.

```html
<div class="alert banner">
  <p>Site-wide announcement.</p>
</div>
```

**It is a modifier, so use both classes.** `.banner` on its own has no background, no border and no padding.

## The whole component

```css
.banner {
  --alert-align: center;

  p { margin-block: 0; }
}
```

That is the entire file. `--alert-align` feeds both `align-items` and `text-align` in the alert rule, so one token centres the block on both axes at once. The paragraph reset flattens the margins that would otherwise show up when the banner holds a single line.

## Variants come from the alert

Every `.alert-*` colour works here, because the variant only sets `--alert-color-*` tokens and the banner only sets alignment:

```html
<div class="alert alert-warning banner">
  <p>Scheduled maintenance on Sunday.</p>
</div>
```

## Full-bleed announcements

A banner is usually the one alert that spans the viewport. Put a `.container` inside it rather than constraining the banner itself: the alert passes its flex layout down to `.container`, so the announcement stays aligned with the rest of the page while the tint runs edge to edge. See [the alert page](../alert/#container-inside-an-alert).
