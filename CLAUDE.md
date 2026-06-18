# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a GitHub Pages site hosted at [github.com/sampwhite/Gyassa](https://github.com/sampwhite/Gyassa), using the `jekyll-theme-midnight` Jekyll theme. Content is served directly by GitHub Pages — there is no local build step required.

## Structure

- `index.md` — the single page of site content (Jekyll front matter + Markdown)
- `_config.yml` — Jekyll configuration (currently just sets the theme)
- `_includes/head-custom.html` — injects favicons and the web manifest link into `<head>`
- `site.webmanifest` — PWA manifest (currently with blank name fields)
- Favicon images at various sizes for browser, Apple, and Android

## Deploying

Push to the `main` branch on `origin` — GitHub Pages picks it up automatically. There is no CI or build pipeline.
