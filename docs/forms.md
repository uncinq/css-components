---
isIndex: false
title: Forms
description: The .form grid, its layout helpers, and where the actual control styling lives.
weight: 7
icon: ui-checks
---

Form **controls** are styled by [@uncinq/css-base](../../css-base/base/#forms), on the native elements themselves. There is no `.input` or `.select` class to apply.

This package adds only what css-base cannot: the layout around those controls.

## `.form`

A responsive two-column grid that collapses to one column when there is not enough room.

```html
<form class="form">
  <div>
    <label for="first">First name</label>
    <input id="first" type="text">
  </div>
  <div>
    <label for="last">Last name</label>
    <input id="last" type="text">
  </div>
  <div class="full">
    <label for="message">Message</label>
    <textarea id="message"></textarea>
    <p class="help">Markdown is supported.</p>
  </div>
  <div class="actions">
    <button class="btn" type="submit">Send</button>
  </div>
</form>
```

The column rule is worth reading once:

```css
grid-template-columns: repeat(
  auto-fill,
  minmax(max(var(--form-col-min-width, 25ch), calc(50% - var(--gap) / 2)), 1fr)
);
```

The `max()` sets a floor of `25ch` **and** caps the result at half the width. The effect is a form that is one column or two, never three, however wide the container gets. A three-column form is almost never what you want, because it breaks the vertical reading order of a sequence of fields.

Change the floor with `--form-col-min-width`.

## Layout helpers

| Class | Effect |
| --- | --- |
| `.full` | Spans both columns, `grid-column: 1 / -1` |
| `.actions` | Spans both columns and stacks its children vertically |
| `.help` | Help text under a field |
| `.form-check` | A checkbox or radio laid out inline with its label |

```html
<div class="form-check">
  <input id="terms" type="checkbox">
  <label for="terms">I accept the terms</label>
</div>
```

`.form-check` is deliberately minimal, just `display: flex`. The checkbox and radio appearance, including the gap with the adjacent label, comes from css-base, which styles `[type='checkbox'] + label` directly.

## Accessibility

Nothing here supplies an accessible name. Every control needs a `<label for>` pointing at its `id`, or a `.visually-hidden` label when the design has no room for a visible one.

Tie `.help` text to its field with `aria-describedby`, otherwise it is visible but never announced:

```html
<input id="message" aria-describedby="message-help">
<p class="help" id="message-help">Markdown is supported.</p>
```

For error states, use `aria-invalid="true"` on the control and point `aria-describedby` at the error message.
