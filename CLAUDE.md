# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal Jekyll blog (svs.io) using the "no-style-please" theme. It's a minimalist, fast-loading blog focused on content about technology, engineering management, and personal writing. The site is deployed as a static site and contains blog posts, talks, and personal information.

## Development Commands

### Local Development
```bash
bundle install          # Install dependencies
bundle exec jekyll serve # Start local development server (http://localhost:4000)
```

### Building
```bash
bundle exec jekyll build # Build the static site to _site/ directory
```

### Content Management
```bash
bundle exec jekyll post "Post Title"  # Create a new post (via jekyll-compose)
bundle exec jekyll draft "Draft Title"  # Create a new draft
```

## Architecture & Structure

### Core Files
- `_config.yml` - Main Jekyll configuration, theme settings, site metadata
- `Gemfile` - Ruby dependencies including Jekyll and theme
- `index.md` - Homepage content using the `home` layout
- `_data/menu.yml` - Site navigation structure and menu configuration

### Content Structure
- `_posts/` - Published blog posts (dating back to 2005)
- `_drafts/` - Draft posts
- `_layouts/` - Theme templates (default, home, post, page, archive)
- `_includes/` - Reusable template components
- `_sass/` - Stylesheet source
- `_site/` - Generated static site (ignored in git)

### Theme Customization
The site uses the "no-style-please" theme which provides:
- Minimal CSS (1kb)
- Dark/light/auto mode support
- Content-first typography
- Fast loading performance

### Menu Configuration
The main navigation is configured in `_data/menu.yml` and supports:
- Nested menu items
- Post lists with category filtering
- Custom URLs and HTML content
- Pagination with "show more" links

### Site Configuration
Key settings in `_config.yml`:
- Theme appearance: "auto" (follows system preference)
- Permalink structure: `/:slug.html`
- Date format: `%Y-%m-%d`
- SEO and RSS feed plugins enabled

## Content Creation

### Blog Posts
- Posts are stored in `_posts/` with filename format `YYYY-MM-DD-title.md`
- Use standard Jekyll front matter for metadata
- Supports categories for organization

### Pages
- Static pages like `about.md`, `talks.md` use the `page` layout
- Archive pages use the `archive` layout for post listings

## Theme Notes

This is a gem-based theme, so core theme files are in the gem. Local customizations should be placed in:
- `_layouts/` to override theme layouts
- `_includes/` to override theme includes
- `_sass/` to override theme styles
- `assets/` for custom assets

The theme is optimized for minimal resource usage and fast loading, making it ideal for content-focused sites.