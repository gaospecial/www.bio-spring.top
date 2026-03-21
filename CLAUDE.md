# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal website built with [Hugo](https://gohugo.io/) and [blogdown](https://bookdown.org/yihui/blogdown/), using a custom fork of the [hugo-prose theme](https://github.com/yihui/hugo-prose). The site is primarily in Chinese and focuses on bioinformatics, R programming, and personal research.

## Build Commands

### Local Development

```bash
# Build the site (knits Rmd to md, then runs Hugo)
make site
# or
Rscript -e "blogdown::build_site()"

# Clean the build output
make clean

# Serve the site locally (for development in RStudio)
# Set blogdown.serve_site.startup = TRUE in .Rprofile
```

### Deployment

The site deploys automatically via GitHub Actions ([`.github/workflows/blogdown.yml`](.github/workflows/blogdown.yml)) on push to `master` branch. It builds to `gh-pages` branch and can also deploy to Netlify using [`netlify.toml`](netlify.toml).

## Content Structure

- `content/post/` - Blog posts. Each post is in a directory named `YYYY-MM-DD-slug/` containing `index.md` or `index.zh.md`
- `content/card/` - Home page cards (publications, projects, interests)
- `content/publication/` - Publication lists (selected and full publications)
- `content/about/` - About page content
- `content/work/` - Work-related content

## Key Configuration

### Hugo Config ([`config.yaml`](config.yaml))

- Default language: `zh` (Chinese)
- Theme: `hugo-prose` (custom fork in `themes/hugo-prose/`)
- Permalinks: `/post/:year/:month/:day/:slug/`
- Hugo version check disabled: `hugo_version: ""`

### Blogdown Config ([`.Rprofile`](.Rprofile))

- `blogdown.knit.on_save = TRUE` - Auto-knit Rmd files on save
- `blogdown.method = 'markdown'` - Build Rmd to markdown (not HTML)
- Hugo version is not pinned: `blogdown.hugo.version = NULL`

### Theme Customization

The site uses a custom theme fork: `themes/hugo-prose/` (from `github.com/gaospecial/hugo-prose.git` branch `bio-spring`). Custom layouts and shortcodes are in:
- `layouts/` - Custom template overrides
- `layouts/shortcodes/blogdown/` - Custom shortcodes

## Creating New Content

### New Blog Post

Create a directory in `content/post/` with format `YYYY-MM-DD-slug/` and add `index.md` or `index.zh.md`. Example frontmatter:

```yaml
---
title: Post Title
author: Chun-Hui Gao
date: 'YYYY-MM-DD'
slug: post-slug
categories:
  - Category Name
tags:
  - tag1
  - tag2
---
```

### Publication Content

- Selected publications: `content/publication/selected-publication.Rmd` (knitted to `.md`)
- Full publications: `content/publication/full-publication.Rmd` (knitted to `.md`)

## Author Information

Author bios are defined in [`data/authors.yaml`](data/authors.yaml). The site uses "gaoch" or "Chun-Hui Gao" as the author identifier.

## Language Notes

- The site is primarily in Chinese (`defaultContentLanguage: "zh"`)
- English content is supported (e.g., `about.en.md`)
- Blog posts are mostly in Chinese (files ending in `.zh.md`)

## GitHub Actions CI/CD

The workflow [`.github/workflows/blogdown.yml`](.github/workflows/blogdown.yml):
- Runs on push to `master` branch
- Uses Ubuntu with R, Hugo latest, and blogdown
- Caches R packages and Hugo modules
- Builds with `blogdown::build_site(baseURL = "/bio-spring/")`
- Deploys to `gh-pages` branch using `peaceiris/actions-gh-pages@v3`
