---
isIndex: false
title: Overview
description: The one utility in the package, and the test for whether anything else belongs here.
weight: 1
icon: book
---

One utility, in its own layer.

| Page | Class | Purpose |
| --- | --- | --- |
| [Scrollsnap](../scrollsnap/) | `.scrollsnap` | Turns a grid into a horizontal snap carousel |

## Why it is not a component

`.scrollsnap` changes the `grid-template-columns` that a component such as [`.items`](../../content/items/) declares for itself in `@layer components`. A later layer beats an earlier one regardless of specificity, so putting it in `@layer utilities` is what lets a single class opt an existing component into carousel behaviour without rewriting the component.

That is also the test for anything else that might land here: a utility is a **modifier applied to something else**, not a thing of its own. If it has its own markup, it is a component.

`@layer utilities` must come last in your layer declaration for this to hold. See [Cascade layers](../../cascade-layers/).
