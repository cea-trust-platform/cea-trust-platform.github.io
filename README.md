# TRUST Platform Website

Source for the public website of the [TRUST Platform](https://cea-trust-platform.github.io), an HPC CFD software developed at CEA.

Built with **Jekyll** on top of the [Jekyll Serif Theme](https://github.com/zerostaticthemes/jekyll-serif-theme). Hosted on **GitHub Pages** (primary) with a Netlify fallback config.

---

## Project layout

```
.
├── _config.yml          # Jekyll configuration (site title, collections, plugins)
├── Gemfile              # Ruby dependencies (Jekyll ~4.2)
├── netlify.toml         # Netlify build config (mirrors GitHub Pages deployment)
│
├── index.md             # Home page  (layout: home)
├── about.md             # About page (layout: page)
├── news.md              # News page  (layout: page) — releases, events, publications
├── gallery.md           # Gallery    (layout: page) — embedded YouTube videos
├── cite_us.md           # Cite us    (layout: page) — BibTeX / Markdown / RST citation
├── team.md              # Team page  (layout: team)
├── contact.md           # Contact    (layout: page)
│
├── RN/                  # Release notes (one Markdown file per version, e.g. v1_9_7.md)
│
├── _data/
│   ├── menus.yml        # Navigation links (main navbar + footer)
│   ├── features.json    # Feature cards shown on the home page
│   ├── seo.yml          # SEO metadata (title, description, OG tags)
│   └── social.json      # Social/external links used in footer
│
├── _layouts/            # HTML page templates
│   ├── default.html     # Base layout (header + footer shell)
│   ├── home.html        # Home page (hero + feature cards)
│   ├── page.html        # Generic content page
│   ├── team.html        # Team listing page
│   ├── classe.html      # Single "class" (collection item) page
│   └── classes.html     # List of all classes
│
├── _includes/           # Reusable HTML snippets (header, footer, menus, YouTube embed…)
├── _sass/               # SCSS source (Bootstrap + custom component styles)
│   ├── bootstrap/       # Bootstrap 4 source
│   └── components/      # Site-specific overrides
├── assets/
│   ├── css/style.scss   # Main stylesheet entry point (compiled by Jekyll)
│   └── js/scripts.js    # Minimal vanilla JS (mobile menu toggle)
└── images/              # Static images (logos, team photos, simulation screenshots)
```

---

## Running locally

**Requirements:** Ruby ≥ 2.7, Bundler.

```bash
# Install dependencies (first time only)
bundle install

# Serve with live reload
bundle exec jekyll serve --livereload

# Open http://localhost:4000
```

The built site lands in `_site/` (git-ignored).

---

## How to update content

### Add a news item

Edit `news.md`. News is plain Markdown organized under `##` headings (releases, events, publications). No special front-matter needed.

### Add a release note

1. Create `RN/v<major>_<minor>_<patch>.md` (copy an existing one as a template).
2. Add a bullet to the **Version Release** section in `news.md` linking to it.

### Update the team

Edit `team.md`. Photos are fetched directly from the GitHub repo raw URL — add the image to `images/social/` and push it first.

### Change navigation links

Edit `_data/menus.yml`. Both `main` (top navbar) and `footer` menus are defined there; `weight` controls the order.

### Change homepage feature cards

Edit `_data/features.json`. Each object has a `title` and a `description`.

---

## Deployment

Pushes to the `master` branch are deployed automatically by **GitHub Actions** to GitHub Pages at `https://cea-trust-platform.github.io`.

`netlify.toml` contains an equivalent Netlify build config (`jekyll build` → `_site/`) in case the hosting ever needs to be switched.
