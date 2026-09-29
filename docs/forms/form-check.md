---
isIndex: false
title: Check row
description: A checkbox or radio laid out beside its label, and why the class is only one declaration.
weight: 2
icon: check-square
---

A checkbox or radio laid out inline with its label.

```html
<div class="form-check">
  <input id="terms" type="checkbox">
  <label for="terms">I accept the terms</label>
</div>
```

## The whole component

```css
.form-check { display: flex; }
```

That is the entire rule, and it is deliberate. The control's appearance, its size, and the gap between it and the adjacent label all come from [css-base](../../../css-base/base/#forms), which styles `[type='checkbox'] + label` directly on the native elements.

So the only thing missing from a bare input and label is that they sit on separate lines by default. This class fixes that, and stays out of everything else.

The practical consequence: if the spacing between a checkbox and its label is wrong, the fix is in css-base or in the tokens, not here. Adding a `gap` to `.form-check` would apply on top of the one css-base already sets, and the two would drift apart.

## The label must come after the control

`[type='checkbox'] + label` is an adjacent sibling selector. Wrapping the input inside the label, or putting the label first, means the control gets no spacing and no styling from css-base.

```html
<!-- works -->
<div class="form-check">
  <input id="terms" type="checkbox">
  <label for="terms">I accept the terms</label>
</div>

<!-- does not -->
<div class="form-check">
  <label for="terms">I accept the terms</label>
  <input id="terms" type="checkbox">
</div>
```

## Inside a form grid

A check row is a direct child of [`.form`](../form/) like any other field, so it takes a grid track. Give it `.full` when the label is long enough to wrap in half the width:

```html
<div class="form-check full">
  <input id="newsletter" type="checkbox">
  <label for="newsletter">Send me the monthly newsletter</label>
</div>
```

## Groups of radios

There is no class for the group. Use a `<fieldset>` with a `<legend>`, and one `.form-check` per option:

```html
<fieldset class="full">
  <legend>Preferred contact</legend>
  <div class="form-check">
    <input id="by-email" type="radio" name="contact" value="email">
    <label for="by-email">Email</label>
  </div>
  <div class="form-check">
    <input id="by-phone" type="radio" name="contact" value="phone">
    <label for="by-phone">Phone</label>
  </div>
</fieldset>
```

The legend is what names the group in the accessibility tree. A set of radios with individual labels and no legend is announced one option at a time, with nothing saying what is being chosen.
