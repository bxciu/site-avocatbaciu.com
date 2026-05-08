# Nav Hide on First Load / Show on Scroll-Up — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ascunde bara de navigație la primul acces pe site; o afișează la scroll în sus; o ascunde din nou la scroll în jos și când utilizatorul ajunge sus.

**Architecture:** Două blocuri inline noi în `avocatbaciu-glint/index.html`: un `<style>` care adaugă clasa `.nav--hidden` (transform + opacity + transition) și un `<script>` (înainte de `</body>`) cu logica scroll. `style.css` rămâne nemodificat.

**Tech Stack:** CSS pur + vanilla JS (scroll event + requestAnimationFrame throttle), fără librării.

---

## File Map

| Fișier | Acțiune |
|---|---|
| `C:\site-avocat.baciu-root\avocatbaciu-glint\index.html` | Modificat — `<style>` în `<head>` + `<script>` înainte de `</body>` |

---

## Task 1: CSS — clasa `.nav--hidden`

**Files:**
- Modify: `C:\site-avocat.baciu-root\avocatbaciu-glint\index.html`

Găsește în fișier blocul de glint `<style>` (după boost-color block). Inserează **după** ultimul `</style>` din `<head>` (înainte de `</head>`):

- [ ] **Step 1: Inserează blocul `<style>`**

```html
<!-- NAV HIDE-ON-LOAD / SHOW-ON-SCROLL-UP -->
<style>
  nav.site-nav {
    transition: transform 0.3s ease, opacity 0.3s ease,
                background 0.4s ease, border-color 0.4s;
  }
  nav.site-nav.nav--hidden {
    transform: translateY(-100%);
    opacity: 0;
    pointer-events: none;
  }
  @media (prefers-reduced-motion: reduce) {
    nav.site-nav { transition: none; }
  }
</style>
```

- [ ] **Step 2: Verifică că nu există deja `.nav--hidden` în fișier**

Caută string-ul `nav--hidden` — nu trebuie să existe înainte de această modificare.

---

## Task 2: JS — logica scroll

**Files:**
- Modify: `C:\site-avocat.baciu-root\avocatbaciu-glint\index.html`

Găsește `</body>` și inserează imediat **înainte** de el:

- [ ] **Step 1: Inserează blocul `<script>`**

```html
<!-- NAV HIDE-ON-LOAD / SHOW-ON-SCROLL-UP -->
<script>
(function () {
  'use strict';
  var nav = document.getElementById('siteNav');
  if (!nav) return;

  nav.classList.add('nav--hidden');

  var lastY = window.scrollY;
  var ticking = false;

  function onScroll() {
    var y = window.scrollY;
    if (y < 10) {
      nav.classList.add('nav--hidden');
    } else if (y > lastY) {
      nav.classList.add('nav--hidden');
    } else {
      nav.classList.remove('nav--hidden');
    }
    lastY = y;
    ticking = false;
  }

  window.addEventListener('scroll', function () {
    if (!ticking) {
      requestAnimationFrame(onScroll);
      ticking = true;
    }
  }, { passive: true });
})();
</script>
```

- [ ] **Step 2: Verifică că nav are `id="siteNav"`**

Caută `id="siteNav"` în index.html — trebuie să existe pe elementul `<nav>`. Dacă nav-ul are alt id sau nu are id, ajustează `document.getElementById(...)` corespunzător.

- [ ] **Step 3: Test manual în browser**

| Scenariu | Rezultat așteptat |
|---|---|
| Încărcare pagină (scrollY = 0) | Nav invizibil (opacity 0, translateY(-100%)) |
| Scroll jos 200px | Nav rămâne ascuns |
| Scroll sus 50px | Nav apare (slide in, 300ms) |
| Scroll înapoi la top (scrollY < 10) | Nav se ascunde din nou |
| Reduced-motion | Fără tranziție, show/hide instant |

---

## Task 3: Commit

- [ ] **Step 1: Commit**

```bash
git add avocatbaciu-glint/index.html
git commit -m "feat(glint): hide nav on load, show on scroll-up, hide at top"
```

---

## Self-Review

- ✅ Ascuns la încărcare — `nav.classList.add('nav--hidden')` imediat în IIFE
- ✅ Scroll jos → ascuns — ramura `y > lastY`
- ✅ Scroll sus → vizibil — ramura `else`
- ✅ La top (< 10px) → ascuns — ramura `y < 10` (prioritate față de direcție)
- ✅ Tranziție 300ms ease — în `<style>` override
- ✅ `style.css` nemodificat
- ✅ Fără librării externe
- ✅ Conflict cu `.hidden` existent: vechiul script scoate `.hidden` la scroll sus, noul script scoate `.nav--hidden` — cooperează natural
