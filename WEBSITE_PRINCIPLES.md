# Website Principles and Architecture (for future AI-assisted maintenance)

Built from the GaryKing.org/mysite prompt (https://gking.harvard.edu/mysite/files/ACADEMIC_SITE_PROMPT.md).
The owner's information form is the source of truth. The design is modelled on George Yean's site
(https://georgeyean.github.io/, HTML5 UP "Future Imperfect"): uppercase letter-spaced Raleway headings,
slate accent, hairline paper posts with an Abstract toggle. It sits on the prompt's warm earth-tone palette
with Oakland University gold (#b59a57) as a thin-rule accent.

## Stack
- Hugo extended **0.148.2** (the prompt says 0.147.0, but Blox v0.10.0 requires ≥ 0.148.2) + Hugo Blox `blox-tailwind` v0.10.0 via Go module, vendored in `hugo-site/_vendor/`.
- Blox compiles Tailwind v4 via `css.TailwindCSS`. This needs `@tailwindcss/cli` and `@tailwindcss/typography` from npm (see `package.json`).
- Pagefind is built after Hugo in CI, over `public/`.
- GitHub Pages project site: `baseURL: https://matthewfails.github.io/matthewfails-site/`. CI does NOT override baseURL (`configure-pages` can emit http://).

## Layout ownership
Most rendering is project-owned; Blox supplies the Tailwind pipeline, colour-theme plumbing, and helpers.
- `layouts/baseof.html`: shell with skip link, header, main, footer, and search modal.
- `layouts/_partials/site_head.html`: replaces Blox's head. It adds a custom favicon, self-hosted fonts, OG tags, `scholarly_meta.html`, and `mysite_meta.html`, and drops Blox's generator tag and icon.
- `components/headers/navbar.html`: brand = owner's name linking home; links on the right; search is an icon only (labelled in the mobile menu); hamburger on the right on mobile.
- `landing/list.html`: hero with photo left, text right (stacked ≤ 640px). The homepage IS the bio page (owner request, Sept 2026): bio paragraphs from `content/_index.md`, plus buttons for Writings & Research Areas, Contact, C.V., Google Scholar, Dataverse, and LinkedIn. There is no separate Bio page (`/bio/` redirects home via `data/redirects.yaml`) and no research-area list on the homepage; research areas are a filter on Writings.
- `publication/list.html`: Writings page. All items are server-rendered with `data-tab`/`data-year`/`data-types`/`data-areas`; one vanilla-JS IIFE handles tabs, search, area, type, year, sort, BibTeX export, and hash state (`#tab&area=…&q=…&years=…&types=…&sort=…`). Conference presentations are intentionally NOT on the site (owner request, Sept 2026); talk layouts remain unused for future use.
- `_partials/work_single.html`: detail page for publications, talks, and software. It never shows the publication_type label or repeats the title in the citation line.
- `_partials/related_finder.html`: See Also, computed at build time (explicit related_* → reverse `related_paper` → response-paper pinning → book editions → see_also → dataverse_url/inline Dataverse URL → area siblings (+3) → title tokens (+2), shared co-authors excluding owner (+1, with last-name fuzzy match), shared tags (+2); threshold 4, or 2 when fewer than 3 explicit picks; cap 8; title dedupe).
- `authors/list.html`: People page (names only), split into Faculty Co-authors and Student Co-authors (from `student_authors`). The student section has an intro from `student_intro` in `content/authors/_index.md`, and names in `exclude:` are hidden. Links come from `data/coauthors.json`. The `author` taxonomy is disabled (`disableKinds: [taxonomy, term]`).
- `content/_content.gotmpl` generates short-URL redirect pages from `data/redirects.yaml`.
- Tab assignment: `data/writings_legacy_map.json` first, then `publication_types`.

## Content decisions
- Legacy site (Google Sites) had no per-paper URLs, so slugs are new, short, and kebab-case. Never rename them.
- Metadata comes from the legacy Research & CV page and the Dec 2025 C.V., cross-checked with Crossref. Abstracts come from Crossref/OpenAlex (EIS from the PDF). Datasets come from Harvard Dataverse.
- Discrepancies are flagged with `<!-- FLAG FOR OWNER -->` comments, never silently fixed in visible text.
- The legacy site's "* = undergraduate student" convention is preserved via `student_authors`.
- PDFs formerly on Dropbox are now in `static/files/`.
- No publisher cover images were added (optional `featured.jpg` per paper).

## Rules
- Subpath safety: templates use `relURL` without a leading slash; content uses `{{< staticrel "files/x.pdf" >}}`; `links:` urls have no leading slash. Menu `.URL` already includes the base path, so do not pass it through relURL again.
- One source of truth per fact; homepage intro ≠ bio; no process commentary on visible pages; labels in `i18n/en.yaml`.
- Always light mode. Colours are tokens in `assets/css/custom.css`.
- `params.mysite.credit` (false per form) and `params.mysite.discovery` (true per form).
- `params.google_site_verification` emits the Google Search Console `<meta name="google-site-verification">` tag (in `site_head.html`); keep it, or Search Console loses verification.
