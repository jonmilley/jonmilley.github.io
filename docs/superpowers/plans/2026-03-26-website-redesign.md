# Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild jonmilley.github.io as a dark-themed personal portfolio with curated projects and skills, replacing the archived github/personal-website template with clean custom layouts.

**Architecture:** Keep Jekyll + GitHub Pages. Replace all `_layouts/` and `_includes/` HTML with clean custom files. Drop Primer CSS and `jekyll-octicons`, write a custom dark stylesheet. Use `_data/projects.yml` and `_data/skills.yml` for content, and keep `jekyll-github-metadata` for sidebar profile data.

**Tech Stack:** Jekyll, Liquid templates, SCSS, `jekyll-github-metadata`

---

## File Map

| Action | File | Purpose |
|---|---|---|
| Modify | `.gitignore` | Add `.superpowers/` |
| Create | `_data/projects.yml` | Curated project list |
| Create | `_data/skills.yml` | Skills grouped by category |
| Rewrite | `_layouts/default.html` | HTML shell (`<head>`, `<body>` wrapper) |
| Rewrite | `_includes/sidebar.html` | Avatar, name, bio, contact links |
| Rewrite | `_includes/projects.html` | Project card grid |
| Create | `_includes/skills.html` | Grouped skill pill tags |
| Rewrite | `_layouts/home.html` | Sidebar + main layout |
| Rewrite | `assets/styles.scss` | Full custom dark CSS |
| Modify | `_config.yml` | Remove old keys, keep metadata plugin |
| Delete | `_includes/header.html` | Replaced by default layout + sidebar |
| Delete | `_includes/footer.html` | No footer needed |
| Delete | `_includes/masthead.html` | Replaced by sidebar |
| Delete | `_includes/interests.html` | Section removed |
| Delete | `_includes/thoughts.html` | Blog section removed |
| Delete | `_includes/repo-card.html` | Replaced by project card |
| Delete | `_includes/topic-card.html` | Interests section removed |
| Delete | `_includes/post-card.html` | Blog section removed |
| Delete | `_layouts/post.html` | Blog removed |
| Delete | `_posts/2019-01-29-hello-world.md` | Unpublished draft |

---

## Task 1: Update .gitignore and create data files

**Files:**
- Modify: `.gitignore`
- Create: `_data/projects.yml`
- Create: `_data/skills.yml`

- [ ] **Step 1: Add `.superpowers/` to `.gitignore`**

Append to `.gitignore`:
```
.superpowers/
```

- [ ] **Step 2: Create `_data/projects.yml` with placeholder content**

Create `_data/projects.yml`:
```yaml
- name: my-project
  description: A short description of what this project does and why it matters.
  url: https://github.com/jonmilley/my-project
  tech: [TypeScript, React, Node.js]

- name: another-project
  description: Another project description. One or two sentences is ideal.
  url: https://github.com/jonmilley/another-project
  tech: [Python, FastAPI, PostgreSQL]

- name: third-project
  description: A third project to fill the grid. Replace with real projects.
  url: https://github.com/jonmilley/third-project
  tech: [Go, Docker]
```

- [ ] **Step 3: Create `_data/skills.yml` with placeholder content**

Create `_data/skills.yml`:
```yaml
- category: Languages
  items: [TypeScript, JavaScript, Python]
- category: Frameworks
  items: [React, Node.js, Express]
- category: Tools
  items: [Git, Docker, PostgreSQL]
```

- [ ] **Step 4: Commit**

```bash
git add .gitignore _data/projects.yml _data/skills.yml
git commit -m "Add data files and update gitignore"
```

---

## Task 2: Rewrite the default layout

**Files:**
- Rewrite: `_layouts/default.html`

- [ ] **Step 1: Replace `_layouts/default.html`**

```html
{% assign user = site.github.owner %}
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{% if user.name %}{{ user.name }}{% else %}{{ user.login }}{% endif %}</title>
    <link href="{{ "/assets/styles.css" | absolute_url }}" rel="stylesheet">
  </head>
  <body>
    {{ content }}
  </body>
</html>
```

- [ ] **Step 2: Verify the build compiles**

```bash
bundle exec jekyll build 2>&1
```

Expected: build completes. Errors at this point are about missing includes/layouts used by `home.html` — that is fine; those will be fixed in later tasks. If you see a Ruby/gem error, that is not expected.

- [ ] **Step 3: Commit**

```bash
git add _layouts/default.html
git commit -m "Rewrite default layout: clean HTML shell, drop Primer CSS"
```

---

## Task 3: Rewrite the sidebar include

**Files:**
- Rewrite: `_includes/sidebar.html`

- [ ] **Step 1: Replace `_includes/sidebar.html`**

Create or overwrite `_includes/sidebar.html` (this file may not exist yet — create it):
```html
{% assign user = site.github.owner %}
<aside class="sidebar">
  <img src="{{ user.avatar_url }}" alt="{{ user.name }}" class="sidebar__avatar">
  <h1 class="sidebar__name">{% if user.name %}{{ user.name }}{% else %}{{ user.login }}{% endif %}</h1>
  {% if user.bio %}
    <p class="sidebar__bio">{{ user.bio }}</p>
  {% endif %}
  <ul class="sidebar__links">
    <li>
      <a href="https://github.com/{{ user.login }}" class="sidebar__link">
        <svg class="sidebar__icon" viewBox="0 0 16 16" width="16" height="16" aria-hidden="true">
          <path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"/>
        </svg>
        @{{ user.login }}
      </a>
    </li>
    {% if user.email %}
    <li>
      <a href="mailto:{{ user.email }}" class="sidebar__link">
        <svg class="sidebar__icon" viewBox="0 0 16 16" width="16" height="16" aria-hidden="true">
          <path fill-rule="evenodd" d="M1.75 2A1.75 1.75 0 000 3.75v.736a.75.75 0 000 .027V12.25c0 .966.784 1.75 1.75 1.75h12.5A1.75 1.75 0 0016 12.25V3.75A1.75 1.75 0 0014.25 2H1.75zM14.5 4.07v-.32a.25.25 0 00-.25-.25H1.75a.25.25 0 00-.25.25v.32L8 7.88l6.5-3.81zm-13 1.74v6.441l3.769-3.966L1.5 5.81zm9.256 3.475l3.744 3.966V5.809l-3.744 1.476zM2.115 12.5h11.77l-4.13-4.354L8 9.62l-1.755-1.474L2.115 12.5z"/>
        </svg>
        {{ user.email }}
      </a>
    </li>
    {% endif %}
  </ul>
</aside>
```

- [ ] **Step 2: Commit**

```bash
git add _includes/sidebar.html
git commit -m "Add sidebar include: avatar, name, bio, contact links with inline SVG"
```

---

## Task 4: Rewrite the projects include

**Files:**
- Rewrite: `_includes/projects.html`

- [ ] **Step 1: Replace `_includes/projects.html`**

```html
<section class="section" id="projects">
  <h2 class="section__title">Featured Projects</h2>
  <div class="projects-grid">
    {% for project in site.data.projects %}
      <a href="{{ project.url }}" class="project-card" target="_blank" rel="noopener noreferrer">
        <h3 class="project-card__name">{{ project.name }}</h3>
        <p class="project-card__description">{{ project.description }}</p>
        {% if project.tech %}
          <ul class="project-card__tech">
            {% for tag in project.tech %}
              <li class="project-card__tag">{{ tag }}</li>
            {% endfor %}
          </ul>
        {% endif %}
      </a>
    {% endfor %}
  </div>
</section>
```

- [ ] **Step 2: Commit**

```bash
git add _includes/projects.html
git commit -m "Rewrite projects include: curated card grid from _data/projects.yml"
```

---

## Task 5: Create the skills include

**Files:**
- Create: `_includes/skills.html`

- [ ] **Step 1: Create `_includes/skills.html`**

```html
<section class="section" id="skills">
  <h2 class="section__title">Skills</h2>
  {% for group in site.data.skills %}
    <div class="skills-group">
      <h3 class="skills-group__category">{{ group.category }}</h3>
      <ul class="skills-group__items">
        {% for item in group.items %}
          <li class="skills-group__tag">{{ item }}</li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</section>
```

- [ ] **Step 2: Commit**

```bash
git add _includes/skills.html
git commit -m "Add skills include: grouped tag pills from _data/skills.yml"
```

---

## Task 6: Rewrite the home layout

**Files:**
- Rewrite: `_layouts/home.html`

- [ ] **Step 1: Replace `_layouts/home.html`**

```html
---
layout: default
---
<div class="layout">
  {% include sidebar.html %}
  <main class="main">
    {% include projects.html %}
    {% include skills.html %}
  </main>
</div>
```

- [ ] **Step 2: Verify the site builds cleanly**

```bash
bundle exec jekyll build 2>&1
```

Expected: build completes with no errors. At this point the layouts reference `sidebar.html`, `projects.html`, and `skills.html` — all created in previous tasks. If you see a Liquid error about a missing include, check that all three files from Tasks 3-5 exist in `_includes/`.

- [ ] **Step 3: Commit**

```bash
git add _layouts/home.html
git commit -m "Rewrite home layout: sidebar + main, remove stacked/dark toggle"
```

---

## Task 7: Write the custom CSS

**Files:**
- Rewrite: `assets/styles.scss`

- [ ] **Step 1: Replace `assets/styles.scss`**

```scss
---
---
:root {
  --bg-page: #0d1117;
  --bg-surface: #161b22;
  --border: #30363d;
  --text-primary: #e6edf3;
  --text-muted: #8b949e;
  --accent: #58a6ff;
  --accent-subtle: rgba(88, 166, 255, 0.1);
  --accent-border: rgba(88, 166, 255, 0.2);
}

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: var(--bg-page);
  color: var(--text-primary);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
  font-size: 14px;
  line-height: 1.6;
  min-height: 100vh;
}

a {
  color: var(--accent);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

ul {
  list-style: none;
}

/* Layout */
.layout {
  display: flex;
  min-height: 100vh;
}

/* Sidebar */
.sidebar {
  width: 240px;
  flex-shrink: 0;
  background: var(--bg-surface);
  border-right: 1px solid var(--border);
  padding: 32px 24px;
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

.sidebar__avatar {
  display: block;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 2px solid var(--border);
  margin-bottom: 16px;
}

.sidebar__name {
  font-size: 18px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 8px;
  line-height: 1.3;
}

.sidebar__bio {
  font-size: 13px;
  color: var(--text-muted);
  margin-bottom: 24px;
  line-height: 1.6;
}

.sidebar__links {
  display: flex;
  flex-direction: column;
  gap: 10px;
  border-top: 1px solid var(--border);
  padding-top: 20px;
}

.sidebar__link {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-muted);
  transition: color 0.15s;
}

.sidebar__link:hover {
  color: var(--accent);
  text-decoration: none;
}

.sidebar__icon {
  fill: currentColor;
  flex-shrink: 0;
}

/* Main content */
.main {
  flex: 1;
  padding: 40px;
  max-width: 960px;
}

/* Sections */
.section {
  margin-bottom: 48px;
}

.section__title {
  font-size: 20px;
  font-weight: 600;
  color: var(--text-primary);
  padding-bottom: 8px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 20px;
}

/* Projects grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

/* Project card */
.project-card {
  display: block;
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-top: 2px solid var(--accent);
  border-radius: 6px;
  padding: 16px;
  color: var(--text-primary);
  transition: border-color 0.15s;
}

.project-card:hover {
  border-color: var(--accent);
  text-decoration: none;
}

.project-card__name {
  font-size: 14px;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 8px;
}

.project-card__description {
  font-size: 12px;
  color: var(--text-muted);
  margin-bottom: 12px;
  line-height: 1.6;
}

.project-card__tech {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.project-card__tag {
  font-size: 11px;
  color: var(--accent);
  background: var(--accent-subtle);
  border: 1px solid var(--accent-border);
  border-radius: 20px;
  padding: 3px 10px;
}

/* Skills */
.skills-group {
  margin-bottom: 20px;
}

.skills-group__category {
  font-size: 11px;
  font-weight: 600;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  margin-bottom: 10px;
}

.skills-group__items {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.skills-group__tag {
  font-size: 12px;
  color: var(--text-muted);
  background: var(--bg-surface);
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 4px 10px;
}

/* Responsive */
@media (max-width: 768px) {
  .layout {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    height: auto;
    position: static;
    border-right: none;
    border-bottom: 1px solid var(--border);
    padding: 24px 20px;
  }

  .sidebar__bio {
    margin-bottom: 16px;
  }

  .main {
    padding: 24px 20px;
  }

  .projects-grid {
    grid-template-columns: 1fr;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

- [ ] **Step 2: Verify the build produces a stylesheet**

```bash
bundle exec jekyll build 2>&1 && ls _site/assets/
```

Expected: `styles.css` appears in `_site/assets/`. The build should complete with no errors.

- [ ] **Step 3: Commit**

```bash
git add assets/styles.scss
git commit -m "Add custom dark CSS: GitHub-dark palette, sidebar layout, project cards, skills"
```

---

## Task 8: Update `_config.yml`

**Files:**
- Modify: `_config.yml`

- [ ] **Step 1: Replace `_config.yml`**

The current file has `style: dark`, `include.metadata: true`, `topics:`, and the layout/style toggles. Remove all of that — only the permalink config and metadata plugin are needed:

```yaml
plugins:
  - jekyll-github-metadata

permalink: /:year/:month/:day/:title/
```

- [ ] **Step 2: Verify the build still works**

```bash
bundle exec jekyll build 2>&1
```

Expected: clean build. The `jekyll-github-metadata` plugin will still inject `site.github.owner` into templates.

- [ ] **Step 3: Commit**

```bash
git add _config.yml
git commit -m "Simplify _config.yml: remove unused style/layout/topics config"
```

---

## Task 9: Delete old files

**Files:**
- Delete: `_includes/header.html`, `_includes/footer.html`, `_includes/masthead.html`, `_includes/interests.html`, `_includes/thoughts.html`, `_includes/repo-card.html`, `_includes/topic-card.html`, `_includes/post-card.html`
- Delete: `_layouts/post.html`
- Delete: `_posts/2019-01-29-hello-world.md`

- [ ] **Step 1: Delete old includes and layouts**

```bash
git rm _includes/header.html _includes/footer.html _includes/masthead.html \
       _includes/interests.html _includes/thoughts.html _includes/repo-card.html \
       _includes/topic-card.html _includes/post-card.html \
       _layouts/post.html \
       _posts/2019-01-29-hello-world.md
```

- [ ] **Step 2: Verify clean build after deletion**

```bash
bundle exec jekyll build 2>&1
```

Expected: clean build with no Liquid errors about missing includes. All old includes are gone; none of the new templates reference them.

- [ ] **Step 3: Preview the site locally**

```bash
bundle exec jekyll serve 2>&1
```

Open `http://localhost:4000` and verify:
- Dark background (`#0d1117`)
- Sidebar on the left with avatar, name, bio, GitHub link
- Projects section with card grid
- Skills section with grouped tag pills
- Mobile: stack sidebar above main content at narrow widths

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "Remove old template files: header, footer, masthead, interests, thoughts, repo-card, topic-card, post-card, post layout"
```

---

## Task 10: Populate real content

**Files:**
- Modify: `_data/projects.yml`
- Modify: `_data/skills.yml`

- [ ] **Step 1: Replace placeholder projects with real ones in `_data/projects.yml`**

Replace the three placeholder entries with your actual projects. Each entry:
```yaml
- name: <repo-name-or-display-name>
  description: <1-2 sentence description of what it does>
  url: https://github.com/jonmilley/<repo>
  tech: [<tech1>, <tech2>]
```

- [ ] **Step 2: Replace placeholder skills with real ones in `_data/skills.yml`**

Replace the placeholder groups/items with your actual skills. Add or remove categories as needed:
```yaml
- category: Languages
  items: [TypeScript, JavaScript, ...]
- category: Frameworks
  items: [React, ...]
- category: Tools
  items: [Git, ...]
```

- [ ] **Step 3: Verify locally**

```bash
bundle exec jekyll serve 2>&1
```

Open `http://localhost:4000` and confirm projects and skills render correctly.

- [ ] **Step 4: Commit**

```bash
git add _data/projects.yml _data/skills.yml
git commit -m "Add real project and skills content"
```
