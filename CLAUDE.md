# CLAUDE.md — iam-architect-roadmap

## What this is
A concept-first, vendor-neutral IAM Architect roadmap. Public repo, deployed to GitHub Pages
at https://arafatomer66.github.io/iam-architect-roadmap

## Stack
- Plain Markdown + Jekyll, `remote_theme: just-the-docs/just-the-docs`, built by GitHub Pages (no local build needed).
- Custom premium landing page: `_layouts/landing.html` (self-contained HTML/CSS/JS, used only by `index.md`).
  Stage cards on the landing page are driven by `_data/stages.yml`.
- Docs pages are skinned by `_sass/custom/custom.scss` (just-the-docs' documented extension point) +
  `_includes/head_custom.html` (fonts, favicon).
- Mermaid enabled in `_config.yml` — diagrams render on the site AND natively on GitHub.

## Content rules (see CONTRIBUTING.md)
- Every substantive page: **concept → mechanics → architect's lens**, ending with an "Architect's checklist".
- Vendor detail lives in `{: .vendor }` callouts and in `05-platform-landscape/` only — never woven into concept pages.
- Callout classes available: `.note` `.concept` `.architect` `.warning` `.vendor` (defined in `_config.yml`).
- Front matter is mandatory or the page won't appear in nav:
  `title:`, `parent:` (exact title of the section index page), `nav_order:`.
  Section index pages use `has_children: true` and no `parent`.
- Cross-link with relative `.md` links — `jekyll-relative-links` rewrites them for the site, and they work on GitHub.

## Section titles (must match `parent:` exactly)
Start Here · 1. IT Fundamentals · 2. Identity Fundamentals · 3. Identity Domains ·
4. Architecture Practice · 5. Platform Landscape · 6. Business & Risk ·
7. Delivery & Operations · 8. The Frontier · 9. Practice · 10. Reference

## Gotchas
- `README.md` has `nav_exclude: true` — `index.md` is the site home, README is the GitHub landing.
- Page counts are quoted in `README.md`, `index.md`, `_data/stages.yml` and the landing stats. Update all of them together.
- Don't add external JS/CSS beyond Google Fonts; keep pages readable offline in the repo.
