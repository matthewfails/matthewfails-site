# Updating Your Website

Everything on the site is plain text files you can edit right on GitHub.
Open a file on github.com, click the pencil icon, edit, and click **Commit changes**.
The site rebuilds and goes live by itself about two minutes later.
You can watch progress under the repository's **Actions** tab.

All site files live in the `hugo-site/` folder.

## Before you go further: please review these

- **People page is auto-generated and its links were guessed by web search.**
  Every co-author on any paper or talk is listed automatically. Web links for co-authors
  come from `hugo-site/data/coauthors.json`. They were found by searching the web, so
  **check every entry** and fix or remove any wrong person or URL. Names with no link
  had no page that could be verified. Add one in that file if you know it.
- **Flags.** Search the repository for `FLAG FOR OWNER` to see every spot where sources
  disagreed, such as a volume number, author order, or address. Flags are hidden
  comments and never show on the site.

## Add a new paper

1. Go to `hugo-site/content/publication/` and click **Add file → Create new file**.
2. Name it `your-short-slug/index.md`, e.g. `oil-and-coups/index.md`.
   The folder name becomes the web address (`…/publication/oil-and-coups/`). **Never rename it later.**
3. Paste and fill in:

```yaml
---
title: "Paper Title"
date: 2026-05-01
authors: ["Matthew D. Fails", "Co Author"]
student_authors: ["Co Author"]          # marks the name with * (undergraduate); delete if none
publication_types: ["journal_article"]  # or book_review, essay, data
publication: "Journal Name 12 (3): 45–67"
doi: "10.xxxx/xxxxx"
abstract: "Paste the abstract here."
links:
  - name: "Publisher's Version"
    url: "https://doi.org/10.xxxx/xxxxx"
  - name: "Article (PDF)"
    url: "files/my-paper.pdf"            # no leading slash
research_areas: ["resource-curse"]       # see list below
tags: ["oil", "autocracy"]
dataverse_url: "https://doi.org/10.7910/DVN/XXXXX"   # optional
---
```

4. Upload the PDF to `hugo-site/static/files/` with the same filename.
5. Optional: drop a journal cover image named `featured.jpg` into the paper's folder.

The "See Also" links, the Writings page, the homepage research areas, search, and the
People page all update automatically. A co-author listed in `student_authors` appears under
"Student Co-authors" on the People page; everyone else appears under "Faculty Co-authors".

**Research area names:** `democratic-backsliding`, `resource-curse`, `authoritarian-politics`,
`political-risk`, `colonialism`, `mass-attitudes`. To rename an area or add a new one, edit
`hugo-site/data/research_areas.json`.
Research areas appear as a filter on the Writings page.

## Add a replication dataset

Create `hugo-site/content/publication/<paper-slug>-data/index.md` with
`publication_types: ["data"]`, a `dataverse_url:`, and `related_paper: "<paper-slug>"`.
Copy any existing `*-data` folder as a template.

## Update your bio, teaching, or contact page

Your bio paragraphs are on the homepage: edit `content/_index.md`.
For the other pages, edit `content/teaching/index.md` or `content/contact/index.md`.
The homepage buttons (Writings & Research Areas, Contact, C.V., Google Scholar, Dataverse, LinkedIn) come from `hugo.yaml` under `params.owner`.
To replace the C.V., upload the new PDF to `static/files/` as `fails-cv.pdf`, overwriting the old file.
The short link `…/cv/` always points there.

## Replace your photo

Your headshot is `hugo-site/assets/media/bio-photo.jpg`. It is used on the homepage.
To swap it, upload a new photo with **exactly that name**, overwriting the old file.
A square image works best, about 720×720 pixels. If you delete the file, an "MF" circle appears instead.

## People page

- **Add or fix a co-author link:** edit `hugo-site/data/coauthors.json`. Use the name exactly as it appears in the papers' `authors:` lists.
- **Hide someone:** add their name to `exclude:` in `hugo-site/content/authors/_index.md`.
- **Edit the paragraph about student co-authors:** it is `student_intro:` in the same file.

## Change a button label

All labels (e.g. "Download C.V. (PDF)") live in `hugo-site/i18n/en.yaml`.

## Footer credit / hidden marker

In `hugo-site/hugo.yaml`, under `params.mysite`:
`credit: true` shows a small "Created using GaryKing.org/mysite" line in the footer (currently off).
`discovery: false` removes the invisible marker.

## Coming back after a while

- **Small text edits:** edit the file on github.com and commit. The site updates about two minutes later.
- **Bigger changes:** open VS Code in WSL, start Claude, and say what you want changed on "my academic website at ~/matthewfails-site".
  Claude reads `CLAUDE.md` in this repository, updates its copy from GitHub, previews the change for you, and publishes when you approve.
- **New computer:** `gh repo clone matthewfails/matthewfails-site`, then ask Claude to set up the local preview. `CLAUDE.md` has the steps.

## Preview locally (optional)

With Hugo extended ≥ 0.148.2 and Node installed, run from `hugo-site/`:
`npm install`, then `hugo server`. Open http://localhost:1313/matthewfails-site/
