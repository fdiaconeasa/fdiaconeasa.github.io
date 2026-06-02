# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jekyll-based personal blog/portfolio site deployed to GitHub Pages. The site is built and deployed automatically on every push to `main` via the GitHub Actions workflow in `.github/workflows/jekyll-gh-pages.yml`.

## Local development

There is no Gemfile — the site relies on GitHub Pages' built-in Jekyll. To run locally, Jekyll must be installed separately:

```bash
gem install jekyll bundler
jekyll serve
```

The site is then available at `http://localhost:4000`.

## Adding content

Posts go in `_posts/` with the filename format `YYYY-MM-DD-title.md` (or `.markdown`). Each post requires front matter:

```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD
categories: tag1 tag2
---
```

The home page (`index.markdown`) uses `layout: home` and contains the bio/intro text directly in the file body.

## Configuration

`_config.yml` controls the site title, theme (`minima`), and GitHub username. Changes to `_config.yml` require a server restart when developing locally.

## Deployment

Pushing to `main` triggers the GitHub Actions workflow, which builds the Jekyll site and deploys it to GitHub Pages. There is no staging environment.
