---
isIndex: false
title: Buttons
description: The .btn base class with its colour, size and style variants, and the seven specialised buttons built on it.
weight: 2
icon: hand-index
---

## `.btn`

The base class. Works on `<button>`, `<a>` and `<input type="submit">`, and reads `--btn-*` from [@uncinq/component-tokens](../../component-tokens/reference/).

```html
<button class="btn">Send</button>
<a class="btn btn-secondary" href="/contact">Contact</a>
```

### Colour variants

| Class | Intent |
| --- | --- |
| `.btn-primary` | Primary action |
| `.btn-secondary` | Secondary action |
| `.btn-brand` | Brand-coloured |
| `.btn-neutral` | Neutral emphasis |
| `.btn-dark` | Dark surface |
| `.btn-light` | Light surface |
| `.btn-success` | Confirmation |
| `.btn-danger` | Destructive action |
| `.btn-warning` | Warning |
| `.btn-info` | Informational |

Each variant sets `--btn-color-background`, `--btn-color-border` and `--btn-color-text` together. The text colour always comes from the matching `--color-text-on-*` semantic token, so contrast is preserved when the underlying colour is overridden. That pairing is the reason to change `--color-brand` rather than `--btn-color-background` when rebranding.

### Sizes

| Class | Use |
| --- | --- |
| `.btn-xs` | Dense UI, inline controls |
| `.btn-sm` | Compact |
| `.btn-md` | Default, can be omitted |
| `.btn-lg` | Prominent |
| `.btn-xl` | Hero call to action |

### Style variants

| Class | Effect |
| --- | --- |
| `.btn-ghost` | Transparent background, visible border |
| `.btn-link` | Renders as a link, no background or border |
| `.btn-control` | Form control styling, transparent border, for buttons sitting next to inputs |

Variants compose: `class="btn btn-danger btn-sm btn-ghost"`.

## Specialised buttons

Seven single-purpose buttons. Read the second column before using one: whether `.btn` is required differs between them.

| Class | Needs `.btn` | Icon | Purpose |
| --- | --- | --- | --- |
| `.btn-close` | no | `--icon-close` | Close control, used inside panels and alerts |
| `.btn-menu` | no | `--icon-menu` | Menu toggle, typically the burger in a header |
| `.btn-search` | yes | `--icon-search` | Search toggle |
| `.btn-filter` | yes | `--icon-filter` | Filters toggle |
| `.btn-share` | yes | `--icon-share` | Share control |
| `.btn-toc` | yes | `--icon-toc` | Table of contents toggle |
| `.btn-toggle-video` | yes | child elements | Play and pause control overlaid on a video |

```html
<!-- self-contained -->
<button class="btn-close" aria-label="Close"></button>

<!-- modifier on .btn -->
<button class="btn btn-search" aria-label="Search"></button>
```

`.btn-close` and `.btn-menu` redeclare everything they need, with `--btn-close-*` and `--btn-menu-*` falling back to the matching `--btn-*` token. They therefore look consistent with `.btn` without depending on it. The other five are modifiers, and omitting the base class leaves them largely unstyled.

Six of the seven **do ship their icon**, drawn as a `mask` on `::before` from the icon tokens in [@uncinq/design-tokens](../../design-tokens/reference/#icon). Because it is a mask rather than a background image, the glyph takes `currentColor` and follows the button's text colour through every state. Size it with `--icon-size`, or per button with `--btn-close-icon-size` and its siblings.

`.btn-toggle-video` is the exception and expects two child elements, for the reason given below.

When a button has no visible text, give it an accessible name with `aria-label`, or a `.visually-hidden` span from [css-base](../../css-base/base/#accessibility-helpers). None of these buttons provides one for you.

## `.btn-toggle-video`

The one worth explaining, because its behaviour is driven by a state class rather than by CSS alone.

It is a play and pause control overlaid on a video that autoplays without native controls, which WCAG requires to be pausable. It stays transparent so the video shows through, and the `.is-playing` class, toggled by whatever script drives the video, decides which of the two icons is shown.

```html
<div class="video">
  <video autoplay muted loop playsinline></video>
  <button class="btn btn-toggle-video is-playing" aria-label="Pause">
    <span class="icon-pause"></span>
    <span class="icon-play"></span>
  </button>
</div>
```

The `.video` wrapper is what positions it. See [Embeds](../embeds/).

Your script owns two things: toggling `.is-playing` in step with the video's actual state, and keeping the accessible name in sync, since the control means "pause" while playing and "play" while paused.
