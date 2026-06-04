# Revision Sheet — Design System Reference

A complete specification for recreating the exact visual language of the revision sheet.  
Copy these tokens and component rules verbatim to produce a consistent second sheet.

---

## 1. CSS Custom Properties (Design Tokens)

Paste this entire `:root` / theme block at the top of your `<style>` tag.

```css
/* ── Light mode (default) ──────────────────────────────── */
:root {
  --bg:       #f5f5f0;   /* warm off-white page background   */
  --text:     #2d2d2d;   /* soft dark body text              */
  --card:     #ffffff;   /* card surface                     */
  --muted:    #6b7280;   /* secondary / label text           */
  --accent1:  #4a7c8e;   /* primary accent — muted blue-teal */
  --accent2:  #5a8a6a;   /* secondary accent — sage green    */
  --quote-bg: #fdf6ec;   /* definition blockquote tint       */
  --code-bg:  #eef3f5;   /* formula / code cell background   */
  --code-txt: #2d4a55;   /* formula / code cell text         */
  --border:   #e2e8e4;   /* dividers, table borders          */
  --nav-bg:   #ffffffee; /* navbar (semi-transparent)        */
  --shadow:   0 2px 12px rgba(0,0,0,.07);
  --radius:   10px;
}

/* ── Dark mode (manual override) ──────────────────────── */
[data-theme="dark"] {
  --bg:       #1a1a2e;   /* deep navy                        */
  --text:     #e0e0d0;   /* warm off-white                   */
  --card:     #222235;   /* card surface                     */
  --muted:    #9999aa;   /* secondary text                   */
  --accent1:  #5ba3b0;   /* primary accent — bright teal     */
  --accent2:  #6aab7a;   /* secondary accent — bright sage   */
  --quote-bg: #2a2a3e;   /* blockquote tint                  */
  --code-bg:  #2e2e45;   /* code cell background             */
  --code-txt: #c9d1d9;   /* code cell text                   */
  --border:   #33334a;   /* dividers                         */
  --nav-bg:   #1a1a2eee; /* navbar                           */
  --shadow:   0 2px 16px rgba(0,0,0,.35);
}

/* ── Dark mode (system preference, no user override) ───── */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* …same values as [data-theme="dark"] above… */
  }
}
```

> **Third accent colour** (every 3rd card left-border): `#8a6a9e` (muted violet).  
> **Warning/remember border**: `#c0803a` (amber).  
> These two are hard-coded values, not CSS variables.

---

## 2. Typography

| Element | Font stack | Size | Weight | Other |
|---|---|---|---|---|
| Body / paragraphs | `Georgia, 'Times New Roman', serif` | `0.95 rem` | 400 | `line-height: 1.7` |
| Card title `h2` | Georgia (inherited) | `1.2 rem` | 600 | `line-height: 1.3` |
| Page title `h1` | Georgia (inherited) | `clamp(1.6rem, 4vw, 2.4rem)` | 400 | `letter-spacing: -0.01em` |
| Page subtitle `p` | `system-ui, sans-serif` | `0.9 rem` | 400 | `color: var(--muted)` |
| Nav brand | `system-ui, sans-serif` | `0.78 rem` | 700 | `letter-spacing: 0.12em; text-transform: uppercase` |
| Nav links | `system-ui, sans-serif` | `0.72 rem` | 400 | `color: var(--muted)` |
| Sub-section labels | `system-ui, sans-serif` | `0.7 rem` | 700 | `letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted)` |
| Card type-tag | `system-ui, sans-serif` | `0.65 rem` | 700 | `letter-spacing: 0.1em; text-transform: uppercase` |
| Page topic tags | `system-ui, sans-serif` | `0.68 rem` | 600 | `letter-spacing: 0.08em; text-transform: uppercase` |
| Formula table body | `system-ui, sans-serif` | `0.875 rem` | 400 | — |
| Formula table header | `system-ui, sans-serif` | `0.68 rem` | — | `letter-spacing: 0.08em; text-transform: uppercase` |
| Formula values (2nd column) | `'Courier New', monospace` | inherited | — | `background: var(--code-bg); color: var(--code-txt)` |
| Key-point list items | Georgia (inherited) | `0.95 rem` | 400 | bullet: `▸` in `var(--accent1)` |
| Definition blockquote | Georgia (inherited) | `0.97 rem` | 400 | `font-style: italic` |
| Remember/warning box | `system-ui, sans-serif` | `0.9 rem` | 400 | — |
| Toggle button | `system-ui, sans-serif` | `0.72 rem` | 400 | — |

---

## 3. Layout & Spacing

```
Max content width : 1000px (centered, auto margins)
Main padding      : 2.5rem top · 1.25rem sides · 4rem bottom
Gap between cards : 1.75rem
Mobile breakpoint : 600px — reduce card padding to 1.25rem 1rem
```

### Page header
- Centred text
- `h1` followed by a muted subtitle `<p>`
- Tag pills row below subtitle (`margin-top: 0.8rem`, `gap: 0.4rem`)

---

## 4. Navbar

```
position: sticky; top: 0; z-index: 100
background: var(--nav-bg)  ← semi-transparent
backdrop-filter: blur(10px)
border-bottom: 1px solid var(--border)
transition: background 0.3s, border-color 0.3s
```

- Inner flex row: **brand** | **jump links** (flex-wrap) | **toggle button** (margin-left: auto)
- Jump link hover: `color: var(--text); background: var(--border); border-radius: 5px`
- Toggle button: pill shape (`border-radius: 20px`), stores preference in `localStorage`

---

## 5. Card Component

```css
section.card {
  background    : var(--card);
  border-radius : var(--radius);          /* 10px */
  box-shadow    : var(--shadow);
  border-left   : 4px solid <accent>;     /* see rotation below */
  padding       : 1.75rem 1.75rem 1.5rem;
  transition    : background 0.3s, box-shadow 0.3s;
}
```

**Left-border accent rotation** (applied via `:nth-child`):

| Card position | Border colour |
|---|---|
| Odd cards (default) | `var(--accent1)` — blue-teal |
| Even cards | `var(--accent2)` — sage green |
| Every 3rd card | `#8a6a9e` — muted violet |

Card header is a flex row: **h2 title** + **type-tag pill** (baseline-aligned, `gap: 0.75rem`).  
Type-tag pill: `background: var(--code-bg)`, coloured text matching the card's accent, `border-radius: 4px`.

---

## 6. Sub-section Label

A horizontal rule with a text label on the left:

```css
.sub-label {
  font-family   : system-ui, sans-serif;
  font-size     : 0.7rem;
  font-weight   : 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color         : var(--muted);
  margin        : 1.2rem 0 0.5rem;
  display       : flex;
  align-items   : center;
  gap           : 0.4rem;
}
.sub-label::after {
  content   : '';
  flex      : 1;
  height    : 1px;
  background: var(--border);
}
```

Always appears directly above its content block. Labels used: **Definition**, **Key Points**, **Formulas & Rules**, **Use Cases**, **Things to Remember**.  
Skip any label whose section has no content for that topic.

---

## 7. Component Inventory

### Definition Blockquote
```css
blockquote.def {
  background   : var(--quote-bg);
  border-left  : 3px solid var(--accent1);
  border-radius: 0 6px 6px 0;
  padding      : 0.75rem 1rem;
  font-style   : italic;
  font-size    : 0.97rem;
  transition   : background 0.3s;
}
```

### Key Points List (`ul.kp`)
- `list-style: none`; items have `padding-left: 1.4rem`
- `::before` pseudo-element: content `'▸'`, `color: var(--accent1)`, `font-size: 0.75rem`
- Even-card items use `var(--accent2)` for the bullet
- `strong` inside items: `font-style: normal; color: var(--text)`

### Formula Table (`.formula-table`)
- Full-width, `border-collapse: collapse`, `font-family: system-ui`
- `th`: `background: var(--code-bg)`, `color: var(--muted)`, `0.68rem` uppercase, `border-bottom: 2px solid var(--border)`
- `td`: `padding: 0.5rem 0.85rem`, `border-bottom: 1px solid var(--border)`
- **Second column** cells: `font-family: 'Courier New', monospace`, `background: var(--code-bg)`, `color: var(--code-txt)`, `border-radius: 4px`
- Row hover: `background: var(--quote-bg)` on all `td`

### Comparison Table (`.cmp-table`)
- `th`: `background: var(--accent1)`, `color: #fff`, first/last th get rounded top corners (`6px`)
- First column `td`: `font-weight: 600; color: var(--muted)` (row labels)
- Row hover: `background: var(--quote-bg)`

### Use Cases Block
Plain `<p class="use-cases">` at `0.95rem`, `color: var(--text)`. No special background.

### Remember / Warning Box (`.remember`)
```css
.remember {
  background   : var(--code-bg);
  border-left  : 3px solid #c0803a;   /* amber */
  border-radius: 0 6px 6px 0;
  padding      : 0.7rem 1rem;
  font-size    : 0.9rem;
  font-family  : system-ui, sans-serif;
  display      : flex;
  gap          : 0.5rem;
  align-items  : flex-start;
}
```
Always begins with an emoji icon in a `<span class="icon">` (`font-size: 1rem`).  
Use `⚠️` for warnings/mistakes, `💡` for tips.

### Page / Navbar Topic Tags
```css
.tag {
  font-size    : 0.68rem;
  font-weight  : 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding      : 0.25rem 0.65rem;
  border-radius: 20px;
  background   : var(--code-bg);
  color        : var(--accent1);
  border       : 1px solid var(--border);
}
```

---

## 8. Transitions & Animation

All theme-affected elements carry:
```css
transition: background 0.3s, color 0.3s;
```
Nav additionally transitions `border-color 0.3s`.  
Cards additionally transition `box-shadow 0.3s`.  
No other animations. No keyframes. No transforms.

---

## 9. Theme Toggle — JavaScript Logic

```js
(function () {
  const root = document.documentElement;

  function systemPrefersDark() {
    return window.matchMedia('(prefers-color-scheme: dark)').matches;
  }
  function applyTheme(dark) {
    root.setAttribute('data-theme', dark ? 'dark' : 'light');
    // update button icon/label here
  }

  // On load: respect saved preference, else fall back to OS setting
  const saved = localStorage.getItem('theme');
  applyTheme(saved ? saved === 'dark' : systemPrefersDark());

  // Button click: toggle and save
  document.getElementById('themeToggle').addEventListener('click', function () {
    const next = root.getAttribute('data-theme') !== 'dark';
    applyTheme(next);
    localStorage.setItem('theme', next ? 'dark' : 'light');
  });

  // Sync with OS changes only if user hasn't set a preference
  window.matchMedia('(prefers-color-scheme: dark)')
    .addEventListener('change', e => {
      if (!localStorage.getItem('theme')) applyTheme(e.matches);
    });
})();
```

Toggle button HTML:
```html
<button class="toggle-btn" id="themeToggle" aria-label="Toggle theme">
  <span id="themeIcon">☀️</span>
  <span id="themeLabel">Light</span>
</button>
```
Icon is `☀️` in light mode, `🌙` in dark mode. Label text is `Light` / `Dark`.

---

## 10. Mobile Breakpoint (`max-width: 600px`)

```css
main        { padding: 1.5rem 0.85rem 3rem; }
section.card { padding: 1.25rem 1rem 1rem; }
.nav-inner  { padding: 0 0.75rem; }
.nav-brand  { font-size: 0.7rem; }
.toggle-btn { padding: 0.3rem 0.6rem; }
.formula-table td:nth-child(2) { white-space: normal; }
```

---

## 11. Required HTML Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[Topic] — Revision Sheet</title>
  <style>/* paste full CSS here */</style>
</head>
<body>

<nav>
  <div class="nav-inner">
    <span class="nav-brand">📐 [Topic Name]</span>
    <div class="nav-links">
      <a href="#section-id">Section Name</a>
      <!-- one <a> per section -->
    </div>
    <button class="toggle-btn" id="themeToggle" aria-label="Toggle theme">
      <span id="themeIcon">☀️</span>
      <span id="themeLabel">Light</span>
    </button>
  </div>
</nav>

<main>

  <div class="page-header">
    <h1>[Topic] — Revision Sheet</h1>
    <p>[One-line description]</p>
    <div class="tag-row">
      <span class="tag">Tag One</span>
      <span class="tag">Tag Two</span>
    </div>
  </div>

  <section class="card" id="section-id">
    <div class="card-header">
      <h2>Section Title</h2>
      <span class="card-tag">Category Label</span>
    </div>

    <!-- Include only subsections that have content -->

    <div class="sub-label">Definition</div>
    <blockquote class="def">One-liner definition here.</blockquote>

    <div class="sub-label">Key Points</div>
    <ul class="kp">
      <li><strong>Term</strong> — explanation.</li>
    </ul>

    <div class="sub-label">Formulas &amp; Rules</div>
    <table class="formula-table">
      <thead><tr><th>Property</th><th>Formula</th></tr></thead>
      <tbody>
        <tr><td>Label</td><td>value</td></tr>
      </tbody>
    </table>

    <div class="sub-label">Use Cases</div>
    <p class="use-cases">Prose description.</p>

    <div class="sub-label">Things to Remember</div>
    <div class="remember">
      <span class="icon">⚠️</span>
      Warning text here.
    </div>

  </section>

  <!-- repeat section.card for each topic -->

</main>

<script>/* paste theme toggle JS here */</script>
</body>
</html>
```

---

## 12. Rules Checklist

- [ ] All CSS inside a single `<style>` tag — no external libraries or CDNs
- [ ] All JS inside a single `<script>` tag at end of `<body>`
- [ ] Single `.html` file — fully self-contained
- [ ] Skip any subsection (`sub-label` + its content block) that has no data for that topic
- [ ] Card left-border rotates: accent1 → accent2 → violet → repeat
- [ ] `localStorage` key is `"theme"`, values are `"dark"` or `"light"`
- [ ] Nav anchor IDs match `section.card` element IDs exactly
- [ ] `transition: background 0.3s, color 0.3s` on `body` and all theme-sensitive elements
- [ ] Mobile styles inside `@media (max-width: 600px)`
