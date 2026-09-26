# CLAUDE.md: matthewfails-site

Academic website for Matthew D. Fails (Professor of Political Science, Oakland University).
Live at https://matthewfails.github.io/matthewfails-site/. Every push to `main` deploys via `.github/workflows/deploy.yml`.

Read `WEBSITE_PRINCIPLES.md` (architecture, template ownership, rules) and `UPDATING.md` (owner-facing how-tos) before changing anything.

## Workflow
1. `git pull` first; the owner sometimes edits on github.com.
2. Make the change in `hugo-site/`.
3. Build and preview locally (below), check the affected pages, and show the owner.
4. Commit and push only after the owner approves. Then confirm the Actions run succeeded (`gh run list`) and spot-check the live page.

## Local build and preview
Hugo 0.148.2 extended (Blox v0.10.0 requires ≥ 0.148.2), Go, and Node 20 are in `~/.local/bin` on Matthew's WSL machine.
If they are missing (e.g. a new computer), install them without sudo: Hugo and Node tarballs into `~/.local/bin`, Go into `~/.local/opt`.

```bash
cd ~/matthewfails-site/hugo-site
export PATH=$PWD/node_modules/.bin:~/.local/bin:$PATH
npm install                     # Tailwind CLI + typography plugin + Pagefind
hugo server                     # quick preview at http://localhost:1313/matthewfails-site/ (no search)
# Full preview with search, served under the real subpath:
hugo --gc --minify -d /tmp/mfsite/matthewfails-site && npx pagefind --site /tmp/mfsite/matthewfails-site
cd /tmp/mfsite && python3 -m http.server 8765   # http://localhost:8765/matthewfails-site/
```

## Owner decisions (don't undo without asking)
- The homepage IS the bio: photo, two first-person bio paragraphs (`content/_index.md`), and buttons for Contact, Writings & Research Areas, C.V. (PDF), Google Scholar, and Harvard Dataverse. There is no separate Bio page; `/bio/` redirects home. There is no research-area list on the homepage.
- No conference presentations anywhere on the site. Writings tabs: All, Articles, Reviews & Essays, Data.
- People page: Faculty Co-authors and Student Co-authors (driven by `student_authors`), with a student-mentoring intro in `content/authors/_index.md`. Donna Folland and Kalsoom Hussain are excluded.
- LinkedIn appears on the Contact page and in the footer, not as a homepage button.
- Footer credit `params.mysite.credit: false`; invisible marker `discovery: true`. The mysite team form was submitted on 2026-09-24; never resend it.
- For published articles, use the published title and author names over the C.V. where they differ (e.g. "Resources, Rent Diversification…", "Marc C. DuBuis").

## Status and records (as of 2026-09-24)
- **No custom domain** (owner's choice): the github.io address above is the permanent URL. Don't propose moving it unless asked.
- **Google Search Console**: property verified via `params.google_site_verification` in `hugo.yaml`; sitemap submitted. Don't remove that tag, or the verification lapses.
- **Old Google Site** (sites.google.com/oakland.edu/mfails) now points visitors to this site. The owner plans to unpublish it once this site outranks it in search.
- **C.V.**: `hugo-site/static/files/fails-cv.pdf` (the `/cv/` short link points here). Current file = `MDF_CV_Sept26_update.pdf`; its "personal website" link points to this site. To update, overwrite that one file, keeping the same name.
- **Teaching page** opening describes courses by topic, not course title (owner preference).
- **Headshot** (new photo, 2026-09-25): shown as a 4:5 PORTRAIT rectangle with rounded corners, not a circle. The owner rejected circle crops: too close up, and padding the sides made the shoulders look too broad. `bio-photo.jpg` is the owner's own high-res 4:5 crop (2887×3609, uploaded via GitHub). The homepage generates 200/400/600/800-px-wide WebP versions with `srcset` so it stays sharp on high-DPI screens. Upload future photos at ≥1200×1500, 4:5.
- **Name display** (2026-09-26): site-level display uses "Matthew Fails" (`params.owner.short_name`: navbar brand, homepage heading, footer name, copyright). Citations, author lists, page/tab titles, and scholarly metadata keep "Matthew D. Fails" (`params.owner.name`). Don't change `owner.name`; it's how templates recognize the owner in `authors:` lists.
- **"Writings" was renamed "Research"** (menu, page title, breadcrumbs, homepage button). The URL stays `/publication/` for link stability; i18n keys are still named `writings*`.

## Pending
- Owner will update the OU Political Science directory link after the new headshot arrives.

## Gotchas
- Subpath site: use `relURL` without a leading slash in templates and `{{< staticrel "files/x.pdf" >}}` in content. Menu `.URL` already includes the base path.
- Blox Tailwind needs `@tailwindcss/cli` and `@tailwindcss/typography` from npm, or the build fails.
- Minified HTML is one line, so count matches with `grep -o … | wc -l`, not `grep -c`.
