# Copilot Instructions — Academic Pages (concise)

This repository is a Jekyll-based academic portfolio (Academic Pages theme + Minimal Mistakes customizations) that mixes hand-written Markdown with generated collection items (publications, talks). The site is built locally with Ruby/Jekyll or inside the provided Docker environment; content can be generated from TSV files in `markdown_generator/`.

Key points for contributors and AI agents:

- **Big Picture**: source content = Markdown in `_posts/`, `_publications/`, `_talks/`, `_pages/`, plus small data files under `_data/`. Layouts live in `_layouts/` and reusable parts in `_includes/`. Site output is `_site/` after a Jekyll build.
- **Generators**: `markdown_generator/publications.py` and `markdown_generator/talks.py` turn TSV rows into `YYYY-MM-DD-slug.md` files under `_publications/` and `_talks/`. Edit TSVs (not the generated markdown) and re-run the script.
- **Why TSV**: TSV is used instead of CSV to avoid comma-escaping problems (see `markdown_generator/publications.tsv` for exact column order).

Essential files & folders to reference when making changes:

- `markdown_generator/`: `publications.py`, `talks.py`, `publications.tsv`, `talks.tsv`, `readme.md` — generators and source TSVs.
- `_layouts/`, `_includes/`, `_sass/`: theme and layout customizations.
- `_data/`: `cv.json`, `navigation.yml`, UI text and comment data.
- `_publications/`, `_talks/`: generated content — treat as generated artifacts unless editing manually on purpose.
- `Dockerfile`, `docker-compose.yaml`: containerized dev environment.

Developer workflows (concrete commands):

- Native Ruby/Jekyll (local dev):
  - `bundle install`
  - `bundle exec jekyll serve -l -H 0.0.0.0`  # serves with auto-reload; use 0.0.0.0 in containers
- Docker (recommended for reproducible dev):
  - `docker compose up --build`
- JS assets: `npm run build:js` (bundles/minifies JS under `assets/js/`)
- Regenerate collections (after editing TSVs):
  - `python3 markdown_generator/publications.py`
  - `python3 markdown_generator/talks.py`
  - Commit generated `_publications/*.md` and `_talks/*.md` as appropriate

Patterns & conventions (repo-specific):

- Date-slug filenames: use `YYYY-MM-DD-descriptive-slug.md` for collection items. The generator scripts and Jekyll expect this format.
- Front matter: collection files require fields like `collection: publications`, `date`, `permalink`, `venue`, and `citation` — see `_publications/2025-06-08-paper-title-number-5.md` for an example.
- Navigation: update `_data/navigation.yml` to change header/footer links.
- `_config.yml` changes require a Jekyll restart to take effect.

Integration notes & external dependencies:

- Geocoding: `talks.py` and `talkmap.ipynb` may call external geocoders (Nominatim). Network access and API rate limits may apply.
- GitHub Pages: repository is set to publish from `master` (auto-deploy on push). Do not rely on local `_site/` being authoritative until you test builds.

Practical examples (copyable):

```
# Install Ruby gems and serve locally
bundle install
bundle exec jekyll serve -l -H 0.0.0.0

# Build JS assets
npm run build:js

# Regenerate publications after editing TSV
python3 markdown_generator/publications.py

# Start dev containers
docker compose up --build
```

Notes for AI agents working in this repo:

- Prefer editing TSV source files and updating generator scripts rather than hand-editing many generated markdown files.
- When making layout changes, run Jekyll locally and inspect `_site/` output; check `/_pages/publications.html` and sample pages under `_publications/`.
- If you add or rename collection fields, update the generator scripts and `_config.yml` collection defaults together.
- Avoid altering build/deploy defaults unless explicitly requested; these live in `_config.yml`, `Dockerfile`, and `docker-compose.yaml`.

If you'd like, I can:

- add a short example that shows one TSV row and the resulting generated front matter,
- or run the generators and show a diff for a sample TSV change.
# Copilot Instructions for Academic Pages

This is a Jekyll-based academic portfolio site using the Academic Pages theme. The site combines dynamic content generation from data files with hand-crafted markdown files.

## Architecture Overview

**Core Stack**: Jekyll (static site generator), Ruby, Node.js for asset bundling
- **Collections**: Defined in `_config.yml` with outputs to `/publications/`, `/talks/`, `/portfolio/`, `/teaching/`
- **Content Organization**: Markdown files in prefixed directories (`_publications/`, `_talks/`, etc.) with YAML front matter
- **Layouts**: HTML/Liquid templates in `_layouts/` - key ones are `single.html` (default), `talk.html` (talks), `archive.html` (collection pages)
- **Theming**: Minimal Mistakes fork with customizations in `_sass/` (syntax highlighting, responsive design)

## Critical Workflows

### Local Development Setup
- **Ruby + Jekyll**: Run `bundle install`, then `jekyll serve -l -H localhost` (auto-rebuilds on Markdown/HTML changes)
- **Docker**: `docker compose up` for isolated environment with auto-reload
- **Node assets**: Run `npm run build:js` to minify JavaScript (bundles jQuery plugins from `assets/js/`)

### Content Generation Pipeline
Three Python/Jupyter tools in `markdown_generator/` auto-generate collection files from TSV data:
- **Publications**: `publications.py` reads `publications.tsv` (cols: pub_date, title, venue, excerpt, citation, site_url, paper_url) → creates `_publications/YYYY-MM-DD-[url_slug].md`
- **Talks**: `talks.py` reads `talks.tsv` → creates `_talks/YYYY-MM-DD-[url_slug].md` with geolocation support via talkmap
- **Talkmap**: `talkmap.py` uses `getorg` + Nominatim geocoder to create interactive map from talk locations
- Critical: Always update `*.tsv` files before regenerating markdown; scripts use HTML escaping for YAML special chars

### Site Build & Deployment
- Configured for GitHub Pages (auto-deploys from `master` branch on push)
- Gems: `jekyll-feed`, `jekyll-sitemap`, `jekyll-redirect-from`, `jemoji` for feed/SEO/redirects
- Exclude list in `_config.yml` prevents non-content files from being published

## Key Conventions & Patterns

### Front Matter Structure
All content files require YAML front matter with collection-specific fields:
- **Publications**: `collection: publications`, `category: [books|manuscripts|conferences]`, `permalink`, `date`, `venue`, `citation`
- **Talks**: `collection: talks`, `type: "Talk"|"Tutorial"|...`, `permalink`, `date`, `venue`, `location`
- **Blog posts**: `date`, `permalink`, `tags: [list]` (no collection field)
- **Pages**: `permalink` only; uses `layout: archive` for collection aggregation pages
- **Author profile**: Controlled via `_data/authors.yml` (currently empty) and author info in `_config.yml` sidebar section

### URL Slug Convention
`YYYY-MM-DD-[descriptive-slug]` pattern used in all collection files (publications, talks, teaching, portfolio).
The date + slug combination must be globally unique to avoid file collisions.

### Navigation & Site Structure
- Main nav defined in `_data/navigation.yml` (controls header links)
- Archive pages are hand-written in `_pages/` (e.g., `publications.html`, `talks.html`) using Liquid loops over `site.[collection]`
- Category/tag archives auto-generated via `liquid` method in `_config.yml`

### Data Files
- `_data/cv.json`: Structured CV data (referenced by cv-json layout)
- `_data/comments/`: Staticman comment data (when comments enabled)
- All user-facing text configurable in `_config.yml` (title, author, social links)

## Project-Specific Quirks

1. **Jekyll restart required** for `_config.yml` changes (YAML front matter reloads automatically)
2. **TSV over CSV** intentionally used in data generation (publications/talks have many commas; TSV avoids escaping issues)
3. **HTML escaping in Python generators**: Quotes and ampersands escaped to `&quot;`, `&apos;`, `&amp;` for YAML safety
4. **Collection defaults** in `_config.yml`: Automatically apply `author_profile: true`, layout, etc. based on path
5. **Sitemap & feeds** auto-generated; exclude/include lists control which content appears where

## Typical Tasks & Patterns

- **Add a publication**: Update `markdown_generator/publications.tsv`, run `publications.py`, commit generated `_publications/*.md`
- **Add a talk**: Update `markdown_generator/talks.tsv`, run `talks.py` (geolocation happens here), commit generated files + regenerated talkmap
- **Customize theme**: Edit `_sass/` for styling, `_includes/` for component reuse, `_config.yml` for global settings
- **Add custom page**: Create `.md` or `.html` in `_pages/`, set `permalink` and `layout` in front matter
- **Edit layouts**: Liquid templates in `_layouts/` use `{{ site.* }}` for global config and `{{ page.* }}` for per-page front matter

## Files to Reference

- `_config.yml` - Global settings, collection definitions, defaults
- `markdown_generator/readme.md` - Explains data generation workflow
- `_layouts/single.html` - Default page template (understand this to customize appearance)
- `_pages/publications.html` - Example of iterating over collection with Liquid
