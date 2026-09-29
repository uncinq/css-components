---
isIndex: false
title: Overlays
description: The shared panel skeleton behind .modal and .drawer, the two opening strategies, and the .dropdown contract.
weight: 3
icon: window-stack
---

Four files, and a design worth understanding before reading any of them.

| Page | Covers |
| --- | --- |
| [Panel](panel/) | The shared skeleton, `.panel-inline-*`, `.panel-trigger-*`, the backdrop and the JavaScript contract |
| [Modal](modal/) | `.modal`, the centred configuration |
| [Drawer](drawer/) | `.drawer` and its four edge variants |
| [Dropdown](dropdown/) | `.dropdown`, a contextual menu with its own contract |

## `.modal` and `.drawer` are the same panel

Same structure, header and content and footer. Same open and closed contract. Same backdrop. They differ only in where they sit: modal centres them, drawer pins them to an edge.

`components/panel.css` owns **every rule**. The two variants are pure configuration, declaring nothing but `--panel-*` tokens.

There is **no `.panel` class to apply.** The file targets `:where(.modal, .drawer)` directly, so the skeleton arrives with either variant class. `panel` names the shared concept and the token namespace, not a class.

Each variant aliases `--panel-*` from its own namespace, so `--modal-*` and `--drawer-*` remain the public API. Override those.

## The tag picks the strategy, not the class

Either variant works both ways, and the choice is an accessibility one rather than a styling one. See [Two ways to open a panel](panel/#two-ways-to-open-a-panel).

## No JavaScript ships here

`.modal`, `.drawer` and `.dropdown` all expect a script. The CSS defines the classes and attributes that script must toggle; each page documents the contract it needs.
