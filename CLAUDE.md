# CLAUDE.md — benferguson.com-2.0

AI assistant context for the `BenJFerguson/benferguson.com-2.0` repository.

---

## Project Overview

Personal portfolio website for Ben Ferguson — a Supply Chain Management student (University of Arkansas, graduating December 2026). The site showcases his background, Lean Six Sigma certifications, and resume. There is no backend, no build system, and no package manager. Everything is plain static HTML/CSS/JS served directly.

---

## Repository Structure

```
benferguson.com-2.0/
├── index.html                        # Main single-page portfolio
├── resume.html                       # Standalone HTML resume page
├── headshot.jpg                      # Profile photo used in index.html
├── headhsot.image.jpg                # Duplicate/original upload (typo in filename)
├── headshot.svg                      # SVG monogram fallback placeholder
├── BenFergusonResume.pdf             # Resume PDF (used in the modal viewer)
├── Ben_Ferguson_Resume 2026 (1).pdf  # Duplicate resume upload (not referenced in HTML)
├── green-belt-cert.png               # Green Belt cert image referenced in HTML
├── yellow-belt-cert.png              # Yellow Belt cert image referenced in HTML
├── LSSGI Green Belt Cert.png         # Original upload of green belt cert (not referenced)
├── LSSGI Yellow Belt Cert.png        # Original upload of yellow belt cert (not referenced)
└── README.md                         # Minimal one-line description
```

**No build artifacts, no node_modules, no package.json, no config files.**

---

## Tech Stack

| Layer      | Technology                          |
|------------|-------------------------------------|
| Markup     | Plain HTML5 (`<!DOCTYPE html>`)     |
| Styling    | Inline `<style>` in each HTML file  |
| Scripting  | Vanilla JavaScript (no libraries)   |
| Fonts      | Google Fonts (Montserrat, Poppins)  |
| Hosting    | Static file server / GitHub Pages   |

---

## Design System

All design tokens are CSS custom properties defined in `:root` inside `index.html`.

### Color Palette

| Variable        | Light mode  | Dark mode   | Role                          |
|-----------------|-------------|-------------|-------------------------------|
| `--pale-sand`   | `#EDE6D1`   | `#22343F`   | Hero gradient top             |
| `--mist-blue`   | `#CFD9DC`   | `#2B4A55`   | Hero gradient bottom, borders |
| `--seafoam`     | `#A7C6C9`   | `#548CA8`   | Accents, section divider line |
| `--deep-ocean`  | `#446A77`   | `#1A2F38`   | Primary CTA, nav pills, links |
| `--pearl`       | `#F9F9F7`   | `#0F2027`   | Page background               |
| `--text-main`   | `#2C3E50`   | `#EAF6F6`   | Body text                     |
| `--text-light`  | `#F9F9F7`   | `#F9F9F7`   | Text on dark backgrounds      |

Dark mode is activated by adding `data-theme="dark"` to `<body>`. The JavaScript toggle in the contact section flips between `"dark"` and `"light"`.

### Typography

- **Headings (`h1`, `h2`, `h3`):** Montserrat (weights 600, 700, 800) — loaded from Google Fonts
- **Body text:** Poppins (weights 300, 400, 500) — loaded from Google Fonts
- Hero title uses `clamp(3rem, 6vw, 5rem)` for fluid responsive sizing

### Badge Colors

| Class           | Background | Text  | Used for          |
|-----------------|------------|-------|-------------------|
| `.badge-yellow` | `#F4D03F`  | dark  | Yellow Belt card  |
| `.badge-green`  | `#2ECC71`  | white | Green Belt card   |
| `.badge-black`  | `#2C3E50`  | white | Black Belt card   |

---

## Page Sections (index.html)

1. **Scroll progress bar** — Fixed `6px` bar at very top of viewport. Updates on scroll via `requestAnimationFrame` for performance. Gradient from `--seafoam` to `--deep-ocean`.

2. **Hero** (`header.hero`) — Full-viewport height, centered. Contains name, subtitle, tagline, and a "View My Work" CTA that smooth-scrolls to `#nav`.

3. **Nav pills** (`#nav`) — Vertical stack of pill-shaped links (max-width 500px) pointing to `#about`, `#projects`, the resume modal, LinkedIn, and `#contact`.

4. **About Me** (`#about`) — Headshot image + short bio paragraph side by side. Image is `180×180px`, `border-radius: 50%`.

5. **Projects / Lean Six Sigma** (`#projects`) — Three-column responsive card grid (`auto-fit, minmax(280px, 1fr)`). Cards: Yellow Belt, Green Belt (links to Notion project page), Black Belt (in progress).

6. **Contact** (`#contact`) — Email link, LinkedIn link, and the dark/light theme toggle button.

7. **Footer** — Single copyright line.

8. **Resume modal** (`#resumeModal`) — Full-screen overlay opened by the "Resume" nav pill. Embeds `BenFergusonResume.pdf` via `<embed>`. Closed by clicking "X" or clicking outside the modal content area.

---

## JavaScript Behaviour

All scripts are in a single inline `<script>` block at the bottom of `index.html`.

```
resumeLink.onclick  →  modal.classList.add("active")
closeResume.onclick →  modal.classList.remove("active")
modal.onclick (bg)  →  modal.classList.remove("active")

toggleTheme.onclick →  body.dataset.theme toggles "dark" / "light"

scroll event        →  requestAnimationFrame → updates scrollBar.style.width
                        calculated as (scrollTop / (scrollHeight - clientHeight)) * 100 + "%"
resize event        →  recalculates scroll progress width
```

No external JavaScript libraries are used.

---

## Known File Notes

- `headhsot.image.jpg` — filename has a typo ("headhsot"). `index.html` references the correctly-named `headshot.jpg` (both files exist and are identical).
- `Ben_Ferguson_Resume 2026 (1).pdf` — not referenced in any HTML; the modal uses `BenFergusonResume.pdf`.
- `LSSGI Green Belt Cert.png` / `LSSGI Yellow Belt Cert.png` — originals with spaces in name; `index.html` references the renamed copies `green-belt-cert.png` / `yellow-belt-cert.png`.
- Avoid adding more files with spaces in their names; prefer `kebab-case` for all assets.

---

## Development Workflow

There is no build step. To work on the site:

1. Open `index.html` directly in a browser, or serve with any static server:
   ```bash
   python3 -m http.server 8080
   # or
   npx serve .
   ```
2. Edit HTML/CSS/JS directly in `index.html` or `resume.html`.
3. Refresh the browser to see changes.

There are no linting, testing, or compilation steps.

---

## Conventions for AI Assistants

- **No build system** — do not add `package.json`, bundlers, or frameworks without explicit instruction.
- **No external CSS frameworks** — the site uses hand-written CSS. Do not introduce Bootstrap, Tailwind, etc.
- **Inline styles** — section-level layout tweaks use inline `style=""` attributes (e.g., `display:flex;flex-wrap:wrap`). This is intentional for a small static site.
- **CSS variables first** — always use the existing `--variable` tokens for colors rather than hardcoding hex values.
- **Dark theme coverage** — any new element that changes color/background must have a corresponding `[data-theme="dark"]` rule; otherwise it will look broken in dark mode.
- **Asset naming** — use `kebab-case` with no spaces for all new image/asset files.
- **Accessibility** — images should have meaningful `alt` text. Interactive elements (modals, toggles) should remain keyboard-accessible.
- **Performance** — the scroll progress bar uses `requestAnimationFrame` + a `ticking` guard; follow this pattern for any other scroll/resize listeners.
- **Single HTML file** — keep all CSS and JS inside `index.html` unless there is a strong reason to split. The site is intentionally simple.
- **No unused files** — do not leave unreferenced duplicates in the repo.

---

## Git Branch Strategy

- Default branch: `master`
- AI/Claude feature branches follow the pattern: `claude/claude-md-<session-id>`
- Commit messages use conventional-commit prefixes: `feat:`, `fix:`, `docs:`, `chore:`

---

## Contact & Links

| Item       | Value                                                          |
|------------|----------------------------------------------------------------|
| Email      | benfergusoncollege@gmail.com                                   |
| LinkedIn   | https://linkedin.com/in/benjamin-ferguson-03186426b            |
| Notion     | Green Belt project linked from the Green Belt card in index.html |
| Resume PDF | `BenFergusonResume.pdf` (embedded in resume modal)            |
