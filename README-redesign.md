# Portfolio redesign maintenance guide

This site is built with Jekyll and uses the custom dark portfolio theme.

## Preview locally

1. Install Ruby and Bundler, then install the dependencies used by GitHub Pages/Jekyll.
2. From the repository root, run `bundle exec jekyll serve`.
3. Open `http://localhost:4000`.

Alternatively, download the `portfolio-preview` artifact from the GitHub Actions workflow, extract it, open a terminal in the folder containing `index.html`, run `py -m http.server 8000`, and visit `http://localhost:8000`.

## Updating content

- Homepage introduction and background: `index.html`.
- Homepage project cards: `_data/projects.yml`.
- Individual project and experience writeups: `capstone.md`, `spear.md`, `robotics.md`, `ai.md`, and `wkexp.md`.
- Site navigation and shared page structure: `_layouts/default.html`.
- Shared page wrapper: `_layouts/page.html`.
- Visual styling: `assets/css/main.css`.

## Adding a project

Add an entry to `_data/projects.yml` with a title, category, short description, destination URL, image filename, accessible image alt text, and display number. Add the corresponding Markdown page at the repository root. Give the page front matter `layout: page` and a descriptive `title`.

Keep image filenames and paths in sync with files committed to the repository. The homepage image paths are relative to the repository root.
