# Index Page — Design System & Maintenance Guide

A complete specification for building and updating the `index.html` hub page that links to all revision sheets. Covers the full design system, card anatomy, category structure, and step-by-step instructions for adding new pages.

---

## 1. Purpose & Mental Model

The `index.html` is the **home / dashboard** page. It does three things:

1. Lists every subject category (e.g. Data Structures, Algorithms, Databases).
2. Inside each category, shows cards — one per revision sheet.
3. Each card links to the corresponding `.html` file.

Think of it as a **library index**: categories are shelves, cards are books on those shelves.

```
index.html
│
├── Category: Data Structures
│   ├── Card → binary-tree-revision.html
│   ├── Card → linked-list-revision.html
│   └── Card → graph-revision.html
│
├── Category: Algorithms
│   ├── Card → sorting-revision.html
│   └── Card → dynamic-programming-revision.html
│
└── Category: Databases
    └── Card → sql-revision.html
```

---

## 2. CSS Design Tokens

Identical token set to the revision sheets — paste this block verbatim so both pages share the same visual language.

```css
:root {
  --bg:          #f5f5f0;
  --text:        #2d2d2d;
  --card:        #ffffff;
  --muted:       #6b7280;
  --accent1:     #4a7c8e;
  --accent2:     #5a8a6a;
  --accent3:     #8a6a9e;   /* third accent; rotation continues below */
  --accent4:     #8e6a4a;   /* warm brown — 4th category              */
  --accent5:     #7a8a4a;   /* olive — 5th category                    */
  --quote-bg:    #fdf6ec;
  --code-bg:     #eef3f5;
  --code-txt:    #2d4a55;
  --border:      #e2e8e4;
  --nav-bg:      #ffffffee;
  --shadow:      0 2px 12px rgba(0,0,0,.07);
  --shadow-hover:0 6px 24px rgba(0,0,0,.13);
  --radius:      10px;
}

[data-theme="dark"] {
  --bg:          #1a1a2e;
  --text:        #e0e0d0;
  --card:        #222235;
  --muted:       #9999aa;
  --accent1:     #5ba3b0;
  --accent2:     #6aab7a;
  --accent3:     #a07abe;
  --accent4:     #b08060;
  --accent5:     #96a660;
  --quote-bg:    #2a2a3e;
  --code-bg:     #2e2e45;
  --code-txt:    #c9d1d9;
  --border:      #33334a;
  --nav-bg:      #1a1a2eee;
  --shadow:      0 2px 16px rgba(0,0,0,.35);
  --shadow-hover:0 8px 28px rgba(0,0,0,.5);
}

@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* same values as [data-theme="dark"] */
  }
}
```

### Accent-to-Category Mapping

Assign one accent colour per **category**. Never re-use an accent within the same category. Cards inside a category all share their category's accent for the top border.

| Variable | Hex (light) | Hex (dark) | Suggested Category |
|---|---|---|---|
| `--accent1` | `#4a7c8e` | `#5ba3b0` | Data Structures |
| `--accent2` | `#5a8a6a` | `#6aab7a` | Algorithms |
| `--accent3` | `#8a6a9e` | `#a07abe` | Databases |
| `--accent4` | `#8e6a4a` | `#b08060` | Operating Systems |
| `--accent5` | `#7a8a4a` | `#96a660` | Networks |

To add a 6th+ category: pick a new muted colour pair, add `--accent6` to both `:root` and `[data-theme="dark"]`.

---

## 3. Typography

Identical scale to the revision sheets.

| Element | Font | Size | Weight | Notes |
|---|---|---|---|---|
| Body | `Georgia, serif` | `0.95rem` | 400 | `line-height: 1.7` |
| Page `h1` | Georgia | `clamp(1.6rem, 4vw, 2.4rem)` | 400 | `letter-spacing: -0.01em` |
| Page subtitle | `system-ui` | `0.9rem` | 400 | `color: var(--muted)` |
| Category heading `h2` | `system-ui` | `0.7rem` | 700 | `uppercase; letter-spacing: 0.12em; color: var(--muted)` |
| Card title | `system-ui` | `1rem` | 600 | `color: var(--text)` |
| Card description | Georgia | `0.88rem` | 400 | `color: var(--muted); line-height: 1.55` |
| Card meta (topic count) | `system-ui` | `0.72rem` | 400 | `color: var(--muted)` |
| Card topic pills | `system-ui` | `0.65rem` | 600 | `uppercase; letter-spacing: 0.07em` |
| Nav brand | `system-ui` | `0.78rem` | 700 | `uppercase; letter-spacing: 0.12em; color: var(--accent1)` |
| Nav links | `system-ui` | `0.72rem` | 400 | `color: var(--muted)` |

---

## 4. Layout & Spacing

```
Page max-width   : 1000px (centered, auto margins)
Main padding     : 2.5rem top · 1.25rem sides · 4rem bottom
Category gap     : 3rem between category sections
Card grid        : CSS Grid, auto-fill, minmax(280px, 1fr), gap 1.25rem
Mobile (≤600px)  : single-column grid, reduced padding
```

### Grid declaration
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.25rem;
}
```

---

## 5. Page Structure (HTML Skeleton)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Revision Index</title>
  <style>/* full CSS */</style>
</head>
<body>

  <!-- ① Navbar -->
  <nav>
    <div class="nav-inner">
      <span class="nav-brand">📚 Revision Index</span>
      <div class="nav-links">
        <a href="#data-structures">Data Structures</a>
        <a href="#algorithms">Algorithms</a>
        <a href="#databases">Databases</a>
        <!-- one <a> per category -->
      </div>
      <button class="toggle-btn" id="themeToggle" aria-label="Toggle theme">
        <span id="themeIcon">☀️</span>
        <span id="themeLabel">Light</span>
      </button>
    </div>
  </nav>

  <!-- ② Main content -->
  <main>

    <!-- Page header -->
    <div class="page-header">
      <h1>Revision Index</h1>
      <p>All study sheets, organised by subject</p>
    </div>

    <!-- ③ Category section (repeat per subject) -->
    <section class="category" id="data-structures">
      <div class="category-header">
        <h2>Data Structures</h2>
        <span class="category-count">3 sheets</span>
      </div>
      <div class="card-grid">

        <!-- ④ Sheet card (repeat per sheet) -->
        <a class="sheet-card accent1" href="binary-tree-revision.html">
          <div class="sheet-card-top">
            <span class="sheet-icon">🌲</span>
            <span class="sheet-status complete">Complete</span>
          </div>
          <h3 class="sheet-title">Binary Trees</h3>
          <p class="sheet-desc">Types, properties, B+ trees, and balancing rules.</p>
          <div class="sheet-footer">
            <span class="sheet-meta">10 topics</span>
            <div class="sheet-pills">
              <span class="pill">Full</span>
              <span class="pill">Complete</span>
              <span class="pill">B+ Tree</span>
            </div>
          </div>
        </a>

        <!-- more .sheet-card elements -->

      </div>
    </section>

    <!-- repeat .category sections -->

  </main>

  <script>/* theme toggle JS */</script>
</body>
</html>
```

---

## 6. Navbar

Same rules as revision sheets — see `revision-sheet-design-system.md` §4.

**Index-specific addition**: nav jump links point to `#category-id` anchors on the same page, not to section IDs inside sheets.

```html
<a href="#data-structures">Data Structures</a>
<a href="#algorithms">Algorithms</a>
```

---

## 7. Category Section — Full CSS

```css
.category {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.category-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding-bottom: 0.6rem;
  border-bottom: 2px solid var(--border);
}

.category-header h2 {
  font-family: system-ui, sans-serif;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--muted);
  margin: 0;
}

.category-count {
  font-family: system-ui, sans-serif;
  font-size: 0.68rem;
  color: var(--muted);
  background: var(--code-bg);
  padding: 0.1rem 0.5rem;
  border-radius: 10px;
  border: 1px solid var(--border);
}
```

**Labelling rule**: update `category-count` whenever you add or remove a card. The text format is `N sheets`.

---

## 8. Sheet Card — Full CSS

```css
a.sheet-card {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  background: var(--card);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  border-top: 3px solid var(--accent1);   /* overridden by accent class */
  padding: 1.25rem 1.25rem 1rem;
  text-decoration: none;
  color: inherit;
  transition: box-shadow 0.2s, transform 0.2s, background 0.3s;
  cursor: pointer;
}
a.sheet-card:hover {
  box-shadow: var(--shadow-hover);
  transform: translateY(-2px);
}

/* Accent modifier classes — one per category */
a.sheet-card.accent1 { border-top-color: var(--accent1); }
a.sheet-card.accent2 { border-top-color: var(--accent2); }
a.sheet-card.accent3 { border-top-color: var(--accent3); }
a.sheet-card.accent4 { border-top-color: var(--accent4); }
a.sheet-card.accent5 { border-top-color: var(--accent5); }

/* Card internals */
.sheet-card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.sheet-icon {
  font-size: 1.4rem;
  line-height: 1;
}
.sheet-title {
  font-family: system-ui, sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: var(--text);
  margin: 0;
  line-height: 1.3;
}
.sheet-desc {
  font-family: Georgia, serif;
  font-size: 0.88rem;
  color: var(--muted);
  line-height: 1.55;
  margin: 0;
  flex: 1;
}
.sheet-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: 0.2rem;
  padding-top: 0.6rem;
  border-top: 1px solid var(--border);
}
.sheet-meta {
  font-family: system-ui, sans-serif;
  font-size: 0.72rem;
  color: var(--muted);
}
.sheet-pills {
  display: flex;
  flex-wrap: wrap;
  gap: 0.3rem;
}
.pill {
  font-family: system-ui, sans-serif;
  font-size: 0.65rem;
  font-weight: 600;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  background: var(--code-bg);
  color: var(--muted);
  padding: 0.15rem 0.45rem;
  border-radius: 4px;
  border: 1px solid var(--border);
}
```

### Status Badge (`.sheet-status`)

Optionally shown in `.sheet-card-top` to indicate sheet completeness.

```css
.sheet-status {
  font-family: system-ui, sans-serif;
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 0.15rem 0.5rem;
  border-radius: 4px;
}
.sheet-status.complete  { background: #d4edda; color: #2d6a3f; }
.sheet-status.draft     { background: #fff3cd; color: #856404; }
.sheet-status.planned   { background: var(--code-bg); color: var(--muted); }

/* Dark mode overrides */
[data-theme="dark"] .sheet-status.complete { background: #1a3a24; color: #6aab7a; }
[data-theme="dark"] .sheet-status.draft    { background: #3a2e10; color: #c9a94a; }
```

Use `complete` once the sheet is finished, `draft` for work-in-progress, `planned` for placeholder cards.

---

## 9. Category Colour Reference Card

Print or bookmark this table. Every time you add a sheet card, use the accent class that matches its category row.

| Category | Section ID | CSS class | Accent (light) | Accent (dark) |
|---|---|---|---|---|
| Data Structures | `#data-structures` | `accent1` | `#4a7c8e` | `#5ba3b0` |
| Algorithms | `#algorithms` | `accent2` | `#5a8a6a` | `#6aab7a` |
| Databases | `#databases` | `accent3` | `#8a6a9e` | `#a07abe` |
| Operating Systems | `#operating-systems` | `accent4` | `#8e6a4a` | `#b08060` |
| Networks | `#networks` | `accent5` | `#7a8a4a` | `#96a660` |
| *(new category)* | `#your-id` | `accent6` | *(define new)* | *(define new)* |

---

## 10. Step-by-Step: Adding a New Sheet Card

Do these steps every time you create a new revision `.html` file and want it listed on the index.

### Step 1 — Identify the category

Find the existing `<section class="category" id="...">` that matches your subject. If none exists, jump to §11 first, then return here.

### Step 2 — Add the card inside `.card-grid`

Copy the template below. Fill in every `[ ]` placeholder.

```html
<a class="sheet-card [accent-class]" href="[filename].html">
  <div class="sheet-card-top">
    <span class="sheet-icon">[emoji]</span>
    <span class="sheet-status [complete|draft|planned]">[Complete|Draft|Planned]</span>
  </div>
  <h3 class="sheet-title">[Short topic name]</h3>
  <p class="sheet-desc">[One or two sentences describing what the sheet covers.]</p>
  <div class="sheet-footer">
    <span class="sheet-meta">[N] topics</span>
    <div class="sheet-pills">
      <span class="pill">[KeyTopic1]</span>
      <span class="pill">[KeyTopic2]</span>
      <span class="pill">[KeyTopic3]</span>
    </div>
  </div>
</a>
```

**Filling in the blanks:**

| Placeholder | Rule |
|---|---|
| `[accent-class]` | Must match the category — see §9 table |
| `[filename].html` | Exact filename of the revision sheet, relative path |
| `[emoji]` | One emoji visually representing the topic (e.g. 🌲 trees, ⚡ algorithms, 🗄️ databases) |
| `[status]` | `complete` / `draft` / `planned` |
| `[Short topic name]` | 2–4 words, title-case (e.g. "Linked Lists", "Sorting Algorithms") |
| `[description]` | 1–2 sentences max; use `var(--muted)` text so it doesn't compete with the title |
| `[N] topics` | Count of `<section class="card">` elements inside the linked sheet |
| `[pill text]` | 2–4 key sub-topics from the sheet; single words or short phrases |

### Step 3 — Update the category count

Find the `<span class="category-count">` inside that category's `.category-header` and increment the number.

```html
<!-- Before: 2 sheets -->
<span class="category-count">2 sheets</span>

<!-- After adding one card: -->
<span class="category-count">3 sheets</span>
```

### Step 4 — Check the navbar

The navbar only lists categories, not individual sheets. No navbar change is needed when adding a new card to an existing category.

---

## 11. Step-by-Step: Adding a New Category

Do these steps when your new sheet belongs to a subject that doesn't yet have a section.

### Step 1 — Define a new accent variable

In the `:root` block (and the `[data-theme="dark"]` block), add:

```css
:root {
  /* existing ... */
  --accent6: #4a6a8e;   /* example: muted slate blue for new category */
}
[data-theme="dark"] {
  /* existing ... */
  --accent6: #6a9ab0;
}
```

Pick a muted, mid-saturation colour. Avoid anything too similar to existing accents.

### Step 2 — Add the accent modifier class

In the CSS section with `.sheet-card.accent1`, `.accent2` etc., add:

```css
a.sheet-card.accent6 { border-top-color: var(--accent6); }
```

### Step 3 — Add a nav link

Inside `<div class="nav-links">` in the navbar, append:

```html
<a href="#your-category-id">Your Category Name</a>
```

### Step 4 — Add the category section to `<main>`

Append this block after the last existing `<section class="category">`:

```html
<section class="category" id="your-category-id">
  <div class="category-header">
    <h2>Your Category Name</h2>
    <span class="category-count">1 sheet</span>
  </div>
  <div class="card-grid">

    <!-- first card goes here, using class="sheet-card accent6" -->

  </div>
</section>
```

### Step 5 — Add your first card

Follow §10 using `accent6` as the class.

### Step 6 — Update the §9 table (in your own records)

Add a row to your local copy of the category colour reference table so future additions stay consistent.

---

## 12. File Naming Convention

Keep all revision sheet filenames lowercase, hyphenated, and suffixed with `-revision.html`:

```
binary-tree-revision.html
linked-list-revision.html
sorting-algorithms-revision.html
sql-basics-revision.html
tcp-ip-revision.html
```

The `href` in the card must match the filename exactly (case-sensitive on Linux/servers).

---

## 13. Mobile Breakpoint (`max-width: 600px`)

```css
@media (max-width: 600px) {
  main         { padding: 1.5rem 0.85rem 3rem; }
  .card-grid   { grid-template-columns: 1fr; }
  .sheet-card  { padding: 1rem 1rem 0.85rem; }
  .nav-inner   { padding: 0 0.75rem; }
  .nav-brand   { font-size: 0.7rem; }
  .toggle-btn  { padding: 0.3rem 0.6rem; }
  .sheet-pills { display: none; }   /* hide pills on very small screens */
}
```

---

## 14. Linking Index ↔ Revision Sheets

### From index → sheet
The `href` on each `.sheet-card` is a relative path. If all files live in the same folder:

```html
<a class="sheet-card accent1" href="binary-tree-revision.html">
```

If sheets are in a subfolder (e.g. `sheets/`):
```html
<a class="sheet-card accent1" href="sheets/binary-tree-revision.html">
```

### From sheet → index (back link)
Add a subtle back-link in each revision sheet's navbar, before the jump links:

```html
<a class="back-link" href="index.html">← Index</a>
```

```css
.back-link {
  font-family: system-ui, sans-serif;
  font-size: 0.72rem;
  color: var(--muted);
  text-decoration: none;
  padding: 0.4rem 0.55rem;
  border-radius: 5px;
  margin-right: 0.5rem;
  border-right: 1px solid var(--border);
  transition: color 0.2s, background 0.2s;
  white-space: nowrap;
}
.back-link:hover { color: var(--text); background: var(--border); }
```

Place it as the **first child** of `.nav-inner`, before `.nav-brand`.

---

## 15. Maintenance Checklist

Run through this list every time you add or update content.

**When adding a new sheet card:**
- [ ] Card uses the correct `accent[N]` class for its category
- [ ] `href` matches the actual filename exactly
- [ ] `sheet-meta` topic count matches the sheet's actual section count
- [ ] `sheet-status` reflects current state (`complete` / `draft` / `planned`)
- [ ] Category `category-count` number incremented
- [ ] No new nav link added (cards don't get nav links — only categories do)

**When adding a new category:**
- [ ] New `--accent[N]` variable added to both `:root` and `[data-theme="dark"]`
- [ ] New `.sheet-card.accent[N]` CSS modifier class added
- [ ] New `<a href="#id">` added to navbar
- [ ] New `<section class="category" id="...">` added to `<main>`
- [ ] Section `id` matches the navbar `href` exactly (without the `#`)
- [ ] Category colour added to the reference table in your records

**When renaming or moving a sheet:**
- [ ] `href` on the index card updated
- [ ] `← Index` back-link in the sheet still points to `index.html` (or correct relative path)
- [ ] Old filename removed / redirected if already shared publicly

---

## 16. Full Worked Example

Here is a complete, minimal `index.html` with two categories and three cards, ready to extend.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Revision Index</title>
<style>
  :root {
    --bg:#f5f5f0; --text:#2d2d2d; --card:#ffffff; --muted:#6b7280;
    --accent1:#4a7c8e; --accent2:#5a8a6a; --accent3:#8a6a9e;
    --quote-bg:#fdf6ec; --code-bg:#eef3f5; --code-txt:#2d4a55;
    --border:#e2e8e4; --nav-bg:#ffffffee;
    --shadow:0 2px 12px rgba(0,0,0,.07);
    --shadow-hover:0 6px 24px rgba(0,0,0,.13);
    --radius:10px;
  }
  [data-theme="dark"] {
    --bg:#1a1a2e; --text:#e0e0d0; --card:#222235; --muted:#9999aa;
    --accent1:#5ba3b0; --accent2:#6aab7a; --accent3:#a07abe;
    --quote-bg:#2a2a3e; --code-bg:#2e2e45; --code-txt:#c9d1d9;
    --border:#33334a; --nav-bg:#1a1a2eee;
    --shadow:0 2px 16px rgba(0,0,0,.35);
    --shadow-hover:0 8px 28px rgba(0,0,0,.5);
  }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) {
      --bg:#1a1a2e; --text:#e0e0d0; --card:#222235; --muted:#9999aa;
      --accent1:#5ba3b0; --accent2:#6aab7a; --accent3:#a07abe;
      --quote-bg:#2a2a3e; --code-bg:#2e2e45; --code-txt:#c9d1d9;
      --border:#33334a; --nav-bg:#1a1a2eee;
      --shadow:0 2px 16px rgba(0,0,0,.35);
      --shadow-hover:0 8px 28px rgba(0,0,0,.5);
    }
  }
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
  html{scroll-behavior:smooth;}
  body{font-family:Georgia,'Times New Roman',serif;background:var(--bg);color:var(--text);line-height:1.7;transition:background .3s,color .3s;min-height:100vh;}

  nav{position:sticky;top:0;z-index:100;background:var(--nav-bg);backdrop-filter:blur(10px);border-bottom:1px solid var(--border);transition:background .3s,border-color .3s;}
  .nav-inner{max-width:1000px;margin:0 auto;padding:0 1.25rem;display:flex;align-items:center;flex-wrap:wrap;gap:.1rem;}
  .back-link{font-family:system-ui,sans-serif;font-size:.72rem;color:var(--muted);text-decoration:none;padding:.4rem .55rem;border-radius:5px;margin-right:.4rem;border-right:1px solid var(--border);transition:color .2s,background .2s;white-space:nowrap;}
  .back-link:hover{color:var(--text);background:var(--border);}
  .nav-brand{font-family:system-ui,sans-serif;font-size:.78rem;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--accent1);padding:.75rem 0;margin-right:.75rem;white-space:nowrap;flex-shrink:0;}
  .nav-links{display:flex;flex-wrap:wrap;gap:.1rem;flex:1;}
  .nav-links a{font-family:system-ui,sans-serif;font-size:.72rem;color:var(--muted);text-decoration:none;padding:.4rem .55rem;border-radius:5px;transition:color .2s,background .2s;white-space:nowrap;}
  .nav-links a:hover{color:var(--text);background:var(--border);}
  .toggle-btn{margin-left:auto;flex-shrink:0;background:var(--card);border:1px solid var(--border);color:var(--text);border-radius:20px;padding:.35rem .85rem;font-size:.72rem;font-family:system-ui,sans-serif;cursor:pointer;transition:background .3s,color .3s,border-color .3s;display:flex;align-items:center;gap:.35rem;}
  .toggle-btn:hover{background:var(--border);}

  main{max-width:1000px;margin:0 auto;padding:2.5rem 1.25rem 4rem;display:flex;flex-direction:column;gap:3rem;}
  .page-header{text-align:center;padding:1rem 0 .5rem;}
  .page-header h1{font-size:clamp(1.6rem,4vw,2.4rem);font-weight:400;letter-spacing:-.01em;}
  .page-header p{color:var(--muted);font-size:.9rem;margin-top:.4rem;font-family:system-ui,sans-serif;}

  .category{display:flex;flex-direction:column;gap:1rem;}
  .category-header{display:flex;align-items:center;gap:.75rem;padding-bottom:.6rem;border-bottom:2px solid var(--border);}
  .category-header h2{font-family:system-ui,sans-serif;font-size:.7rem;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--muted);margin:0;}
  .category-count{font-family:system-ui,sans-serif;font-size:.68rem;color:var(--muted);background:var(--code-bg);padding:.1rem .5rem;border-radius:10px;border:1px solid var(--border);}

  .card-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:1.25rem;}

  a.sheet-card{display:flex;flex-direction:column;gap:.6rem;background:var(--card);border-radius:var(--radius);box-shadow:var(--shadow);border-top:3px solid var(--accent1);padding:1.25rem 1.25rem 1rem;text-decoration:none;color:inherit;transition:box-shadow .2s,transform .2s,background .3s;cursor:pointer;}
  a.sheet-card:hover{box-shadow:var(--shadow-hover);transform:translateY(-2px);}
  a.sheet-card.accent1{border-top-color:var(--accent1);}
  a.sheet-card.accent2{border-top-color:var(--accent2);}
  a.sheet-card.accent3{border-top-color:var(--accent3);}

  .sheet-card-top{display:flex;align-items:center;justify-content:space-between;}
  .sheet-icon{font-size:1.4rem;line-height:1;}
  .sheet-title{font-family:system-ui,sans-serif;font-size:1rem;font-weight:600;color:var(--text);margin:0;line-height:1.3;}
  .sheet-desc{font-family:Georgia,serif;font-size:.88rem;color:var(--muted);line-height:1.55;margin:0;flex:1;}
  .sheet-footer{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:.4rem;margin-top:.2rem;padding-top:.6rem;border-top:1px solid var(--border);}
  .sheet-meta{font-family:system-ui,sans-serif;font-size:.72rem;color:var(--muted);}
  .sheet-pills{display:flex;flex-wrap:wrap;gap:.3rem;}
  .pill{font-family:system-ui,sans-serif;font-size:.65rem;font-weight:600;letter-spacing:.07em;text-transform:uppercase;background:var(--code-bg);color:var(--muted);padding:.15rem .45rem;border-radius:4px;border:1px solid var(--border);}
  .sheet-status{font-family:system-ui,sans-serif;font-size:.65rem;font-weight:700;letter-spacing:.08em;text-transform:uppercase;padding:.15rem .5rem;border-radius:4px;}
  .sheet-status.complete{background:#d4edda;color:#2d6a3f;}
  .sheet-status.draft{background:#fff3cd;color:#856404;}
  .sheet-status.planned{background:var(--code-bg);color:var(--muted);}
  [data-theme="dark"] .sheet-status.complete{background:#1a3a24;color:#6aab7a;}
  [data-theme="dark"] .sheet-status.draft{background:#3a2e10;color:#c9a94a;}

  @media(max-width:600px){
    main{padding:1.5rem .85rem 3rem;}
    .card-grid{grid-template-columns:1fr;}
    .sheet-card{padding:1rem 1rem .85rem;}
    .nav-inner{padding:0 .75rem;}
    .nav-brand{font-size:.7rem;}
    .toggle-btn{padding:.3rem .6rem;}
    .sheet-pills{display:none;}
  }
</style>
</head>
<body>

<nav>
  <div class="nav-inner">
    <span class="nav-brand">📚 Revision Index</span>
    <div class="nav-links">
      <a href="#data-structures">Data Structures</a>
      <a href="#algorithms">Algorithms</a>
    </div>
    <button class="toggle-btn" id="themeToggle" aria-label="Toggle theme">
      <span id="themeIcon">☀️</span>
      <span id="themeLabel">Light</span>
    </button>
  </div>
</nav>

<main>
  <div class="page-header">
    <h1>Revision Index</h1>
    <p>All study sheets, organised by subject</p>
  </div>

  <section class="category" id="data-structures">
    <div class="category-header">
      <h2>Data Structures</h2>
      <span class="category-count">2 sheets</span>
    </div>
    <div class="card-grid">

      <a class="sheet-card accent1" href="binary-tree-revision.html">
        <div class="sheet-card-top">
          <span class="sheet-icon">🌲</span>
          <span class="sheet-status complete">Complete</span>
        </div>
        <h3 class="sheet-title">Binary Trees</h3>
        <p class="sheet-desc">Types, properties, B+ trees, and balancing strategies.</p>
        <div class="sheet-footer">
          <span class="sheet-meta">10 topics</span>
          <div class="sheet-pills">
            <span class="pill">Full</span>
            <span class="pill">B+ Tree</span>
            <span class="pill">Balanced</span>
          </div>
        </div>
      </a>

      <a class="sheet-card accent1" href="linked-list-revision.html">
        <div class="sheet-card-top">
          <span class="sheet-icon">🔗</span>
          <span class="sheet-status draft">Draft</span>
        </div>
        <h3 class="sheet-title">Linked Lists</h3>
        <p class="sheet-desc">Singly, doubly, and circular lists with pointer operations.</p>
        <div class="sheet-footer">
          <span class="sheet-meta">6 topics</span>
          <div class="sheet-pills">
            <span class="pill">Singly</span>
            <span class="pill">Doubly</span>
            <span class="pill">Circular</span>
          </div>
        </div>
      </a>

    </div>
  </section>

  <section class="category" id="algorithms">
    <div class="category-header">
      <h2>Algorithms</h2>
      <span class="category-count">1 sheet</span>
    </div>
    <div class="card-grid">

      <a class="sheet-card accent2" href="sorting-revision.html">
        <div class="sheet-card-top">
          <span class="sheet-icon">⚡</span>
          <span class="sheet-status planned">Planned</span>
        </div>
        <h3 class="sheet-title">Sorting Algorithms</h3>
        <p class="sheet-desc">Comparison and non-comparison sorts, complexities and stability.</p>
        <div class="sheet-footer">
          <span class="sheet-meta">8 topics</span>
          <div class="sheet-pills">
            <span class="pill">Merge</span>
            <span class="pill">Quick</span>
            <span class="pill">Heap</span>
          </div>
        </div>
      </a>

    </div>
  </section>
</main>

<script>
(function(){
  const root=document.documentElement;
  const btn=document.getElementById('themeToggle');
  const icon=document.getElementById('themeIcon');
  const lbl=document.getElementById('themeLabel');
  function sys(){return window.matchMedia('(prefers-color-scheme: dark)').matches;}
  function apply(dark){
    root.setAttribute('data-theme',dark?'dark':'light');
    icon.textContent=dark?'🌙':'☀️';
    lbl.textContent=dark?'Dark':'Light';
  }
  const saved=localStorage.getItem('theme');
  apply(saved?saved==='dark':sys());
  btn.addEventListener('click',function(){
    const next=root.getAttribute('data-theme')!=='dark';
    apply(next);
    localStorage.setItem('theme',next?'dark':'light');
  });
  window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change',function(e){
    if(!localStorage.getItem('theme'))apply(e.matches);
  });
})();
</script>
</body>
</html>
```
