---
isIndex: false
title: Rating
description: A star rating out of 5 with fractional values, how each star is filled through its icon mask, and what the markup has to provide.
weight: 10
icon: star-half
---

A star rating out of 5, fractional values included: 4.8 shows four full stars and one filled to 80%.

```html
<div class="rating">
  <span class="rating-star" style="--rating-fill: 100%"><i class="icon icon-star" aria-hidden="true"></i></span>
  <span class="rating-star" style="--rating-fill: 100%"><i class="icon icon-star" aria-hidden="true"></i></span>
  <span class="rating-star" style="--rating-fill: 100%"><i class="icon icon-star" aria-hidden="true"></i></span>
  <span class="rating-star" style="--rating-fill: 100%"><i class="icon icon-star" aria-hidden="true"></i></span>
  <span class="rating-star" style="--rating-fill: 80%"><i class="icon icon-star" aria-hidden="true"></i></span>
  <span class="visually-hidden">Rating: 4.8 out of 5</span>
</div>
```

| Child | Role |
| --- | --- |
| `.rating-star` | One star, carrying its own fill in `--rating-fill`, from `0%` to `100%` |
| `.icon` | The star glyph, rendered by the theme |
| `.visually-hidden` | The value, read out by screen readers |

The fill is computed where the markup is written, not here: star *n* takes `(rating − n + 1) × 100%`, clamped between `0%` and `100%`. A star with no `--rating-fill` is empty.

## The fill goes through the mask

This package ships no star. The `.icon` is the theme's, and the rating expects it to be a **mask**: an element whose `background` shows through the glyph, as the Hugolify icons render.

```css
.rating-star .icon {
  background: linear-gradient(
    to right,
    var(--rating-color-active) var(--rating-fill, 0%),
    var(--rating-color-empty) var(--rating-fill, 0%)
  );
}
```

Both colour stops sit at the same position, so the gradient is a hard edge, not a blend: the glyph is in the active colour up to its fill and in the empty colour after it. With an outline icon set, such as Lucide, that is the outline which changes colour, not the inside of the star.

An icon inlined as an `<svg>` ignores the background and is not filled.

Under `:dir(rtl)` the glyph is mirrored, so stars fill from the right.

## Size and alignment

The rating sets `--icon-size` from `--rating-size`, so the stars follow the rating rather than the font size around them.

It is a flex row aligned to the start. `--rating-justify-content` moves it, which is how a centred context, a quote for instance, centres its stars without a modifier:

```css
.quote .rating {
  --rating-justify-content: center;
}
```

`--rating-gap` is `0` by default: the star glyph carries its own inner margin, and a gap would only widen the row.

## Accessibility

The stars carry no meaning for assistive technology: each `.icon` is `aria-hidden`, and the value is given as text in a `.visually-hidden` span, in words rather than as a fraction.

The empty colour defaults to `--color-text-muted` rather than a border colour, so an empty star stays visible on a muted background such as a card.
