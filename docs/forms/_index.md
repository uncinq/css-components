---
isIndex: false
title: Forms
description: The .form grid, the .form-check row, and where the actual control styling lives.
weight: 7
icon: ui-checks
---

Form **controls** are styled by [@uncinq/css-base](../../css-base/base/#forms), on the native elements themselves. There is no `.input` or `.select` class to apply.

This package adds only what css-base cannot: the layout around those controls.

| Page | Class | Covers |
| --- | --- | --- |
| [Form](form/) | `.form` | The two-column grid and its layout helpers |
| [Check row](form-check/) | `.form-check` | A checkbox or radio laid out beside its label |

Both live in `components/form.css`.

## Accessibility

Nothing here supplies an accessible name. Every control needs a `<label for>` pointing at its `id`, or a `.visually-hidden` label when the design has no room for a visible one.

Tie `.help` text to its field with `aria-describedby`, otherwise it is visible but never announced:

```html
<input id="message" aria-describedby="message-help">
<p class="help" id="message-help">Markdown is supported.</p>
```

For error states, use `aria-invalid="true"` on the control and point `aria-describedby` at the error message.
