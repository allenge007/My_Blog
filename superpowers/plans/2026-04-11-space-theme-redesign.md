# Space Theme Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the MkDocs Material site with a NASA aerospace industrial + cinematic hybrid theme, featuring dual light/dark mode, new typography, and a homepage Hero with two layout options.

**Architecture:** Rewrite `cosmic.css` with new color variables and component styles. Add `fonts.css` for Google Fonts import. Create `overrides/main.html` to inject Hero HTML on the homepage. Update `mkdocs.yml` palette config to match new colors. All styling is pure CSS — no JS beyond the existing font-adjuster and a small scroll-arrow animation.

**Tech Stack:** MkDocs Material (Jinja2 templates), CSS3, Google Fonts

**Spec:** `docs/superpowers/specs/2026-04-11-space-theme-redesign-design.md`

---

## File Structure

| File | Action | Responsibility |
|------|--------|----------------|
| `docs/stylesheets/fonts.css` | Create | Google Fonts import + base font-family overrides |
| `docs/stylesheets/cosmic.css` | Rewrite | All color variables, component styles, starfield, Hero styles |
| `mkdocs.yml` | Modify | Palette config, extra_css order, primary/accent colors |
| `docs/overrides/main.html` | Create | Homepage Hero template (two layouts) |
| `docs/index.md` | Modify | Add `template` and `hero_layout` meta, restructure for Hero |
| `docs/assets/hero/` | Create dir | Placeholder directory for user-provided space images |

Files **not** changed:
- `docs/overrides/partials/header.html` — keep as-is (font-sizer buttons work independently)
- `docs/javascripts/font-adjuster.js` — keep as-is
- `docs/javascripts/katex.js` — keep as-is

---

### Task 1: Font System Setup

**Files:**
- Create: `docs/stylesheets/fonts.css`
- Modify: `mkdocs.yml`

- [ ] **Step 1: Create `docs/stylesheets/fonts.css`**

```css
/* ============================================================
   Font Imports & Base Typography
   ============================================================ */
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500;600;700&family=Noto+Sans+SC:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');

:root {
  --md-text-font: "Noto Sans SC", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --md-code-font: "JetBrains Mono", "SFMono-Regular", Consolas, monospace;
}

/* Orbitron for h1/h2 English headings — Chinese will fallback to Noto Sans SC */
.md-typeset h1,
.md-typeset h2 {
  font-family: "Orbitron", "Noto Sans SC", sans-serif;
  letter-spacing: 0.02em;
}

.md-typeset h3,
.md-typeset h4,
.md-typeset h5,
.md-typeset h6 {
  font-family: "Noto Sans SC", sans-serif;
}

/* Line heights */
.md-typeset {
  line-height: 1.7;
}

.md-typeset h1,
.md-typeset h2,
.md-typeset h3,
.md-typeset h4 {
  line-height: 1.3;
}

.md-typeset pre,
.md-typeset code {
  line-height: 1.6;
}
```

- [ ] **Step 2: Update `mkdocs.yml` to add fonts.css**

In the `extra_css` section, add `stylesheets/fonts.css` **before** `cosmic.css` so font variables are available:

Change:
```yaml
extra_css:
  - stylesheets/cosmic.css
  - https://unpkg.com/katex@0/dist/katex.min.css
```

To:
```yaml
extra_css:
  - stylesheets/fonts.css
  - stylesheets/cosmic.css
  - https://unpkg.com/katex@0/dist/katex.min.css
```

- [ ] **Step 3: Update `mkdocs.yml` palette config**

Replace the current `palette` section under `theme`:

```yaml
  palette:
    # Dark Mode - Space
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      toggle:
        icon: material/weather-sunny
        name: 切换到浅色模式
      primary: custom
      accent: custom
    # Light Mode - Space
    - media: "(prefers-color-scheme: light)"
      scheme: default
      toggle:
        icon: material/weather-night
        name: 切换到深色模式
      primary: custom
      accent: custom
```

Note: `primary: custom` and `accent: custom` tells Material to not apply its own palette colors, deferring to our CSS variables.

- [ ] **Step 4: Verify fonts load**

Run: `cd /Volumes/WorkSpace/Code/05_templates/MkDocsMaterialDemo && mkdocs serve`

Open browser to `http://127.0.0.1:8000`. Open DevTools → Network tab → filter "fonts". Verify Orbitron, Noto Sans SC, and JetBrains Mono are loading. Check that h1/h2 use Orbitron and body text uses Noto Sans SC.

- [ ] **Step 5: Commit**

```bash
git add docs/stylesheets/fonts.css mkdocs.yml
git commit -m "feat: add font system with Orbitron, Noto Sans SC, JetBrains Mono"
```

---

### Task 2: Dark Mode Color Palette & Starfield Background

**Files:**
- Rewrite: `docs/stylesheets/cosmic.css` (lines 1-135 — color variables + starfield)

- [ ] **Step 1: Replace the top of `cosmic.css` with new dark mode color variables**

Replace everything from line 1 through the end of the `[data-md-color-scheme="default"]` block (line 56) with:

```css
/* ============================================================
   Space Theme for MkDocs Material
   NASA Industrial + Cinematic Hybrid
   ============================================================ */

/* --- Dark Mode (Slate) --- */
[data-md-color-scheme="slate"] {
  --md-default-bg-color: #0B0B10;
  --md-default-bg-color--light: #12131A;
  --md-default-bg-color--lighter: #1A1B26;
  --md-default-bg-color--lightest: #222436;

  --md-default-fg-color: rgba(248, 250, 252, 0.87);
  --md-default-fg-color--light: rgba(148, 163, 184, 0.54);
  --md-default-fg-color--lighter: rgba(148, 163, 184, 0.32);
  --md-default-fg-color--lightest: rgba(148, 163, 184, 0.12);

  --md-primary-fg-color: #3B82F6;
  --md-primary-fg-color--light: #60A5FA;
  --md-primary-fg-color--dark: #2563EB;
  --md-primary-bg-color: #E0EAFF;
  --md-primary-bg-color--light: #EFF4FF;

  --md-accent-fg-color: #22D3EE;
  --md-accent-fg-color--transparent: rgba(34, 211, 238, 0.1);
  --md-accent-bg-color: #22D3EE;
  --md-accent-bg-color--light: rgba(34, 211, 238, 0.1);

  --md-code-bg-color: #0D0E18;
  --md-code-fg-color: #E2E8F0;
  --md-code-hl-color: rgba(59, 130, 246, 0.15);

  --md-typeset-a-color: #22D3EE;

  --md-footer-bg-color: rgba(6, 8, 14, 0.85);
  --md-footer-bg-color--dark: rgba(4, 5, 10, 0.9);

  --md-admonition-bg-color: rgba(13, 14, 24, 0.7);
}

/* --- Light Mode (Default) --- */
[data-md-color-scheme="default"] {
  --md-default-bg-color: #F1F5F9;
  --md-default-bg-color--light: #E2E8F0;
  --md-default-bg-color--lighter: #CBD5E1;
  --md-default-bg-color--lightest: #94A3B8;

  --md-default-fg-color: rgba(15, 23, 42, 0.87);
  --md-default-fg-color--light: rgba(71, 85, 105, 0.54);
  --md-default-fg-color--lighter: rgba(71, 85, 105, 0.32);
  --md-default-fg-color--lightest: rgba(71, 85, 105, 0.12);

  --md-primary-fg-color: #2563EB;
  --md-primary-fg-color--light: #3B82F6;
  --md-primary-fg-color--dark: #1D4ED8;
  --md-primary-bg-color: #FFFFFF;
  --md-primary-bg-color--light: #F8FAFC;

  --md-accent-fg-color: #0891B2;
  --md-accent-fg-color--transparent: rgba(8, 145, 178, 0.1);
  --md-accent-bg-color: #0891B2;
  --md-accent-bg-color--light: rgba(8, 145, 178, 0.1);

  --md-code-bg-color: #E8ECF2;
  --md-code-fg-color: #1E293B;
  --md-code-hl-color: rgba(37, 99, 235, 0.1);

  --md-typeset-a-color: #2563EB;

  --md-footer-bg-color: #E2E8F0;
  --md-footer-bg-color--dark: #CBD5E1;

  --md-admonition-bg-color: rgba(255, 255, 255, 0.8);
}
```

- [ ] **Step 2: Replace the starfield background section**

Replace the old starfield section (the `[data-md-color-scheme="slate"] .md-main` rules through the `@keyframes twinkle`) with:

```css
/* ============================================================
   Starfield Background (Dark mode only)
   ============================================================ */
[data-md-color-scheme="slate"] .md-main {
  position: relative;
}

[data-md-color-scheme="slate"] .md-main::before {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  pointer-events: none;
  background:
    /* Distant tiny stars — white */
    radial-gradient(1px 1px at 8% 15%, rgba(248, 250, 252, 0.6), transparent),
    radial-gradient(1px 1px at 22% 42%, rgba(248, 250, 252, 0.5), transparent),
    radial-gradient(1px 1px at 37% 8%, rgba(248, 250, 252, 0.4), transparent),
    radial-gradient(1px 1px at 52% 58%, rgba(248, 250, 252, 0.55), transparent),
    radial-gradient(1px 1px at 68% 32%, rgba(248, 250, 252, 0.5), transparent),
    radial-gradient(1px 1px at 83% 72%, rgba(248, 250, 252, 0.4), transparent),
    radial-gradient(1px 1px at 12% 78%, rgba(248, 250, 252, 0.5), transparent),
    radial-gradient(1px 1px at 58% 88%, rgba(248, 250, 252, 0.35), transparent),
    radial-gradient(1px 1px at 91% 12%, rgba(248, 250, 252, 0.55), transparent),
    radial-gradient(1px 1px at 33% 66%, rgba(248, 250, 252, 0.3), transparent),
    radial-gradient(1px 1px at 4% 52%, rgba(248, 250, 252, 0.45), transparent),
    radial-gradient(1px 1px at 46% 28%, rgba(248, 250, 252, 0.4), transparent),
    radial-gradient(1px 1px at 76% 50%, rgba(248, 250, 252, 0.5), transparent),
    radial-gradient(1px 1px at 94% 85%, rgba(248, 250, 252, 0.3), transparent),
    radial-gradient(1px 1px at 16% 93%, rgba(248, 250, 252, 0.4), transparent),
    /* Brighter closer stars */
    radial-gradient(1.5px 1.5px at 28% 12%, rgba(226, 232, 240, 0.75), transparent),
    radial-gradient(1.5px 1.5px at 63% 48%, rgba(226, 232, 240, 0.65), transparent),
    radial-gradient(1.5px 1.5px at 48% 78%, rgba(226, 232, 240, 0.75), transparent),
    radial-gradient(1.5px 1.5px at 80% 22%, rgba(226, 232, 240, 0.65), transparent),
    radial-gradient(1.5px 1.5px at 6% 40%, rgba(226, 232, 240, 0.55), transparent),
    /* Colored accent stars — launch blue & aurora cyan */
    radial-gradient(1.5px 1.5px at 18% 53%, rgba(59, 130, 246, 0.6), transparent),
    radial-gradient(1.5px 1.5px at 73% 17%, rgba(34, 211, 238, 0.5), transparent),
    radial-gradient(1.5px 1.5px at 43% 92%, rgba(59, 130, 246, 0.5), transparent),
    /* Deep space gradient base */
    linear-gradient(
      160deg,
      #0B0B10 0%,
      #0F1225 40%,
      #0B1020 70%,
      #0B0B10 100%
    );
}

/* Twinkling animation — restrained */
[data-md-color-scheme="slate"] .md-main::after {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  pointer-events: none;
  background:
    radial-gradient(2px 2px at 20% 30%, rgba(248, 250, 252, 0.85), transparent),
    radial-gradient(2px 2px at 56% 70%, rgba(34, 211, 238, 0.7), transparent),
    radial-gradient(2px 2px at 85% 45%, rgba(248, 250, 252, 0.8), transparent),
    radial-gradient(2px 2px at 41% 10%, rgba(59, 130, 246, 0.7), transparent),
    radial-gradient(2px 2px at 70% 89%, rgba(248, 250, 252, 0.75), transparent);
  animation: twinkle 4s ease-in-out infinite alternate;
}

@keyframes twinkle {
  0%   { opacity: 0.5; }
  50%  { opacity: 0.8; }
  100% { opacity: 0.5; }
}
```

- [ ] **Step 3: Verify dark mode colors and starfield**

Run: `mkdocs serve`

Open browser, switch to dark mode. Verify:
- Background is deep space black (`#0B0B10`), not the old purple-tinted black
- Star points visible with blue/cyan accent stars
- Twinkling is subtle (not flashy)
- Text is bright white, readable

- [ ] **Step 4: Commit**

```bash
git add docs/stylesheets/cosmic.css
git commit -m "feat: new dark/light color palette and restrained starfield background"
```

---

### Task 3: Header, Tabs & Sidebar Styling

**Files:**
- Modify: `docs/stylesheets/cosmic.css` (replace header/tabs/sidebar sections)

- [ ] **Step 1: Replace the Header, Tabs, and Sidebar sections in `cosmic.css`**

Remove the old header section (from `/* Header - Frosted Glass */` through the sidebar section ending with `.md-nav__item--active`). Replace with:

```css
/* ============================================================
   Header - Frosted Glass
   ============================================================ */
[data-md-color-scheme="slate"] .md-header {
  background: rgba(11, 11, 16, 0.75);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid rgba(59, 130, 246, 0.15);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
}

[data-md-color-scheme="default"] .md-header {
  background: rgba(241, 245, 249, 0.8);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

/* Tabs */
[data-md-color-scheme="slate"] .md-tabs {
  background: rgba(11, 11, 16, 0.6);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(59, 130, 246, 0.08);
}

[data-md-color-scheme="default"] .md-tabs {
  background: rgba(241, 245, 249, 0.7);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(37, 99, 235, 0.06);
}

[data-md-color-scheme="slate"] .md-tabs__link--active,
[data-md-color-scheme="slate"] .md-tabs__link:hover {
  color: #22D3EE;
}

[data-md-color-scheme="default"] .md-tabs__link--active,
[data-md-color-scheme="default"] .md-tabs__link:hover {
  color: #2563EB;
}

/* ============================================================
   Sidebar
   ============================================================ */
[data-md-color-scheme="slate"] .md-sidebar {
  background: transparent;
}

[data-md-color-scheme="slate"] .md-nav__link:hover {
  color: #22D3EE;
}

[data-md-color-scheme="slate"] .md-nav__link--active {
  color: #22D3EE;
  font-weight: 600;
}

[data-md-color-scheme="slate"] .md-nav__item--active > .md-nav__link {
  color: #22D3EE;
}

[data-md-color-scheme="default"] .md-nav__link:hover {
  color: #2563EB;
}

[data-md-color-scheme="default"] .md-nav__link--active {
  color: #2563EB;
  font-weight: 600;
}

[data-md-color-scheme="default"] .md-nav__item--active > .md-nav__link {
  color: #2563EB;
}
```

- [ ] **Step 2: Verify header and sidebar**

Run: `mkdocs serve`

Check both dark and light mode:
- Header has frosted glass effect
- Tab active/hover colors are correct (cyan in dark, blue in light)
- Sidebar links highlight correctly
- Toggle between dark/light — both should look polished

- [ ] **Step 3: Commit**

```bash
git add docs/stylesheets/cosmic.css
git commit -m "feat: header, tabs, sidebar styling for both light and dark modes"
```

---

### Task 4: Content Area Styling

**Files:**
- Modify: `docs/stylesheets/cosmic.css` (replace content area section)

- [ ] **Step 1: Replace the Content Area section in `cosmic.css`**

Remove the old content area section (headings, links, code blocks). Replace with:

```css
/* ============================================================
   Content Area
   ============================================================ */

/* Content container */
[data-md-color-scheme="slate"] .md-content {
  background: rgba(11, 11, 16, 0.4);
  border-radius: 4px;
}

[data-md-color-scheme="default"] .md-content {
  background: rgba(255, 255, 255, 0.6);
  border-radius: 4px;
}

/* Headings — Dark */
[data-md-color-scheme="slate"] .md-typeset h1 {
  color: #F8FAFC;
  text-shadow: 0 0 20px rgba(59, 130, 246, 0.3);
}

[data-md-color-scheme="slate"] .md-typeset h2 {
  color: #E2E8F0;
  border-bottom: 1px solid transparent;
  border-image: linear-gradient(to right, #3B82F6, transparent) 1;
  padding-bottom: 0.3em;
}

[data-md-color-scheme="slate"] .md-typeset h3,
[data-md-color-scheme="slate"] .md-typeset h4 {
  color: #CBD5E1;
}

/* Headings — Light */
[data-md-color-scheme="default"] .md-typeset h1 {
  color: #0F172A;
}

[data-md-color-scheme="default"] .md-typeset h2 {
  color: #1E293B;
  border-bottom: 1px solid transparent;
  border-image: linear-gradient(to right, #2563EB, transparent) 1;
  padding-bottom: 0.3em;
}

[data-md-color-scheme="default"] .md-typeset h3,
[data-md-color-scheme="default"] .md-typeset h4 {
  color: #334155;
}

/* Links — Dark */
[data-md-color-scheme="slate"] .md-typeset a {
  color: #22D3EE;
  text-decoration: none;
  transition: color 0.2s, text-shadow 0.2s;
}

[data-md-color-scheme="slate"] .md-typeset a:hover {
  color: #67E8F9;
  text-shadow: 0 0 8px rgba(34, 211, 238, 0.4);
}

/* Links — Light */
[data-md-color-scheme="default"] .md-typeset a {
  color: #2563EB;
  text-decoration: none;
  transition: color 0.2s;
}

[data-md-color-scheme="default"] .md-typeset a:hover {
  color: #1D4ED8;
}

/* Code blocks — Dark */
[data-md-color-scheme="slate"] .md-typeset code {
  background: rgba(13, 14, 24, 0.8);
  border: 1px solid rgba(59, 130, 246, 0.15);
  color: #E2E8F0;
}

[data-md-color-scheme="slate"] .md-typeset pre {
  background: rgba(13, 14, 24, 0.9);
  border: 1px solid rgba(59, 130, 246, 0.12);
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
}

[data-md-color-scheme="slate"] .md-typeset pre > code {
  background: transparent;
  border: none;
}

/* Code blocks — Light */
[data-md-color-scheme="default"] .md-typeset code {
  background: #E8ECF2;
  border: 1px solid rgba(37, 99, 235, 0.1);
  color: #1E293B;
}

[data-md-color-scheme="default"] .md-typeset pre {
  background: #E8ECF2;
  border: 1px solid rgba(37, 99, 235, 0.1);
  border-radius: 8px;
}

[data-md-color-scheme="default"] .md-typeset pre > code {
  background: transparent;
  border: none;
}
```

- [ ] **Step 2: Verify content styling**

Run: `mkdocs serve`

Navigate to a content-rich page (e.g., `/cs/os/chapter2/` or any page with code blocks). Check:
- h1 has blue glow in dark mode, clean in light mode
- h2 has gradient underline (blue → transparent)
- Links are cyan (dark) / blue (light) with hover effects
- Code blocks have rounded corners, correct backgrounds
- Inline code has border and correct color

- [ ] **Step 3: Commit**

```bash
git add docs/stylesheets/cosmic.css
git commit -m "feat: content area headings, links, code blocks for both modes"
```

---

### Task 5: Admonitions & Tables

**Files:**
- Modify: `docs/stylesheets/cosmic.css` (replace admonition + table sections)

- [ ] **Step 1: Replace Admonition and Table sections in `cosmic.css`**

Remove the old admonition and table sections. Replace with:

```css
/* ============================================================
   Admonitions - Glass Morphism
   ============================================================ */

/* Dark */
[data-md-color-scheme="slate"] .md-typeset .admonition,
[data-md-color-scheme="slate"] .md-typeset details {
  background: rgba(13, 14, 24, 0.6);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(59, 130, 246, 0.12);
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

[data-md-color-scheme="slate"] .md-typeset .admonition-title,
[data-md-color-scheme="slate"] .md-typeset summary {
  background: rgba(59, 130, 246, 0.08);
  border-bottom: 1px solid rgba(59, 130, 246, 0.1);
}

/* Light */
[data-md-color-scheme="default"] .md-typeset .admonition,
[data-md-color-scheme="default"] .md-typeset details {
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(37, 99, 235, 0.1);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

[data-md-color-scheme="default"] .md-typeset .admonition-title,
[data-md-color-scheme="default"] .md-typeset summary {
  background: rgba(37, 99, 235, 0.06);
  border-bottom: 1px solid rgba(37, 99, 235, 0.06);
}

/* ============================================================
   Tables
   ============================================================ */

/* Dark */
[data-md-color-scheme="slate"] .md-typeset table:not([class]) {
  border: 1px solid rgba(59, 130, 246, 0.15);
  border-radius: 8px;
  overflow: hidden;
}

[data-md-color-scheme="slate"] .md-typeset table:not([class]) th {
  background: rgba(59, 130, 246, 0.12);
  color: #E2E8F0;
}

[data-md-color-scheme="slate"] .md-typeset table:not([class]) tr:hover td {
  background: rgba(59, 130, 246, 0.06);
}

/* Light */
[data-md-color-scheme="default"] .md-typeset table:not([class]) {
  border: 1px solid rgba(37, 99, 235, 0.1);
  border-radius: 8px;
  overflow: hidden;
}

[data-md-color-scheme="default"] .md-typeset table:not([class]) th {
  background: rgba(37, 99, 235, 0.08);
  color: #1E293B;
}

[data-md-color-scheme="default"] .md-typeset table:not([class]) tr:hover td {
  background: rgba(37, 99, 235, 0.04);
}
```

- [ ] **Step 2: Verify admonitions and tables**

Run: `mkdocs serve`

Navigate to `/admonitions/` page. Check both dark and light mode:
- Admonitions have glass morphism effect (dark), clean card look (light)
- Rounded corners, correct borders
- Navigate to a page with tables (e.g., any OS chapter). Check th backgrounds and hover effect.

- [ ] **Step 3: Commit**

```bash
git add docs/stylesheets/cosmic.css
git commit -m "feat: admonition glass morphism and table styling for both modes"
```

---

### Task 6: Footer, Search, Scrollbar, Back-to-top, Content Tabs

**Files:**
- Modify: `docs/stylesheets/cosmic.css` (replace remaining component sections)

- [ ] **Step 1: Replace Footer, Search, Scrollbar, Back-to-top, Content Tabs, and Nebula sections**

Remove all remaining old sections (footer through the end of the file). Replace with:

```css
/* ============================================================
   Footer
   ============================================================ */
[data-md-color-scheme="slate"] .md-footer {
  background: rgba(6, 8, 14, 0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-top: 1px solid rgba(59, 130, 246, 0.1);
}

[data-md-color-scheme="default"] .md-footer {
  background: #E2E8F0;
  border-top: 1px solid rgba(37, 99, 235, 0.08);
}

/* ============================================================
   Search
   ============================================================ */
[data-md-color-scheme="slate"] .md-search__form {
  background: rgba(13, 14, 24, 0.8);
  border: 1px solid rgba(59, 130, 246, 0.15);
}

[data-md-color-scheme="slate"] .md-search__form:focus-within {
  border-color: rgba(34, 211, 238, 0.4);
}

[data-md-color-scheme="slate"] .md-search-result__link:hover {
  background: rgba(59, 130, 246, 0.1);
}

[data-md-color-scheme="default"] .md-search__form {
  background: #FFFFFF;
  border: 1px solid #CBD5E1;
}

[data-md-color-scheme="default"] .md-search__form:focus-within {
  border-color: #3B82F6;
}

[data-md-color-scheme="default"] .md-search-result__link:hover {
  background: rgba(37, 99, 235, 0.06);
}

/* ============================================================
   Scrollbar
   ============================================================ */
[data-md-color-scheme="slate"] ::-webkit-scrollbar {
  width: 6px;
}

[data-md-color-scheme="slate"] ::-webkit-scrollbar-track {
  background: rgba(11, 11, 16, 0.5);
}

[data-md-color-scheme="slate"] ::-webkit-scrollbar-thumb {
  background: rgba(59, 130, 246, 0.3);
  border-radius: 3px;
}

[data-md-color-scheme="slate"] ::-webkit-scrollbar-thumb:hover {
  background: rgba(59, 130, 246, 0.5);
}

[data-md-color-scheme="default"] ::-webkit-scrollbar {
  width: 6px;
}

[data-md-color-scheme="default"] ::-webkit-scrollbar-track {
  background: #F1F5F9;
}

[data-md-color-scheme="default"] ::-webkit-scrollbar-thumb {
  background: #94A3B8;
  border-radius: 3px;
}

[data-md-color-scheme="default"] ::-webkit-scrollbar-thumb:hover {
  background: #64748B;
}

/* ============================================================
   Back-to-top Button
   ============================================================ */
[data-md-color-scheme="slate"] .md-top {
  background: rgba(59, 130, 246, 0.2);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(59, 130, 246, 0.2);
  color: #22D3EE;
}

[data-md-color-scheme="slate"] .md-top:hover {
  background: rgba(59, 130, 246, 0.35);
  box-shadow: 0 0 12px rgba(34, 211, 238, 0.3);
}

[data-md-color-scheme="default"] .md-top {
  background: rgba(37, 99, 235, 0.1);
  border: 1px solid rgba(37, 99, 235, 0.15);
  color: #2563EB;
}

[data-md-color-scheme="default"] .md-top:hover {
  background: rgba(37, 99, 235, 0.2);
  box-shadow: 0 0 8px rgba(37, 99, 235, 0.15);
}

/* ============================================================
   Content Tabs
   ============================================================ */
[data-md-color-scheme="slate"] .md-typeset .tabbed-labels > label {
  color: rgba(226, 232, 240, 0.6);
}

[data-md-color-scheme="slate"] .md-typeset .tabbed-labels > label--active,
[data-md-color-scheme="slate"] .md-typeset .tabbed-labels > label:hover {
  color: #22D3EE;
}

[data-md-color-scheme="default"] .md-typeset .tabbed-labels > label {
  color: rgba(71, 85, 105, 0.6);
}

[data-md-color-scheme="default"] .md-typeset .tabbed-labels > label--active,
[data-md-color-scheme="default"] .md-typeset .tabbed-labels > label:hover {
  color: #2563EB;
}

/* ============================================================
   Reduced Motion
   ============================================================ */
@media (prefers-reduced-motion: reduce) {
  [data-md-color-scheme="slate"] .md-main::after {
    animation: none;
    opacity: 0.6;
  }
}
```

- [ ] **Step 2: Verify all remaining components**

Run: `mkdocs serve`

Check in both modes:
- Footer styling and border
- Search box focus state
- Scrollbar appearance (scroll on a long page)
- Back-to-top button (scroll down then up on a long page)
- Content tabs on `/content-tabs/`

- [ ] **Step 3: Commit**

```bash
git add docs/stylesheets/cosmic.css
git commit -m "feat: footer, search, scrollbar, back-to-top, content tabs, reduced-motion"
```

---

### Task 7: Homepage Hero — Layout A (Fullscreen)

**Files:**
- Create: `docs/overrides/main.html`
- Create: `docs/assets/hero/` (directory)
- Modify: `docs/index.md`
- Modify: `docs/stylesheets/cosmic.css` (append Hero styles)

- [ ] **Step 1: Create hero assets directory with a placeholder**

```bash
mkdir -p /Volumes/WorkSpace/Code/05_templates/MkDocsMaterialDemo/docs/assets/hero
```

Create a `.gitkeep` file so the directory is tracked:

```bash
touch /Volumes/WorkSpace/Code/05_templates/MkDocsMaterialDemo/docs/assets/hero/.gitkeep
```

- [ ] **Step 2: Create `docs/overrides/main.html`**

```html
{% extends "base.html" %}

{% block content %}
  {% if page.is_homepage %}
    {# ---- Hero Layout A: Fullscreen ---- #}
    <section class="hero hero--fullscreen" id="hero-fullscreen">
      <div class="hero__background">
        <img src="{{ 'assets/hero/space-hero.jpg' | url }}" alt="Space" class="hero__image" loading="eager" />
        <div class="hero__overlay"></div>
      </div>
      <div class="hero__content">
        <h1 class="hero__title">allenge 的小站</h1>
        <p class="hero__tagline">探索宇宙，记录思考</p>
        <a href="#page-content" class="hero__scroll-hint" aria-label="向下滚动">
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="6 9 12 15 18 9"></polyline>
          </svg>
        </a>
      </div>
    </section>

    {# ---- Hero Layout C: Split ---- #}
    <section class="hero hero--split" id="hero-split" style="display: none;">
      <div class="hero__image-side">
        <img src="{{ 'assets/hero/space-hero.jpg' | url }}" alt="Space" class="hero__image" loading="eager" />
      </div>
      <div class="hero__text-side">
        <h1 class="hero__title">allenge 的小站</h1>
        <p class="hero__tagline">探索宇宙，记录思考</p>
        <nav class="hero__nav">
          <a href="{{ 'blog/' | url }}" class="hero__nav-card">
            <span class="hero__nav-icon">&#x1F4DD;</span>
            <span class="hero__nav-label">博客</span>
          </a>
          <a href="{{ 'cs/' | url }}" class="hero__nav-card">
            <span class="hero__nav-icon">&#x1F4BB;</span>
            <span class="hero__nav-label">计算机</span>
          </a>
          <a href="{{ 'math/' | url }}" class="hero__nav-card">
            <span class="hero__nav-icon">&#x1F4D0;</span>
            <span class="hero__nav-label">数学</span>
          </a>
          <a href="{{ 'about/about_me/' | url }}" class="hero__nav-card">
            <span class="hero__nav-icon">&#x1F464;</span>
            <span class="hero__nav-label">关于</span>
          </a>
        </nav>
      </div>
    </section>

    <div id="page-content"></div>
  {% endif %}

  {{ super() }}
{% endblock %}
```

Note: Both layouts are included. Layout A is visible by default, Layout C is hidden (`display: none`). To switch, the user changes which one has `style="display: none;"` in the template. After the user chooses, we'll remove the unused layout.

- [ ] **Step 3: Append Hero CSS to `cosmic.css`**

Add the following at the end of `docs/stylesheets/cosmic.css`:

```css
/* ============================================================
   Homepage Hero — Layout A: Fullscreen
   ============================================================ */
.hero--fullscreen {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero--fullscreen .hero__background {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.hero--fullscreen .hero__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero--fullscreen .hero__overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(11, 11, 16, 0.2) 0%,
    rgba(11, 11, 16, 0.5) 60%,
    rgba(11, 11, 16, 0.85) 100%
  );
}

[data-md-color-scheme="default"] .hero--fullscreen .hero__overlay {
  background: linear-gradient(
    to bottom,
    rgba(241, 245, 249, 0.1) 0%,
    rgba(241, 245, 249, 0.5) 60%,
    rgba(241, 245, 249, 0.9) 100%
  );
}

.hero--fullscreen .hero__content {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 0 2rem;
}

.hero__title {
  font-family: "Orbitron", "Noto Sans SC", sans-serif;
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  color: #F8FAFC;
  text-shadow: 0 0 30px rgba(59, 130, 246, 0.4);
  margin-bottom: 0.5rem;
  letter-spacing: 0.05em;
}

[data-md-color-scheme="default"] .hero__title {
  color: #0F172A;
  text-shadow: none;
}

.hero__tagline {
  font-family: "Noto Sans SC", sans-serif;
  font-size: clamp(1rem, 2vw, 1.25rem);
  color: rgba(148, 163, 184, 0.87);
  margin-bottom: 2rem;
}

[data-md-color-scheme="default"] .hero__tagline {
  color: #475569;
}

.hero__scroll-hint {
  display: inline-block;
  color: rgba(248, 250, 252, 0.6);
  animation: bounce 2s ease-in-out infinite;
  text-decoration: none;
}

[data-md-color-scheme="default"] .hero__scroll-hint {
  color: rgba(71, 85, 105, 0.6);
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(8px); }
}

@media (prefers-reduced-motion: reduce) {
  .hero__scroll-hint {
    animation: none;
  }
}

/* ============================================================
   Homepage Hero — Layout C: Split
   ============================================================ */
.hero--split {
  display: flex;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.hero--split .hero__image-side {
  flex: 0 0 55%;
  position: relative;
  overflow: hidden;
}

.hero--split .hero__image-side .hero__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero--split .hero__text-side {
  flex: 0 0 45%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 3rem;
  background: #0B0B10;
}

[data-md-color-scheme="default"] .hero--split .hero__text-side {
  background: #F1F5F9;
}

.hero--split .hero__title {
  text-align: left;
}

.hero--split .hero__tagline {
  text-align: left;
}

.hero__nav {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 2rem;
}

.hero__nav-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 1.25rem;
  border: 1px solid rgba(59, 130, 246, 0.15);
  border-radius: 8px;
  background: rgba(26, 27, 38, 0.5);
  color: #E2E8F0;
  text-decoration: none;
  transition: border-color 0.2s, box-shadow 0.2s, background 0.2s;
}

.hero__nav-card:hover {
  border-color: #3B82F6;
  box-shadow: 0 0 16px rgba(59, 130, 246, 0.2);
  background: rgba(26, 27, 38, 0.8);
}

[data-md-color-scheme="default"] .hero__nav-card {
  border: 1px solid #CBD5E1;
  background: #FFFFFF;
  color: #1E293B;
}

[data-md-color-scheme="default"] .hero__nav-card:hover {
  border-color: #2563EB;
  box-shadow: 0 0 12px rgba(37, 99, 235, 0.12);
  background: #F8FAFC;
}

.hero__nav-icon {
  font-size: 1.5rem;
}

.hero__nav-label {
  font-family: "Noto Sans SC", sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
}

/* Split layout responsive — stack on mobile */
@media (max-width: 768px) {
  .hero--split {
    flex-direction: column;
  }

  .hero--split .hero__image-side {
    flex: 0 0 40vh;
  }

  .hero--split .hero__text-side {
    flex: 1;
    padding: 2rem;
  }

  .hero__nav {
    grid-template-columns: 1fr 1fr;
    gap: 0.75rem;
  }
}
```

- [ ] **Step 4: Update `docs/index.md` front matter**

Change the front matter of `docs/index.md` to also hide the table of contents on the homepage (it will be replaced by the Hero):

Change:
```yaml
---
hide:
  - navigation
title: 欢迎光临我的小站
---
```

To:
```yaml
---
hide:
  - navigation
  - toc
title: 欢迎光临我的小站
---
```

- [ ] **Step 5: Verify Hero Layout A**

Run: `mkdocs serve`

Open homepage. Since no hero image exists yet, the hero section will show a broken image. This is expected — the user will provide the image later. But verify:
- Hero section takes full viewport height
- Overlay gradient is visible
- Title "allenge 的小站" is centered with Orbitron font and blue glow
- Tagline visible below
- Scroll-down arrow bouncing at bottom
- Scrolling down reveals normal page content
- Switch to light mode — overlay turns white-ish

- [ ] **Step 6: Commit**

```bash
git add docs/overrides/main.html docs/stylesheets/cosmic.css docs/index.md docs/assets/hero/.gitkeep
git commit -m "feat: homepage Hero with fullscreen and split layouts"
```

---

### Task 8: Final Verification & Cleanup

**Files:**
- Review: all modified files
- Possibly fix: `docs/stylesheets/cosmic.css`

- [ ] **Step 1: Full visual review — dark mode**

Run: `mkdocs serve`

Open browser in dark mode. Walk through these pages and check each element:
1. Homepage — Hero renders (broken image OK, structure/overlay/text correct)
2. `/blog/` — blog list page, check header, tabs, cards
3. `/cs/os/chapter2/` — long content page with headings, code blocks, tables
4. `/admonitions/` — all admonition types
5. `/content-tabs/` — content tabs
6. `/math/signals_and_systems/chapter1/` — math formulas (KaTeX rendering)

Check: colors match spec, no old purple/cyan remnants, starfield visible, frosted glass on header.

- [ ] **Step 2: Full visual review — light mode**

Toggle to light mode. Repeat the same page walkthrough:
1. Background is `#F1F5F9` (not white, not purple)
2. No starfield visible
3. Header is frosted glass with light treatment
4. Links are blue, not cyan
5. Code blocks have light gray background
6. All text is readable

- [ ] **Step 3: Check for CSS specificity issues**

Open DevTools on any page. Inspect elements and check that the new CSS variables are being applied (not overridden by Material's built-in styles). Look for:
- `--md-primary-fg-color` should show `#3B82F6` (dark) or `#2563EB` (light)
- `--md-accent-fg-color` should show `#22D3EE` (dark) or `#0891B2` (light)
- No leftover `deep purple` or old cyan colors

If Material's palette overrides our custom values (because `primary: custom` may not be supported in all versions), we may need to use `primary: black` as a neutral base and override with CSS. Fix as needed.

- [ ] **Step 4: Verify `prefers-reduced-motion`**

In DevTools → Rendering → Emulate CSS media → `prefers-reduced-motion: reduce`. Verify:
- Starfield twinkling stops
- Hero scroll arrow stops bouncing

- [ ] **Step 5: Commit any fixes**

If any fixes were made:

```bash
git add docs/stylesheets/cosmic.css
git commit -m "fix: address visual issues found during review"
```

- [ ] **Step 6: Provide user with Hero image instructions**

Tell the user:
> Place your space/Earth/Artemis II image at `docs/assets/hero/space-hero.jpg`. Both Hero layouts reference this path. Supported formats: jpg, png, webp. Recommended resolution: at least 1920x1080 for fullscreen, 1200x1080 for split layout.
>
> To switch between Hero layouts:
> - In `docs/overrides/main.html`, find the `<section>` with `id="hero-fullscreen"` and `id="hero-split"`
> - Remove `style="display: none;"` from the layout you want
> - Add `style="display: none;"` to the layout you don't want
> - Once decided, delete the unused layout block entirely

---

## Summary

| Task | Description | Key Files |
|------|-------------|-----------|
| 1 | Font system setup | `fonts.css`, `mkdocs.yml` |
| 2 | Dark/light color palette + starfield | `cosmic.css` (top half) |
| 3 | Header, tabs, sidebar | `cosmic.css` |
| 4 | Content area (headings, links, code) | `cosmic.css` |
| 5 | Admonitions & tables | `cosmic.css` |
| 6 | Footer, search, scrollbar, misc components | `cosmic.css` |
| 7 | Homepage Hero (both layouts) | `main.html`, `cosmic.css`, `index.md` |
| 8 | Final verification & cleanup | All files |
