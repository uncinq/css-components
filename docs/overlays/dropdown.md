---
isIndex: false
title: Dropdown
description: A contextual menu positioned against its trigger, with a three-class transition contract.
weight: 4
icon: caret-down-square
---

A contextual menu. Unlike [`.modal`](../modal/) and [`.drawer`](../drawer/), it is not a panel: it is absolutely positioned against its trigger, stays out of the top layer, and has a tighter JavaScript contract because it animates in both directions.

```html
<div class="dropdown">
  <button class="dropdown-toggle js-dropdown-toggle" aria-expanded="false">Label</button>
  <ul class="dropdown-menu">
    <li><a href="#">Item</a></li>
    <li><hr></li>
    <li><a href="#">Item</a></li>
  </ul>
</div>
```

## Three classes, three jobs

| Class | Role |
| --- | --- |
| `.dropdown` | The positioning context, `inline-flex` and `position: relative` |
| `.dropdown-toggle` | The trigger, styled: transparent, with a chevron on `::after` |
| `.dropdown-menu` | The menu panel, absolutely positioned under the trigger |

The trigger needs **both** `.dropdown-toggle` for the styling and `.js-dropdown-toggle` for the script. They are separate on purpose, so a trigger can be a `.btn`, a [`.btn-share`](../../buttons/btn-share/) or a plain button and still be found by the same script.

**`.js-dropdown-toggle`'s next element sibling must be the `.dropdown-menu`.** That is how the script pairs the two; there is no `data-target` here.

## Items are styled by element, not by class

`.dropdown-menu` styles `li a` directly. There is no `.dropdown-item` rule in this package, and no `.dropdown-divider` rule either: a separator is a plain `<hr>` taking whatever [css-base](../../../css-base/base/) gives it. Adding those classes to your markup is harmless but changes nothing.

Links get `white-space: nowrap`, so the menu widens to its longest item rather than wrapping. `--dropdown-min-width` sets the floor.

## The chevron

Drawn on `::after` as a mask from `--dropdown-icon`, and rotated by the trigger's own ARIA state:

```css
&[aria-expanded="true"]::after { rotate: 180deg; }
```

So the arrow follows `aria-expanded` rather than a state class. If your script forgets to update the attribute, the menu opens with the chevron still pointing down, which makes the omission visible during development rather than only to screen reader users.

The trigger also lays a transparent `::before` over its whole positioning context, widening the click target beyond the text.

## The three-class transition contract

The script toggles three classes on the **menu**, in this sequence:

| Class | Meaning | Effect |
| --- | --- | --- |
| `.showing` | Opening transition has started | Visible and clickable, still at its closed opacity and offset |
| `.show` | Fully open | Opacity 1, translate 0 |
| `.hiding` | Closing transition has started | Still visible while it animates out |

```css
&.show, &.showing, &.hiding { pointer-events: auto; visibility: visible; }
&.show                      { opacity: 1; transform: translateY(0); }
```

The three exist so the menu can animate **out** before being removed from the flow. A single `.show` class would make the closing transition impossible: removing it would snap `visibility` to `hidden` on the same frame as the opacity change, and nothing would be seen.

The script must also keep `aria-expanded` on the toggle in sync with the state.

Transitions are inside `@media (prefers-reduced-motion: no-preference)`, so the three classes still work with motion disabled; they simply take effect instantly.

## Alignment

`.dropdown-menu-end` aligns the menu to the inline end of its trigger rather than the start. It is the only variant, and it goes on the menu, not on the container.

```html
<ul class="dropdown-menu dropdown-menu-end">...</ul>
```

## Inside a navigation

[`.nav`](../../navigation/nav/) restyles `.dropdown-toggle` to take the navigation's own text colour, so a dropdown in a header menu matches the links beside it without a variant class.

## It uses `z-index`, not the top layer

`.dropdown-menu` is positioned with `z-index: var(--dropdown-z-index)` inside its own stacking context. Unlike a panel, it can therefore be clipped by an ancestor with `overflow: hidden` or trapped under a sticky header. If that happens, the fix is the ancestor's overflow, not a higher `z-index`.
