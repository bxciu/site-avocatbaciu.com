# Gold Glint Static — Design Spec

**Date:** 2026-05-08  
**Status:** Approved

## Summary

Add a static metallic streak effect to all gold-coloured elements in `index.html`. The effect mimics light catching polished gold — a bright highlight point fixed diagonally across the text and decorative elements. No animation. Medium intensity (~48% brightness at the highlight peak).

## Affected Elements

| Selector | Current state | Change |
|---|---|---|
| `.hero-name .baciu-gold` | vertical gradient, background-clip:text | diagonal 105° streak gradient |
| `.hero-name em` | vertical gradient, background-clip:text | same diagonal streak gradient |
| `.hero-stat-num` | flat `color: var(--gold)` | diagonal streak gradient, background-clip:text |
| `.hero-stat-num sup` | flat `color: var(--gold-light)` | inherits from parent via `-webkit-text-fill-color` |
| `.hero-divider` | left→transparent gradient | symmetric gradient with bright central spot |
| `.nav-brand` | flat `color: var(--gold-light)` | diagonal streak gradient, background-clip:text |

## CSS Implementation

### Streak gradient (text elements)

```css
background: linear-gradient(
  105deg,
  var(--gold-deep) 0%,
  var(--gold)      28%,
  #F5E9A0          48%,
  var(--gold)      64%,
  var(--gold-deep) 100%
);
-webkit-background-clip: text;
background-clip: text;
-webkit-text-fill-color: transparent;
```

`#F5E9A0` is the bright highlight point — warm pale gold, ~medium intensity. Angle 105° gives a natural top-left-to-bottom-right light direction.

### Divider streak

```css
background: linear-gradient(
  90deg,
  transparent      0%,
  var(--gold-deep) 15%,
  #F5E9A0          50%,
  var(--gold-deep) 85%,
  transparent      100%
);
```

No `background-clip` needed — the divider is a `<div>`, not text.

## Constraints

- CSS added as an override `<style>` block inside `index.html`, after the existing boost-color block. `style.css` is not modified.
- The same override block is also applied to `avocatbaciu-deploy-final-aria/index.html`.
- No JavaScript changes. No new files.
- Must not break the existing `drawLine` animation on `.hero-divider` (the `background` property override must be compatible with the animation — the animation only touches `opacity` and `transform`, so there is no conflict).
- `background-clip: text` requires `-webkit-` prefix for Safari compatibility — both prefixed and unprefixed versions are included.
