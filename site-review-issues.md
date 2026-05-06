# Site Review Issues — avocatbaciu.com
**Fișier:** `index.html` | **Data:** 2026-05-06

---

## 🔴 HIGH

1. **Nav în interiorul `<main>`** (linia 1539) — `<nav class="site-nav">` se află după `<main id="continut-principal">`. Nav-ul principal trebuie să fie înaintea lui `<main>`, la nivel de `<body>`.

2. **`aria-controls="navLinks"` referențiază ID inexistent** (linia 1548) — Butonul toggle al meniului mobil are `aria-controls="navLinks"` dar `<ul class="nav-links">` nu are `id="navLinks"` nicăieri în fișier.

3. **Skip link greșit** (linia 1533) — `<a href="#despre" class="skip-link">` sare la secțiunea "Despre" în loc de `<main id="continut-principal">`.

4. **Ierarhie heading incorectă** (liniile 1662–1684) — Ordinea în DOM este H1 → H3 → H3 → H3 → H2 în secțiunea "Despre". H3-urile cardurilor apar înaintea H2-ului secțiunii.

5. **Lipsesc favicon și apple-touch-icon** — Nu există `<link rel="icon">` sau `<link rel="apple-touch-icon">` în `<head>`. Browserul face cerere 404 la `/favicon.ico`.

6. **Programul de lucru nu este vizibil pe pagină** — Luni-Vineri 09:00-18:00 există doar în JSON-LD (linia 1438) și FAQ, dar nu apare nicăieri vizibil în secțiunea de contact.

7. **Typo: "consilul"** (linia 1846) — `"consilul de părinți"` trebuie `"consiliul de părinți"`.

8. **Focus trap lipsă la meniul mobil** (liniile 2158–2170) — La deschiderea meniului mobil, navigarea cu Tab poate ieși din meniu pe conținutul ascuns din spate. Lipsește focus trap.

9. **JSON-LD al 4-lea injectat prin JS — tip inexistent** (linia 2184) — Scriptul injectează dinamic un bloc JSON-LD de tip `Attorney` (tip invalid în Schema.org). Google nu indexează JSON-LD injectat prin JS. Duplicează date deja prezente în JSON-LD static.

---

## 🟡 MEDIUM

10. **`<nav>` fără `aria-label`** (linia 1539) — `<nav class="site-nav">` nu are `aria-label` sau `aria-labelledby`.

11. **`<aside>` fără `aria-label`** (linia 1906) — `<aside class="contact-panel">` nu are label descriptiv pentru screen readere.

12. **Texte ancoră ambigue** (linia 1669) — `<a>Vezi mai mult</a>` pe link-ul spre baroul-cluj.ro nu este descriptiv fără context vizual.

13. **Inconsistență "Romania" vs "România"** — Linia 1947 (footer): `Romania` fără diacritică. Linia 1873 (contact): `România` cu diacritică.

14. **`sameAs` lipsește din JSON-LD** — Entitățile `LegalService` și `Person` nu conțin `sameAs` cu link spre profilul public pe baroul-cluj.ro.

15. **CTA "Programare consultanță" dispare pe mobil** (linia 1327) — `.nav-cta { display: none }` pe mobile fără nicio alternativă. Singurul CTA rămâne cel din hero, care poate fi depășit prin scroll.

16. **`will-change` pe prea multe elemente** (linia 157) — `will-change: opacity, transform` aplicat pe toate elementele `.reveal` și `.tx-word` (sute de elemente). Presiune mare pe memoria GPU pe dispozitive mobile.

17. **`<article>` în timeline fără heading** (liniile 1760–1811) — Cardurile din secțiunea "Parcurs" folosesc `<article class="flow-card">` cu `<div class="flow-event">` în loc de `<h3>`.

18. **Footer — coloana "Navigare" fără `<nav>`** (linia 1933) — Lista de linkuri din footer este în `<div class="footer-col">` fără tag `<nav>`.

19. **Contradicție `aria-hidden` vs `aria-label` pe SVG** (linia 1600) — Containerul `<div class="hero-right" aria-hidden="true">` ascunde complet SVG-ul interior care are `role="img"` și `aria-label="Sigiliu Cabinet S.M.B."`.

20. **Animații contoare prea lente pentru valori mici** (linia 1995) — `durationFor()` alocă 3600ms pentru target `3` (grade de jurisdicție). La 60fps = 216 frame-uri pentru 3 incremente vizuale.

---

## ⚪ LOW

21. **`og:image` și `twitter:image` lipsesc** — Amânat conștient până la pregătirea imaginii 1200×630px.

22. **`<blockquote>` fără `cite` și fără atribuire vizibilă** (linia 1688) — Citatul nu are atribut `cite` și nu este atribuit vizibil niciunei persoane.

23. **`minimum-scale` lipsește din viewport meta** (linia 5) — Pe unele versiuni iOS poate permite zoom accidental pe inputs (risc scăzut, nu există formulare).

24. **Animațiile `pulse` și `scrollLine` nu sunt oprite explicit** pentru `prefers-reduced-motion` — Se bazează pe regula globală de reset, fragilă la refactorizări viitoare.

25. **Verificat canonical și og:url folosesc `www.`** — Dacă serverul nu redirecționează `avocatbaciu.com` → `www.avocatbaciu.com` cu 301, există risc de conținut duplicat indexat de Google.

---

## Note tehnice adiționale

- **CSS inline 55KB** — Întreg stilul este în `<style>` inline (liniile 27–1404), nu cacheable de browser. Mutarea într-un fișier extern `style.css` ar elimina 55KB la vizitele repetate.
- **Fonturi Google — 18 variante** (linia 24) — Playfair Display, EB Garamond, Cormorant Garamond cu toate weight-urile. `font-weight: 800` pentru Playfair Display nu este folosit în CSS — poate fi eliminat din request.
- **`splitIntoWords` JS** (linia 2068) — Folosește `textContent` pentru a reconstrui text, distruge orice markup HTML interior (`<strong>`, `<a>`, `<em>`) din elementele selectate.
