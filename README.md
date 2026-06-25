# 3P — Ailya Shah

A personal publication built with Jekyll: **Physics · Psyche · Python**. A
post-first homepage, an editorial reading layout, and a three-discipline colour
system used as the site's signature throughout.

Live (project site): `https://<your-username>.github.io/3P/`

---

## What's inside

```
.
├── _config.yml            # site settings, nav, social links
├── index.md               # homepage (hero + featured writing + intro grid)
├── blog.md                # /blog/ — full writing archive, grouped by year
├── about.md               # /about/
├── projects.md            # /projects/
├── contact.md             # /contact/  (links pull from _config.yml)
├── 404.html
├── _layouts/              # default, page, post
├── _includes/             # head, header, footer, figure
├── _posts/                # three starter posts
├── assets/
│   ├── css/main.css       # the full design system
│   ├── js/main.js         # scroll reveal + footer year (optional, no-JS safe)
│   └── images/            # put screenshots here (see images/README.md)
└── .github/workflows/jekyll.yml   # auto build + deploy to GitHub Pages
```

## Run locally

You need Ruby (3.x) and Bundler.

```bash
bundle install
bundle exec jekyll serve --livereload
```

Open <http://localhost:4000/3P/>. The `/3P/` path comes from `baseurl` in
`_config.yml`.

> If you renamed the repo, update **both** `url` and `baseurl` in `_config.yml`.

## Write a new post

Create a file in `_posts/` named `YYYY-MM-DD-title.md`:

```markdown
---
layout: post
title: "Your title here"
date: 2026-07-01
disc: python        # physics | psyche | python | data  → colours the tag
excerpt: "One or two sentences that show up on the homepage and archive."
---

Your content in Markdown.
```

The `disc` field is optional and just picks the discipline colour for the tag.

### Add an image to a post

Drop the file in `assets/images/`, then:

```liquid
{% include figure.html
   src="/assets/images/my-screenshot.png"
   alt="What it shows"
   caption="Fig 1 — caption" %}
```

Leaving out `src` renders a styled **placeholder** (that's what the starter
posts use). Add the `src` when your screenshot is ready.

## Deploy to GitHub Pages (automated)

1. Create a repo named **`3P`** and push this code to the `main` branch.
2. In the repo: **Settings → Pages → Build and deployment → Source → GitHub Actions**.
3. Push. The workflow in `.github/workflows/jekyll.yml` builds and deploys
   automatically; the site appears at `https://<username>.github.io/3P/`.

The workflow sets `baseurl` from GitHub Pages automatically, so it works whether
you keep the name `3P` or change it.

> Using a different default branch? Edit the `branches:` line in the workflow.

## Customise quickly

- **Name / tagline / description** → `_config.yml`
- **Navigation** → the `nav:` list in `_config.yml`
- **Social links** (footer + contact page) → the `social:` list in `_config.yml`
- **Colours / fonts / spacing** → the tokens at the top of `assets/css/main.css`
- **Email** → `author.email` in `_config.yml`

## Notes

- `Gemfile.lock` is gitignored on purpose — the originally uploaded one was built
  on Windows and won't install elsewhere. Bundler regenerates it on first
  `bundle install`.
- The site works with JavaScript disabled; JS only adds the gentle fade-in.
