# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Purpose

This repository is **gyassa.com** — the site of Gyassa Software (Samuel White and Eva Cordes). Its home page
gives the essential facts about the company and links to dynamicruntime.com (the marketing site, kept in
`sampwhite/KdrCommercial`) and dynamicruntime.org (the live KDR reference instance). Beside it is **Sam White's
blog**: essays on software design, rating systems, and the occasional bit of mathematics. The home page is
informational; marketing belongs on dynamicruntime.com. It is a content site, not an application: the "code" is
Markdown, a little HTML, and Jekyll configuration.

## How it is built

It is a [Jekyll](https://jekyllrb.com/) site served by **GitHub Pages**. `_config.yml` still names the
`jekyll-theme-midnight` theme, but the custom layout no longer loads any of its styles: the look is the site's
own stylesheet, `assets/css/gyassa.css`. GitHub Pages runs the Jekyll build server-side on every push — **there is no local
build step, no Gemfile, and no CI pipeline**. You edit Markdown/HTML and push; GitHub renders it.

Jekyll turns each `.md` file into an `.html` page, so **inter-page links use the `.html` extension** even
though the source is `.md` (e.g. a post links another as `personal/other-post.html`). Markdown is rendered
by kramdown, which treats a **single newline inside a paragraph as a space** (no `<br>`), so prose can be
hard-wrapped without changing the output.

### Layout of files

- `index.md` — the home page (`permalink: /`): facts about Gyassa Software, where to find things, contact.
- `blog.md` — **Sam White's blog** (`/blog.html`): a short intro plus the **reverse-chronological index of
  posts**. Each post has an entry here; adding a post means adding an entry (see conventions below). It is
  `/blog.html`, not `/blog/`, so its relative `personal/…` links resolve.
- Post URLs (`/personal/<slug>.html`) are linked from outside (the KDR README, dynamicruntime.com): never
  move or rename a post.
- `personal/` — the post source files, one `.md` per essay, plus `personal/files/` for attachments
  (a PDF, a zip) that some posts link to.
- `_layouts/default.html` — a **custom** layout (overrides the theme's): a header (the Gyassa logo and name,
  then Home · Sam White's blog · Contact), the page in a left-aligned content column (on post pages, under
  `/personal/`, after a link back to the blog), and a footer. All pages use `layout: default`.
- `assets/css/gyassa.css` — the only stylesheet. It shares its palette, type, and light/dark behavior with
  dynamicruntime.com (`sampwhite/KdrCommercial`, `dyncom/site/assets/site.css`) so the two sites read as one
  family; keep them in step. Two classes are set from Markdown with kramdown's `{: .class}` line before a
  block: `.lede` (the home page's opening paragraph) and `.posts` (the post list in `blog.md`, which must
  keep its `{: .posts}` line). The home page's "where to find things" cards are raw HTML (`.cards`, `.card`).
- `_includes/head-custom.html` — injected into `<head>`: favicons, the PWA manifest link, and **MathJax**
  (LaTeX via `$…$` inline and `$$…$$` display — used by the number-theory post).
- `_config.yml` — Jekyll config: the theme, the site `title` and `description` (shown in the header and
  used by the SEO tag).
- `CNAME` — the custom domain, `gyassa.com`.
- `404.md` — custom not-found page.
- `site.webmanifest`, `favicon*.png`, `favicon.ico`, `apple-touch-icon.png`, `android-chrome-*.png` — PWA
  manifest and icon assets.

## Post conventions

A post is a Markdown file in `personal/`. Follow the existing posts:

- **Front matter**: `layout: default` and a `title:`.
- **Body**: an H1 matching the title, then an italic date line, then the content. The date format is
  `*Mon D, YYYY*` — comma after the day, and no zero-padding on the day (`*Jul 5, 2026*`, not
  `*Jul 05 2026*`). The same format is used in `blog.md`. Older posts predating this site carried a
  comma-less form imported from its previous host; those have been normalised, so anything without a
  comma is now a mistake rather than a variant.
- **Line wrapping**: hard-wrap prose at **≤120 characters** (kramdown makes single newlines render as
  spaces, so this is purely for clean diffs and future edits). Keep a whole `[text](url)` link on one line.
- **Math**: use MathJax delimiters (`$…$`, `$$…$$`).
- **Publishing** a post means adding it to the Posts list in `blog.md`, newest first:
  ```
  - **[Title](personal/slug.html)** — *Mon D, YYYY*  ⏎
    One-line blurb.
  ```
  The date is formatted exactly as in-post. The index differs in two ways: the title line ends with **two
  trailing spaces** (a hard break) before the blurb, and each blurb is a **single long line** (the index is
  not wrapped at 120).
- **AI-authored posts**: some posts are written by an AI rather than by Sam. When a post is AI-authored,
  it says so and names the model (e.g. "Claude Opus 4.8, model id `claude-opus-4-8`"), writes in the AI's
  first person, and links the human piece it relates to. See
  `personal/kotlin-dynamic-runtime-project-ai.md`.

## Deploying

Push to `main` on `origin` — GitHub Pages rebuilds and serves it at gyassa.com automatically. No branch,
review gate, or CI stands between a push and the live site, so push deliberately.

## Task tracking

Tasks for this site have been tracked as issues in a **separate** repository, `sampwhite/Actions` (this
repo's own GitHub issues are not the tracker). A typical flow: read the task from an `Actions` issue, do
the work here in Gyassa, reference the issue in the commit (`Addresses sampwhite/Actions#N`), push, then
close it there (`gh issue close N -R sampwhite/Actions`).
