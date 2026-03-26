# Website Redesign Design

**Date:** 2026-03-26
**Status:** Approved

## Goal

Rebuild jonmilley.github.io as a focused personal portfolio. The site should showcase curated projects, list skills, and provide contact links. It should feel developer-native: dark, technical, clean.

## Stack

- **Jekyll + GitHub Pages** (unchanged)
- **`jekyll-github-metadata`** — kept for sidebar profile data (avatar, name, bio)
- **Primer CSS** — removed, replaced with custom stylesheet
- **`jekyll-octicons`** — removed, replaced with inline SVG icons
- **No framework changes** — plain HTML/CSS/Liquid templates

## Layout

Single-page site. Fixed sidebar on the left, main content scrolls on the right.

**Sidebar (fixed left, ~220px):**
- Avatar (circle, from GitHub metadata)
- Name (from GitHub metadata)
- Bio (from GitHub metadata)
- Contact links: GitHub, email — inline SVG icons, no icon library dependency

**Main content:**
- Featured Projects section
- Skills section

No separate contact section — contact lives in the sidebar.

**Responsive:** On mobile (< 768px), sidebar stacks above the main content.

## Visual Design

GitHub-dark color palette:

| Token | Value | Usage |
|---|---|---|
| `bg-page` | `#0d1117` | Page background |
| `bg-surface` | `#161b22` | Cards, sidebar |
| `border` | `#30363d` | All borders |
| `text-primary` | `#e6edf3` | Headings, names |
| `text-muted` | `#8b949e` | Body text, descriptions |
| `accent` | `#58a6ff` | Links, card accents, tech tag color |

Typography: system font stack (`-apple-system, BlinkMacSystemFont, Segoe UI, sans-serif`).

## Project Cards

Source: `_data/projects.yml`

Each card:
- `2px solid #58a6ff` top border accent
- `#161b22` background, `1px solid #30363d` border
- Project name (primary text)
- Description (muted text, 1-2 sentences)
- Tech tag pills (blue-tinted, pill-shaped, from `tech` array in data)
- Entire card links to `url`

Cards render in a 3-column responsive grid (2-column on medium, 1-column on mobile).

### `_data/projects.yml` schema

```yaml
- name: project-name
  description: What this project does, in one or two sentences.
  url: https://github.com/jonmilley/project-name
  tech: [TypeScript, React, Node.js]
```

## Skills Section

Source: `_data/skills.yml`

Skills grouped by category. Each category renders as a labeled group of pill tags.

### `_data/skills.yml` schema

```yaml
- category: Languages
  items: [TypeScript, JavaScript, Python]
- category: Frameworks
  items: [React, Node.js, Express]
- category: Tools
  items: [Git, Docker, PostgreSQL]
```

## Files

### New / rewritten

| File | Purpose |
|---|---|
| `_layouts/default.html` | HTML shell: `<head>`, `<body>` wrapper, meta tags, stylesheet link |
| `_layouts/home.html` | Sidebar + main content structure |
| `_includes/sidebar.html` | Avatar, name, bio, contact links |
| `_includes/projects.html` | Card grid from `_data/projects.yml` |
| `_includes/skills.html` | Grouped skill pills from `_data/skills.yml` |
| `assets/styles.scss` | Custom dark CSS, all color tokens, responsive breakpoints |
| `_data/projects.yml` | Curated project list (to be populated by owner) |
| `_data/skills.yml` | Skills grouped by category (to be populated by owner) |
| `_config.yml` | Remove unused keys (topics, style, layout toggles); keep metadata plugin |

### Deleted

| File | Reason |
|---|---|
| `_includes/header.html` | Replaced by `_layouts/default.html` + `_includes/sidebar.html` |
| `_includes/footer.html` | No footer needed |
| `_includes/masthead.html` | Replaced by `_includes/sidebar.html` |
| `_includes/interests.html` | Interests section removed |
| `_includes/thoughts.html` | Blog/thoughts section removed |
| `_includes/repo-card.html` | Replaced by new project card in `_includes/projects.html` |
| `_includes/topic-card.html` | Interests section removed |
| `_includes/post-card.html` | Blog section removed |
| `_layouts/post.html` | Blog removed |

`_posts/2019-01-29-hello-world.md` can be deleted (unpublished draft, no content).

## Out of Scope

- Blog / writing section
- Work experience / resume
- About page
- Any JavaScript interactivity
- Analytics
