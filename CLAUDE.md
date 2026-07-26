# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, single-page HTML résumé site for GitHub Pages (`jong-bae.github.io`). There is no
backend, no build system, and no package manager — just HTML/SCSS/JS served directly. The site is
based on the "Draco" HTML resume template (CC BY-NC-SA 4.0) and has been substantially customized
with Korean-language résumé content, a contact-info section, and a two-column work-experience
layout.

## Working with SCSS (no CLI build step)

CSS is authored in `scss/` and compiled to `css/` via the VS Code **Live Sass Compile** extension
(config in `.vscode/settings.json`), not a CLI tool — there is no `package.json`/npm/gulp in this
repo.

- Edit source: `scss/custom.scss` (imports `scss/partials/_variables.scss`, `_mixins.scss`,
  `_base.scss`, `_responsive.scss`, and `scss/vendors/_typed.scss`).
- On save, the extension writes **two** output files to `css/`: an expanded `custom.css` and a
  compressed `custom.min.css`.
- `index.html` links `css/custom.min.css` — if you edit SCSS without the Live Sass Compile watcher
  running, regenerate both CSS files manually (e.g. `sass scss/custom.scss css/custom.css` and a
  compressed pass to `css/custom.min.css`) so the compiled output doesn't drift from source.
- Never hand-edit `css/custom.css` / `css/custom.min.css` directly — they are compiled artifacts;
  edit the `.scss` source instead. (`css/kube.min.css`, `css/all.min.css`, `css/font-awesome.min.css`,
  `css/pretendard.css` are third-party/vendor CSS and normally shouldn't need changes.)
- Colors/fonts are centralized in `scss/partials/_variables.scss` ($primary, $secondary, $lato,
  $playfair, $custom).

## Architecture of `index.html`

Everything lives in one file, structured as stacked full-width `<div class="section">` blocks tied
to nav anchors (`#about`, `#experiences`, `#achievements`, `#skills`):

1. **`.main-nav`** — top nav bar with anchor links to each section below.
2. **`.intro#about`** — name/photo, summary blurb, and `.intro-contact` (email/phone/GitHub/blog).
   The name renders via `js/typed.min.js` (a typing animation) — the displayed string is set inline
   in the `<script>` at the bottom of `index.html` (`strings: ["PARK JONG BAE"]"`), not in the
   `<h1>` markup itself.
3. **`.work#experiences`** — work history. Each employer is a `.work-container` split into
   `.work-left` (company/dates/role) and `.work-right` (one `.project` block per project, each with
   a title, date range, and a bulleted `<ul>` of achievements). This two-column layout and its
   mobile stacking are custom additions layered into `scss/custom.scss` (not part of the original
   Draco template) — see the `.work-container` rule and the `@media (max-width: 768px)` block at
   the bottom of that file.
4. **`.award#achievements`** — flat list of awards (`.award-list`).
5. **`.skills#skills`** — flat list of skill categories (`.skill-list`).
6. **`.quote`** and **`footer`** — closing tagline and social links.

jQuery (`js/jquery.min.js`) plus small single-purpose plugins in `js/` (`typed.min.js`, `kube.min.js`
for the Kube CSS framework's JS behaviors, `site.js` for smooth-scroll on anchor links) drive the
minimal interactivity. Most files in `js/` (accordion, tabs, modal, tooltip, etc.) are unused Kube
framework extras carried over from the template — check whether a component actually appears in
`index.html`/`custom.scss` before assuming a given `js/*.js` file is live.

## Making content edits

Résumé content (jobs, achievements, skills, awards) is plain HTML text inside `index.html` — edit
it directly there. There are no templates, data files, or generators.

## Deployment

This is a GitHub Pages user site: pushing to `main` publishes directly, there is no CI/build step.
