# Gold Glint Static Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Adaugă un efect de glint metalic static (streak diagonal 105°) pe toate elementele aurii din `index.html`, fără animație, intensitate medie.

**Architecture:** Un singur bloc `<style>` nou adăugat în `index.html` după blocul boost-color existent (linia 66). Override-urile suprascriu doar selectoarele afectate din `style.css` — fișierul `style.css` nu se modifică. Același bloc se copiază în `avocatbaciu-deploy-final-aria/index.html`.

**Tech Stack:** CSS pur — `linear-gradient`, `background-clip: text`, `-webkit-text-fill-color`. Fără JS, fără librării.

---

## File Map

| Fișier | Acțiune |
|---|---|
| `C:\site-avocat.baciu-root\index.html` | Modificat — se inserează bloc `<style>` după linia 66 |
| `C:\site-avocat.baciu-root\avocatbaciu-deploy-final-aria\index.html` | Modificat identic — același bloc `<style>` |

---

## Task 1: Adaugă blocul CSS de glint în `index.html`

**Files:**
- Modify: `C:\site-avocat.baciu-root\index.html` după linia 66 (după `</style>` al boost-color block)

- [ ] **Step 1: Inserează blocul `<style>` de glint**

Găsește în `index.html` linia care conține exact:
```html
</style>
<script type="application/ld+json">
```
și inserează imediat după primul `</style>` blocul următor:

```html
<style>
  /* ── GOLD GLINT — streak metalic static, fără animație ── */

  /* Gradient reutilizabil via custom property */
  :root {
    --glint-text: linear-gradient(
      105deg,
      var(--gold-deep) 0%,
      var(--gold)      28%,
      #F5E9A0          48%,
      var(--gold)      64%,
      var(--gold-deep) 100%
    );
  }

  /* Numele avocatei — span.baciu-gold și em italic */
  .hero-name .baciu-gold,
  .hero-name em {
    background: var(--glint-text);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  /* Numerele din statistici */
  .hero-stat-num {
    background: var(--glint-text);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    color: transparent; /* fallback pentru browsere fără background-clip:text */
  }

  /* sup (+) din statistici — moștenește fill-ul, dar are nevoie de propriul background */
  .hero-stat-num sup {
    background: var(--glint-text);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  /* Divider-ul decorativ — nu e text, nu necesită background-clip */
  .hero-divider {
    background: linear-gradient(
      90deg,
      transparent      0%,
      var(--gold-deep) 15%,
      #F5E9A0          50%,
      var(--gold-deep) 85%,
      transparent      100%
    );
  }

  /* Brand-ul din nav */
  .nav-brand {
    background: var(--glint-text);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
</style>
```

- [ ] **Step 2: Verifică vizual în browser**

Deschide `index.html` direct în browser (sau preview). Verifică:
- Numele „Simona Maria Baciu" — textul auriu are un punct luminos diagonal
- Numerele 22+, 1000+, 3 — același efect de streak
- Divider-ul orizontal — luminos la mijloc, fade spre capete
- Nav brand (sus) — streak vizibil la hover și în repaus

Dacă ceva apare alb sau invizibil pe un browser, înseamnă că `background-clip:text` nu e suportat — adaugă `color: var(--gold)` ca fallback înainte de `-webkit-background-clip`.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: gold glint static streak metalic pe elemente aurii"
```

---

## Task 2: Aplică același glint în `avocatbaciu-deploy-final-aria/index.html`

**Files:**
- Modify: `C:\site-avocat.baciu-root\avocatbaciu-deploy-final-aria\index.html` — același bloc `<style>`

**Notă:** Dacă folderul `avocatbaciu-deploy-final-aria` nu există, creează-l mai întâi ca o copie curată a lui `index.html` din root (fără `.git`, `.claude`, `netlify.toml`). Folderul trebuie să conțină: `index.html`, `style.css`, `favicon.png`, `apple-touch-icon.png`, `robots.txt`, `sitemap.xml`, `assets/images/og-twitter-card.jpeg`.

- [ ] **Step 1: Verifică dacă folderul există**

```bash
ls avocatbaciu-deploy-final-aria/
```

Dacă nu există, creează copia:
```bash
mkdir -p avocatbaciu-deploy-final-aria/assets/images
cp index.html avocatbaciu-deploy-final-aria/
cp style.css favicon.png apple-touch-icon.png robots.txt sitemap.xml avocatbaciu-deploy-final-aria/
cp assets/images/og-twitter-card.jpeg avocatbaciu-deploy-final-aria/assets/images/
```

- [ ] **Step 2: Inserează același bloc `<style>` de glint în `avocatbaciu-deploy-final-aria/index.html`**

Blocul CSS este identic cu cel din Task 1 Step 1. Inserează-l imediat după `</style>` al boost-color block (aceeași poziție — după linia cu `</style>` urmată de `<script type="application/ld+json">`).

- [ ] **Step 3: Commit**

```bash
git add avocatbaciu-deploy-final-aria/
git commit -m "feat: aplica glint metalic si in folderul deploy-final-aria"
```

---

## Self-Review

**Spec coverage:**
- ✅ `.hero-name .baciu-gold` — Task 1
- ✅ `.hero-name em` — Task 1
- ✅ `.hero-stat-num` — Task 1
- ✅ `.hero-stat-num sup` — Task 1
- ✅ `.hero-divider` — Task 1
- ✅ `.nav-brand` — Task 1
- ✅ `avocatbaciu-deploy-final-aria/index.html` — Task 2
- ✅ `style.css` nemodificat — confirmat, nu apare în niciun task
- ✅ No animation — gradientele sunt statice, fără `@keyframes`

**Placeholder scan:** Niciun TBD sau TODO.

**Consistency:** `--glint-text` definit în Task 1 și folosit consistent în toate selectoarele.
