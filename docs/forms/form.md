---
isIndex: false
title: Form
description: A responsive grid that is one column or two, never three, plus its layout helpers.
weight: 1
icon: columns-gap
---

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
    <p class="help" id="message-help">Markdown is supported.</p>
  </div>
  <div class="actions">
    <button class="btn" type="submit">Send</button>
  </div>
</form>
```

Each field is a plain `div`. There is no `.form-group` or `.field` class: the grid places its direct children, whatever they are.

## One column or two, never three

```css
grid-template-columns: repeat(
  auto-fill,
  minmax(max(var(--form-col-min-width, 25ch), calc(50% - var(--gap) / 2)), 1fr)
);
```

The `max()` is doing two jobs at once. It sets a **floor** of `25ch`, below which a field is too narrow to use, and it sets each track to at least half the width minus half the gap, which **caps** the result at two columns however wide the container gets.

A three-column form is almost never what you want, because it breaks the vertical reading order of a sequence of fields: the eye has to move right twice before dropping a line, and nothing in the layout says which order the fields belong in.

Change the floor with `--form-col-min-width`. Raising it past 50% forces a single column, which is a reasonable way to write a narrow form without a variant class.

## Layout helpers

| Class | Effect |
| --- | --- |
| `.full` | Spans both columns, `grid-column: 1 / -1` |
| `.actions` | Spans both columns and stacks its children vertically |
| `.help` | Help text under a field |

`.actions` is a column flex container aligned to the start, with `--gap` between its children and their block margins flattened. So a submit button, a consent line and a legal note stack in the order written without fighting each other's margins.

`.help` reads `--help-*` for colour, size, weight, margin and alignment. Pair it with `aria-describedby`, or it is seen and never announced.

## Field spacing

Both axes use `--gap`, the same token the grid uses for its column calculation. That is why the two-column cap subtracts `var(--gap) / 2`: the track width and the gutter come from one value, so changing it cannot desynchronise them.

## Controls are not styled here

Inputs, selects, textareas and their focus and invalid states come from [css-base](../../../css-base/base/#forms), applied to the native elements. This file adds layout and nothing else, which is why a form built without `.form` still looks right, it is simply stacked.
