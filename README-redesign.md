# Portfolio redesign maintenance guide

This repository contains the source for the portfolio website. It is built with [Jekyll](https://jekyllrb.com/) and GitHub Pages, using Markdown for most project write-ups and a small amount of HTML, Liquid, YAML, and CSS for the shared site design.

The redesign is intentionally split into content, layout, and styling files. For routine updates—such as changing a project description, adding an image, or updating work experience—you should usually only need to edit a Markdown file, `index.html`, or `_data/projects.yml`. Changes to the shared layouts or stylesheet affect the whole site, so make those when you want to change the design or navigation rather than an individual piece of content.

## Preview locally

### Option 1: Run the Jekyll site

1. Install Ruby and Bundler, then install the dependencies used by GitHub Pages/Jekyll.
2. From the repository root, run `bundle exec jekyll serve`.
3. Open [http://localhost:4000](http://localhost:4000).
4. Jekyll rebuilds the site as you edit files. Refresh the browser to see the changes.

### Option 2: Preview the GitHub Actions artifact

1. Open the repository's **Actions** tab and select a successful **Validate portfolio site** run.
2. Download the `portfolio-preview` artifact and extract it.
3. Open a terminal in the extracted folder that contains `index.html` (if the archive has a nested `_site` folder, use that folder).
4. Run `py -m http.server 8000` on Windows, or `python3 -m http.server 8000` on macOS/Linux.
5. Visit [http://localhost:8000](http://localhost:8000).

Do not preview the built `index.html` by double-clicking it. The site uses root-relative asset paths, and opening it with a `file://` URL can prevent the stylesheet and images from loading correctly. Use a local web server as described above.

## Updating content

This section describes the purpose of the main files and gives practical instructions for editing each one.

### Homepage content — `index.html`

**What it controls:** The homepage's introduction, headline, profile photo, education and current-role facts, selected-work heading, background paragraph, and areas-of-interest tags. It also contains the Liquid loop that displays the project cards configured in `_data/projects.yml`.

**How to update it:**

- Edit the visible text directly in the relevant HTML element. For example, change the introduction inside `<p class="hero-intro">`, or update the education and role text inside the `hero-facts` section.
- Change the homepage headline by editing the text inside the `<h1>` element. Keep the surrounding `<span>` if you want part of the headline to retain the accent colour.
- Replace the profile image by changing the image filename in the `portrait-wrap` image's `src`. The file must exist in the repository, and the `alt` text should describe it.
- Update the background paragraph in the `about-strip` section.
- Add, remove, or rename interest tags by editing the `<span>` elements inside `interest-tags`.
- Project cards are generated automatically from `_data/projects.yml`. Do not copy and paste another card into this file; add or edit a data entry instead.

Keep the YAML front matter at the very top of the file (`layout: default`, `title: Home`, and `home: true`). The `home: true` flag tells the shared layout to render the homepage without the document-page wrapper.

### Homepage project cards — `_data/projects.yml`

**What it controls:** The list of cards in the homepage's Selected Work section. Each list item represents one project or experience entry.

**How to update it:** Edit the existing item or add another item using the same YAML structure. Each item has these fields:

- `title`: The project name shown as the card heading.
- `category`: The small uppercase-style label above the title (for example, a project type or technology area).
- `description`: The short summary shown on the homepage card. Keep it concise; the full explanation belongs on the linked project page.
- `url`: The destination page, normally a root-level Markdown page's generated HTML URL such as `/robotics.html`.
- `image`: The filename of the card image. Store the image in the repository root unless you also change the path convention in the homepage template.
- `image_alt`: A useful text description of the image for accessibility.
- `number`: The small display number shown over the image. Keep the format consistent with the other cards (for example, `06`).

YAML formatting matters. Use spaces for indentation (not tabs), keep each field aligned under its list item, and put quotation marks around text values. If a description contains punctuation such as a colon, quoting the value avoids YAML parsing problems.

To add a project, first create its detail page (see the Markdown page instructions below), add its image file to the repository, and then add a new item here. Make sure the `url` points to the generated page and the `image` filename matches the image exactly, including capitalization and extension.

### Individual project and experience pages — root-level Markdown files

The main write-ups are:

- `capstone.md`: The Robot Arm Satellite Positioning Simulator (RASPS) computer-engineering capstone, including objectives, the arm, rail, control system, and result.
- `spear.md`: Space Exploration Alberta Robotics (SPEAR), including rover software, electrical hardware, firmware, and the 2024 Canadian International Rover Challenge.
- `robotics.md`: FIRST Robotics Competition experience with Team 4334 and Calgary Tech Coalition, organized by the 2015, 2016, and 2017 seasons.
- `ai.md`: Machine-learning and AI coursework, research-related material, Udacity projects, and links to notebooks and certificates. This page also includes the explanation of AI's role in creating the portfolio.
- `wkexp.md`: Professional and research experience, including Subnet Solutions and the University of Alberta Centre for Smart Transportation.

**What they control:** The body content of the corresponding detail page. Jekyll converts the Markdown into HTML and wraps it in the shared page layout.

**How to update them:**

- Open the relevant `.md` file and edit the text under the front matter. Use `##` for a section heading, `###` for a subsection, `-` or `*` for a bullet, and numbered Markdown list syntax for ordered steps.
- Keep the front matter at the top of the file. It should include `layout: page` and a descriptive `title:`. Do not remove the opening and closing `---` lines.
- Add a new section by inserting a heading and its paragraphs in the appropriate place. For example: `## Project Results`.
- Link to another page with a relative link such as `[Work Experience](wkexp.html)`, or to an external website with its full HTTPS URL.
- Add an image with Markdown syntax such as `![Description of the image](my-photo.jpg)`. The image file must also be committed to the repository. The existing pages sometimes use HTML `<img>` tags when a specific display width is needed.
- Add a video or notebook link using normal Markdown links. Check that the linked file or external URL exists before publishing.
- When adding a new page, create a new root-level `.md` file with the same front matter pattern. Add a homepage card in `_data/projects.yml` if it should appear under Selected Work.

The existing navigation links at the top of some older Markdown pages are part of their page content. You can update those links if you add, rename, or remove a page. The main site navigation is separate and lives in `_layouts/default.html`.

### Shared site layout and navigation — `_layouts/default.html`

**What it controls:** The common HTML document structure used across the site: browser title and metadata, font loading, stylesheet reference, ambient background elements, header and navigation links, page container, and footer. It also decides whether to display the homepage content directly or place a detail page inside the document panel.

**How to update it:**

- Edit the links inside `<nav class="main-nav">` to change the navigation menu. Use Jekyll's `relative_url` filter for internal links so they continue to work if the site is hosted under a path prefix.
- Update the wordmark or footer text in the relevant header/footer elements.
- Change the browser description or title logic in the `<head>` section if the site's metadata needs to change.
- The Google Fonts link currently loads Manrope and DM Mono. Change that link if you deliberately choose different fonts, and update the corresponding font variables in the CSS.
- The line containing `{{ content }}` is important: it inserts the page's generated content. Keep it in place.
- The conditional using `page.home` determines which wrapper is used. Preserve it unless you intend to change the structure of both the homepage and detail pages.

This file affects every page. A broken tag or Liquid expression here can break the entire site, so preview the homepage and at least one detail page after editing it.

### Detail-page wrapper — `_layouts/page.html`

**What it controls:** The layout assigned to ordinary content pages. It delegates the overall structure to `default` and outputs the page's actual content.

**How to update it:** In normal use, you should not need to edit this file. It must retain the front matter `layout: default` and the Liquid expression `{{ content }}`. If that expression is removed, the shared header and footer may still appear but the Markdown page's description and images will not render.

### Visual design — `assets/css/main.css`

**What it controls:** The site's colours, typography, spacing, page widths, backgrounds, borders, buttons, project cards, image treatment, document panels, hover effects, and responsive layouts for smaller screens.

**How to update it:**

- The `:root` block at the top defines the main design variables: `--bg` (page background), `--panel` (panel colour), `--line` (subtle borders), `--text` (primary text), `--muted` (secondary text), `--accent` (teal highlight), and the font-family variables.
- Change a variable in `:root` to make a global colour or font adjustment rather than hunting through every selector.
- The homepage rules are grouped around selectors such as `.hero`, `.hero-visual`, `.section-heading`, `.project-grid`, `.project-card`, `.about-strip`, and `.interest-row`.
- The shared header and footer use `.site-header`, `.main-nav`, and `.site-footer`.
- Detail-page appearance is controlled mainly by `.document-topline`, `.document-panel`, and the headings, paragraphs, links, images, and lists nested inside it.
- The `@media(max-width:800px)` and `@media(max-width:540px)` sections contain tablet and phone adjustments. If you add a wide component or change the desktop layout, check that it still fits at these breakpoints.

Prefer changing an existing rule or variable over adding a second conflicting rule. Keep the CSS readable, and check the result at both desktop and mobile widths.

### Site configuration — `_config.yml`

**What it controls:** Jekyll and GitHub Pages settings, including the remote Slate theme dependency, the remote-theme plugin, site title and description, canonical site URL, Markdown processor, default page layout, and excluded files.

**How to update it:**

- Change `title` or `description` to update the site's default metadata.
- Change `url` only if the canonical website address changes. `baseurl` is currently empty because the site is hosted at the domain root.
- The `defaults` block assigns the `page` layout to ordinary pages. Keep it unless you intentionally want a page to use a different layout.
- The `remote_theme` and `plugins` entries are part of the GitHub Pages/Jekyll theme setup. Do not remove them without replacing the build configuration.
- `exclude` prevents the maintenance guide from being copied into the generated website.

YAML indentation is significant. Use spaces, not tabs.

### Automated build and preview — `.github/workflows/validate-site.yml`

**What it controls:** The GitHub Actions workflow that builds the Jekyll site when changes are pushed to the redesign branch, when a pull request targets `main`, or when the workflow is manually run. It also uploads the generated `_site` directory as a downloadable artifact named `portfolio-preview`.

**How to update it:** You normally should not need to change this file when editing site content. If you change the build process, update the relevant workflow steps and keep the artifact output path consistent with the Jekyll destination. The workflow's success indicates that Jekyll built the site; it does not replace a visual review in a browser.

### Images, notebooks, and other supporting files

Image files such as `headshot.jpg`, `capstone.jpg`, `ata2015.jpg`, `CTC.jpg`, `ata2017.png`, `kipp_electrical.jpg`, `circ_team.jpg`, `circ_task.jpg`, and `pointcloud.png` are supporting content assets. The Markdown pages and homepage data refer to them by filename. To replace an image, add the replacement file and update the corresponding reference in `index.html`, `_data/projects.yml`, or the relevant Markdown page. Match the filename's capitalization exactly; GitHub Pages builds on Linux, where `Photo.jpg` and `photo.jpg` are different files.

The AI page also links to supporting notebook exports and a certificate image, including `466project.html`, `Image Classifier Project.html`, `finding_donors.html`, and `certificate.png`. Update the links in `ai.md` if any of those files are renamed or moved. Keep supporting files in the repository so the links remain available on the published site.

## Adding a project from start to finish

1. Add the project images or other supporting assets to the repository.
2. Create a root-level Markdown page, for example `new-project.md`. Add front matter with `layout: page` and a descriptive `title`, then write the project description using Markdown.
3. Add an entry to `_data/projects.yml` with a title, category, short description, destination URL (for example, `/new-project.html`), image filename, image alt text, and display number.
4. If the new page should appear in the main navigation, add a link in `_layouts/default.html`. If relevant, update the navigation links embedded in existing Markdown pages too.
5. Preview the homepage and the new detail page. Check that the card image, links, and page content all load correctly at desktop and mobile widths.

## Before publishing

- Confirm that every referenced image, notebook, video, and internal page exists in the repository.
- Check filenames and capitalization carefully.
- Preview both the homepage and several detail pages; a successful build alone cannot catch every visual or content issue.
- Check the browser console if something appears missing, and confirm that internal links use the correct generated `.html` path.
- Keep content edits in the content files and design edits in the layouts or stylesheet where possible. This makes future updates easier and reduces the risk of unintentionally changing other pages.
