# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Tech Stack

- **Static site generator:** Jekyll with the [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll) remote theme (v6.0.1)
- **Hosting:** GitHub Pages — pushes to `main` trigger automatic builds and deployment at https://ntinasf.github.io
- **No build tooling locally** — no package.json, Gemfile, or npm dependencies are checked in

## Local Development

To serve locally, you need Ruby + Bundler + Jekyll installed. Since there is no Gemfile, you would need to add one or use:

```bash
gem install jekyll bundler
jekyll serve
```

In practice, GitHub Pages handles all builds automatically on push — local preview is optional.

## Architecture

This is a content-driven static site with no client-side state or JavaScript framework.

### Content Structure
- `_config.yml` — site metadata, theme, plugins, navbar links, and social links (source of truth for global config)
- `index.md` — homepage (two-column layout: profile photo sidebar + bio)
- `resume.md` — skills, certifications, education
- `projects/index.md` — projects gallery
- `projects/<project-name>/index.md` — individual project case studies

### Routing
Jekyll maps directory paths directly to URLs. Adding a new project means creating `projects/<slug>/index.md` with the appropriate YAML frontmatter.

### Styling
Custom styles live in [assets/css/custom-styles.css](assets/css/custom-styles.css). The color palette is cyan/teal-based (#0097a7 accent, #2c3e50 dark base). The Beautiful Jekyll theme provides Bootstrap and base layout — avoid duplicating styles already provided by the theme.

### External Embeds
Project pages embed live external apps via iframes:
- Power BI dashboard (Thessaloniki Airbnb project)
- Streamlit app (Credit Risk project)

These are hosted externally; the iframes just reference their URLs.

### Frontmatter Conventions
Pages use YAML frontmatter for layout selection and metadata. Common fields:

```yaml
---
layout: page          # or 'home', 'post'
title: Page Title
subtitle: Optional subtitle
---
```
