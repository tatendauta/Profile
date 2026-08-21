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
| `resume.pdf` | Downloadable resume, linked from the site's "Print Resume"/"View Resume" buttons and the footer. |
| `google47b8eb96dcbce620.html` | Google Search Console site-verification file. Don't touch unless re-verifying ownership. |
| `sitemap.xml` | Single-URL sitemap pointing at the GitHub Pages URL. |
| `Tatenda Resume DS 2026.docx` / `Tatenda Resume DS 2026.pdf` | Source resume documents (not deployed — `resume.pdf` is the deployed copy; these are the editable/original versions and reference material the on-site copy is drawn from). |
| `Professional Site.txt` | An early draft of the site (older palette: green/blue "Growth" theme), saved as `.txt`. Historical reference only — not used, not deployed. |
| `index.local-backup-2026-06-27.html` | A pre-migration snapshot of `index.html` kept during the git-repo setup, when the local copy and the GitHub copy had diverged. Safe to ignore; kept for history, not deployed. |

## How the site works

`index.html` is a client-rendered single-page app with no router — `navigateTo(tabName, projectId)` (in the `<script>` block near the bottom) hides/shows `<div id="view-*">` sections and toggles active-state classes on the nav buttons. Views: `home`, `projects`, `project-detail`, `leadership`, `skills`, `writing`, `resume`, `contact`.

Project case studies live in a single `projectsData` array (JS objects with `title`, `tag`, `impactRange`, `context`, `challenge`, `approachFlow`, `outcomes`, `collaboration`, `lessonsLearned`, etc.). `renderProjectDetail(id)` and `renderProjectsList()`/`renderFeaturedProjects()` build the project cards/detail pages from this array at runtime via template strings — there's no separate template file.

Article content (`articlesData`) and leadership scenario copy (`triggerScenario()`) work the same way: inline data objects rendered via template strings on click.

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
