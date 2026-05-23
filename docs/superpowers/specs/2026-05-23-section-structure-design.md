# Design: Section-rule Pruning & Structural Breaks

**Date:** 2026-05-23  
**Status:** Implemented

## Problem

Site-ul prezenta pattern-ul repetat `eyebrow + H2 + section-rule (64px gold bar)` identic în toate 5 secțiunile non-hero (Despre, Activitate, Parcurs, Civic, Contact). Conform regulii §7 din `no-ai-slop.md`, același divider repetat pe fiecare secțiune este un semnal clar de template AI ("mechanical consistency"). Regula permite maxim 2 separatoare per pagină.

## Soluție (Abordarea A — Chirurgical)

### `section-rule` pruning

| Secțiune | Înainte | După | Rațiune |
|---|---|---|---|
| Despre | ✅ păstrat | ✅ păstrat | Prima secțiune de conținut, ritm necesar |
| Activitate | ✅ păstrat | ✅ păstrat | Introduce cardurile de servicii |
| Parcurs | ✅ era | ❌ eliminat | Timeline-ul vizual e suficient de dominant |
| Civic | ✅ era | ❌ eliminat | Background distinct + grain fac deja separarea |
| Contact | ✅ era | ❌ eliminat | Secțiunea redesenată (vezi mai jos) |

**Rezultat:** 2 `section-rule` pe pagină (față de 5).

### Restructurare Contact

**Eliminat:** `<p class="section-label">Luați legătura</p>` + `<h2 class="section-title">Date de <em>contact</em></h2>` + `<div class="section-rule">`.

**Înlocuit cu:** `<h2 class="contact-heading">Hai să vorbim.</h2>`

Stilizare: Playfair Display italic, `clamp(2.4rem, 4.5vw, 3.8rem)`, verde închis. Ton direct, fără structura de secțiune standard.

### Civic — eliminare bar

`section-rule` eliminat. `section-label` + H2 rămân — textul "Dincolo de sala de judecată" e suficient de specific să susțină secțiunea singur.

## Fișiere modificate

- `index.html` — eliminare `section-rule` din Parcurs, Civic, Contact; înlocuire header Contact
- `style.css` — adăugat `.contact-heading` styles

## Verificare

```js
document.querySelectorAll('.section-rule').length // → 2
document.querySelector('.contact-heading').textContent // → "Hai să vorbim."
document.querySelector('#civic .section-rule') // → null
document.querySelector('#parcurs .section-rule') // → null
```
