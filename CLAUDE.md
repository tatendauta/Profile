# Tatenda Uta — Portfolio Site

## What this is

A single-page portfolio/resume site for Tatenda Uta (AI & Analytics Decision Partner), built as one self-contained static HTML file. No build step, no package manager, no framework beyond Tailwind loaded from a CDN.

- **Live site:** https://tatendauta.github.io/Profile/ (GitHub Pages, served from the `main` branch)
- **GitHub repo:** https://github.com/tatendauta/Profile
- **Owner email:** tatendauta@gmail.com

## Files

| File | Purpose |
| :--- | :--- |
| `index.html` | The entire site — markup, Tailwind config, all styling, and all JS (navigation, project data, article/scenario content) in one file. This is what's deployed. |
| `Resume Site Color Palette.md` | The color system used across `index.html` — palette, component-by-component color rules, and anti-patterns. Update this file whenever colors change in the site; treat it as documentation of current state, not aspiration. |
| `Tatenda Uta Resume.html` | The resume the "Print Resume" button links to (`href="Tatenda%20Uta%20Resume.html"`, opened in a new tab). A self-contained "artifact bundle" HTML file — it unpacks embedded assets into blob URLs via inline JS at load time rather than being plain static markup. Don't hand-edit it; if the resume changes, regenerate/replace the whole file. |
| `resume.pdf` | Older downloadable resume copy. No longer linked from anywhere in `index.html` (superseded by `Tatenda Uta Resume.html` for the Print Resume button) — kept in the repo but currently orphaned; ask before deleting. |
| `google47b8eb96dcbce620.html` | Google Search Console site-verification file. Don't touch unless re-verifying ownership. |
| `sitemap.xml` | Single-URL sitemap pointing at the GitHub Pages URL. |
| `Tatenda Resume DS 2026.docx` / `Tatenda Resume DS 2026.pdf` | Source resume documents (not deployed — `resume.pdf` is the deployed copy; these are the editable/original versions and reference material the on-site copy is drawn from). |
| `Professional Site.txt` | An early draft of the site (older palette: green/blue "Growth" theme), saved as `.txt`. Historical reference only — not used, not deployed. |
| `index.local-backup-2026-06-27.html` | A pre-migration snapshot of `index.html` kept during the git-repo setup, when the local copy and the GitHub copy had diverged. Safe to ignore; kept for history, not deployed. |

## How the site works

`index.html` is a client-rendered single-page app with no router — `navigateTo(tabName, projectId)` (in the `<script>` block near the bottom) hides/shows `<div id="view-*">` sections and toggles active-state classes on the nav buttons. Views: `home`, `projects`, `project-detail`, `leadership`, `skills`, `writing`, `resume`, `contact`.

Project case studies live in a single `projectsData` array (JS objects with `title`, `tag`, `impactRange`, `context`, `challenge`, `approachFlow`, `outcomes`, `collaboration`, `lessonsLearned`, etc.). `renderProjectDetail(id)` and `renderProjectsList()`/`renderFeaturedProjects()` build the project cards/detail pages from this array at runtime via template strings — there's no separate template file.

The on-site **Resume Experience section** (`<div id="resume-experience">`) and the **Insights sidebar menu** (`<div id="articles-menu">`) are also data-driven, not static HTML — see "Adding a new project" below for how they connect to `projectsData`.

Article content (`articlesData`) and leadership scenario copy (`triggerScenario()`) work the same way: inline data objects rendered via template strings on click.

### Adding a new project

When the user describes something new they've completed, add it to `projectsData` (full field list: `id, title, subtitle, tag, impactRange, impactMetric, focus, context, challenge, role, approachFlow, approachDetail, outcomes, collaboration, lessonsLearned` — ask for whatever's missing rather than requiring a rigid template). `tag` must be one of the four values with an existing filter button (`Revenue & Conversion`, `Metric Integrity`, `Experimentation`, `Risk & Compliance`) — a new tag value needs a new filter button hand-added near line ~437, since that part isn't data-driven.

To also put it on the on-site resume, add four more optional fields to the same project object:
- `employer` — must match a `key` in `employerMeta` (currently `'justanswer'`, `'ashley'`, `'techdata'`)
- `resumeCategory` — optional subcategory heading (omit for employers that list bullets flat); if used, must also appear in that employer's `resumeCategoryOrder` in `employerMeta`, or it won't be positioned predictably
- `resumeOrder` — a number controlling bullet order within its category (sorted ascending) — **required whenever `resumeCategory` is set**, since `projectsData`'s own array order doesn't drive resume order
- `resumeBulletLabel` (optional, omit for an unlabeled flat bullet) and `resumeBulletText` — the bold lead-in and bullet prose

Don't add these four fields if the project shouldn't appear on the resume — ask the user.

For an **Insights article**, ask the user once whether they want one (don't assume yes) — if so, add an entry to `articlesData` with the next unused numeric key and all of: `title`, `tag`, `teaser` (the short sidebar preview line), `p1`/`p2`/`p3` (exactly three paragraphs; the reader UI doesn't support more or fewer). `color` is a vestigial field (both values render identically now) — fine to set `"brand"` by convention, doesn't matter functionally.

After any of the above: run a Node syntax check on the extracted `<script>` block (`split` on `<script>`/`</script>`, `new Function(js)`), then open `index.html` in a browser and visually confirm Projects/Resume (incl. print preview)/Insights all look right before considering it done.

`submitForm()` (bottom of the script) is dead code — it references a `#contact-form`/`#form-name`/`#form-status` that don't exist in the current markup (no actual `<form>` in the Contact view, just mailto/LinkedIn link cards). Leave it alone unless you're deliberately adding a real contact form.

Print support: the Resume view has `print:` Tailwind variants and a `@media print` block in the `<style>` tag, used when the user clicks "Print Resume" or hits browser print — verify print preview after any change to the Resume view.

## Styling conventions

- Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com">`), classes compiled client-side — no build step, so any valid Tailwind class works immediately.
- **Use stock Tailwind color classes directly** (`text-slate-900`, `bg-blue-600`, `bg-emerald-50/80`, etc.) — do not introduce new arbitrary-value hex classes (`text-[#...]`) or new custom `tailwind.config` color tokens. See `Resume Site Color Palette.md` for the full system and which color means what.
- Emerald is reserved strictly for revenue/dollar and verified-positive indicators. Don't use it as a generic accent — that's what blue is for.
- Borders/dividers use `border-slate-200` (default) / `border-slate-300` (hover), not translucent `border-black/NN` utilities.

## Working with the git repo

- This folder is a git repo tracking `origin/main` = `https://github.com/tatendauta/Profile.git`. It was connected to an already-existing remote history (not created fresh) — the remote had its own commits before this local folder was wired up, so treat `origin/main` as authoritative history, not something to force-push over.
- Deploys are just a `git push` to `main` — GitHub Pages serves directly from it, no CI/build step.
- Nothing here is committed automatically — commit and push only when explicitly asked.

## Known quirks worth knowing before you touch things

- There is no dev server / build tooling to run. To preview a change, just open `index.html` directly in a browser.
- `darkMode` is not configured and no `dark:` classes exist — the site is light-mode only by design, not by omission.
- The favicon is an inline SVG data-URI in a `<link>` tag (not a separate file) — its "TU" fill color is kept in sync with the header logo badge (currently both: white background, cobalt blue `#2563EB` text) by convention, not by shared code. If you change one, change the other.
