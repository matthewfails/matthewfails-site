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

## Pending
- New headshot coming (Sept/Oct 2026): replace `hugo-site/assets/media/bio-photo.jpg`, keeping the same filename. It's auto-cropped to a circle.

## Gotchas
- Subpath site: use `relURL` without a leading slash in templates and `{{< staticrel "files/x.pdf" >}}` in content. Menu `.URL` already includes the base path.
- Blox Tailwind needs `@tailwindcss/cli` and `@tailwindcss/typography` from npm, or the build fails.
- Minified HTML is one line, so count matches with `grep -o … | wc -l`, not `grep -c`.
