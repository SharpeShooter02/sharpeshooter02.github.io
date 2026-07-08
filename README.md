# Personal site (Jekyll · Minimal Mistakes · GitHub Pages)

This folder is a **self-contained Jekyll site**. It is intentionally separate
from the research code in the parent project so that none of that code, logs, or
the `.env` secrets get published.

## What's here
- `_config.yml` — site + author-profile settings (edit the `TODO` lines)
- `_pages/about.md` — the About page
- `index.html` — home page (lists posts, shows the profile sidebar)
- `_posts/` — articles (the gap-ORB deep dive is already here)
- `assets/images/` — figures and your headshot (`bio-photo.jpg`)
- `_data/navigation.yml` — top navigation bar

## Before you deploy — fill in the TODOs
1. In `_config.yml`: set `title`, `url`, `author.name`, `author.bio`, and the
   GitHub/LinkedIn URLs. For a user site, leave `baseurl: ""`.
2. In `_pages/about.md`: replace the placeholder copy.
3. Add a square headshot at `assets/images/bio-photo.jpg` (≈400×400px).

## Deploy to GitHub Pages (user site)
1. Create a new **public** repo named exactly `YOUR-USERNAME.github.io`.
2. From this `website/` folder:
   ```
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-USERNAME.github.io.git
   git push -u origin main
   ```
3. In the repo on GitHub: **Settings → Pages → Build and deployment →
   Source: Deploy from a branch → Branch: `main` / root → Save.**
4. Wait ~1–2 minutes. Your site is live at `https://YOUR-USERNAME.github.io`.

## Local preview (optional)
```
cd website
bundle install
bundle exec jekyll serve
```
Then open http://localhost:4000. (Requires Ruby + Bundler.)

## Adding a new post
Drop a file in `_posts/` named `YYYY-MM-DD-title.md` with front matter:
```
---
layout: single
title: "Your title"
date: 2026-07-01
categories: [trading, research]
tags: [tag1, tag2]
excerpt: "One-line summary shown on the home feed."
toc: true
---
```
Put any figures under `assets/images/<post-slug>/` and reference them with
absolute paths, e.g. `/assets/images/<post-slug>/fig1.png`.
