# Prompt Used to Generate the Binary Tree Revision Sheet

This is the exact instruction given to the agent. Copy, adapt the topic-specific parts, and provide new notes to produce a new sheet in the same design system.

---

## System / Design Instruction Block

```
Convert the study notes below into a clean, self-contained HTML revision sheet.
Follow these requirements exactly:

**Structure:**
- A sticky top navbar with anchor jump links to every topic section
- Each topic in its own <section> card
- Subsections for: Definition, Key Points, Formulas/Rules, Use Cases, Things to Remember

**Styling (embed everything in a <style> tag — no external libraries):**
- Implement both light and dark mode using `prefers-color-scheme` media query
- A manual dark/light toggle button in the navbar as well

Light mode colors:
- Background: #f5f5f0  (warm off-white, not pure white)
- Body text:  #2d2d2d  (soft dark, not pure black)
- Card background: #ffffff
- Accent/border: muted blues and greens like #4a7c8e or #5a8a6a
- Definition blockquote: light warm tint like #fdf6ec

Dark mode colors:
- Background:  #1a1a2e  (deep navy, not pure black)
- Body text:   #e0e0d0  (warm off-white, easy on eyes)
- Card background: #222235
- Accent/border: muted teal and sage like #5ba3b0 or #6aab7a
- Definition blockquote: subtle dark tint like #2a2a3e
- Code/formula blocks: #2e2e45 background with #c9d1d9 text

**Typography and layout:**
- Font: system-ui or Georgia — nothing harsh
- Line height: 1.7 for body text
- Comfortable padding inside cards
- Smooth transition between modes using `transition: background 0.3s, color 0.3s`
- Each topic card has a colored left border
- Formulas in a styled <table> or <code> block
- Key points as a clean <ul> list
- Definition inside a styled <blockquote>
- Fully mobile-friendly layout

**Rules:**
- Pure HTML + CSS + minimal vanilla JS (only for the toggle button) — no frameworks, no CDN
- Everything in a single .html file
- Skip any subsection that has no content for a topic
- The toggle button must remember the user's preference using localStorage

Here are the notes:

[PASTE YOUR STUDY NOTES HERE]
```

---

## How to Reuse This Prompt

1. Copy the entire block above.
2. Replace `[PASTE YOUR STUDY NOTES HERE]` with your new topic's notes.
3. Keep your notes structured with clear topic headers and sub-headers matching the five subsection names: **One-liner definition**, **Key points / properties**, **Important formulas, rules, or metrics**, **Common use cases**, **Things to remember / common mistakes**.
4. Submit to the agent. The design system will be reproduced identically.

---

## Notes Format That Works Best

Structure each topic in your source notes like this for cleanest parsing:

```
# Topic Title

**One-liner definition**
A concise single sentence.

**Key points / properties**
- Point one
- Point two
- **Term**: explanation

**Important formulas, rules, or metrics**
- Label : formula or value
- Label : formula or value

**Common use cases or real-world applications**
Prose or bullet points.

**Things to remember / common mistakes**
Warning or tip text.
```

Topics without certain subsections are fine — the agent will skip those sub-labels automatically.

---

## What the Agent Will Produce

Given the prompt above and structured notes, the agent generates a single `.html` file with:

| Feature | Detail |
|---|---|
| Sticky navbar | Anchor links to every `<section>`, brand label, theme toggle |
| Page header | `h1` title, subtitle, topic tag pills |
| Topic cards | One per topic; left border rotates blue-teal → sage → violet |
| Definition | Italic `<blockquote>` with warm-tint background |
| Key points | `<ul>` with `▸` bullet, accent-coloured per card |
| Formulas | Two-column `<table>` with monospace value cells |
| Comparison data | Three-column `<table>` with coloured header row |
| Use cases | Plain prose paragraph |
| Remember box | Amber left-border callout with emoji icon |
| Light / dark | Auto (OS) + manual toggle stored in `localStorage` |
| Mobile | Responsive at 600px breakpoint |
| Dependencies | Zero — fully self-contained single file |
