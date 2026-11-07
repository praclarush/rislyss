# Rislyss site

Static character site for CMDR Rislyss (Sarah Waters), Elite Dangerous. Public repo `praclarush/rislyss`; GitHub Pages serves `main` at https://praclarush.github.io/rislyss/. No build step: edit the HTML directly. `README.md` maps the files; read it first.

The content comes from the GameDocs repo (`%USERPROFILE%\Documents\Games`): the game journal, `Elite Dangerous\Flight Log\Docs\` (character, medical and psychological source documents, Log Notes, Inara logbook) and `Elite Dangerous\Fleet\`. Its `Elite Dangerous\CLAUDE.md` (Flight Log section) holds the writing rules, and its `update-logs` skill runs the "update logs" workflow that edits this site. Follow both when changing content here.

## Publishing

- Commit and push `main` to publish; Pages rebuilds in about a minute (`gh api repos/praclarush/rislyss/pages/builds/latest`). The repo's own `credential.helper` uses `gh auth git-credential`, so pushes need no global git setup.
- Everything in the repo is public, including this file and `README.md`. Never publish nude concept images. Never publish the site as a claude.ai artifact.
- `.nojekyll` must stay: it stops Jekyll from processing the pages.

## Page conventions

- Every page is self-contained: one inline `<style>` for the page, a second inline `<style>` for the site nav, inline scripts, fonts from Google Fonts only. No shared CSS or JS files. Each page has a `<meta name="description">`; the nav block opens with the reduced-motion rule.
- Shared palette, repeated in each page's `:root`: `--bg #0c0a08`, `--panel #15110d`, `--line #2e251c`, `--fg #ece3d7`, `--muted #998b7b`, `--accent #ff7a1a`, `--accent-dim #6b3a14`, `--cyan #74d4e2`; fonts Chakra Petch (display), Barlow (body), JetBrains Mono (mono). Single dark theme by design.
- The site nav is copied into every page, chapter pages included, with `aria-current="page"` on the current section. Adding a page means adding its link to the nav in every page. At 900px and below the nav hides the brand and wraps to two centred rows; keep that rule identical across pages.
- Each page's first CSS comment (`/* Layout: ... */`) states its design intent. Keep it accurate when the layout changes.
- Lightbox: `button.zoom` with `data-src` and `data-cap` around an `<img>`, one `dialog.lightbox` per page and a small script, as in `gallery.html`, `index.html` and `hangar.html`. Copy that pattern for new zoomable images.
- Paths are relative. Pages in `chronicle/` prefix site links with `../`.
- Line endings follow `core.autocrlf`; scripts that rewrite pages keep each file's existing newline style.

## Voices

Each page has its own voice; match it when writing:

| Page | Voice |
|---|---|
| `index.html` | Formal Pilots Federation personnel file |
| `chronicle.html`, `chronicle/` | Precise third-person technical record |
| `log.html` | Factual journal data, real times |
| `engineers.html` | Factual record of engineer grades and Inara awards |
| `medical.html` | Clinical Pilots Federation medical record |
| `psychological.html` | Clinical Pilots Federation psychological assessment |

In the chronicle keep time frames ambiguous (no day dates or short durations, but start-up and shutdown times and month gaps stay) and keep game mechanics out (logins, menus, relaunches, crashes). Real times and relaunches belong only in `log.html`.

## Chronicle

- `chronicle.html` is the index: header, prologue (`#prologue`), then a two-column block with "Where she stands now" (`#now`) on the left and the chapter list on the right, stacking at 900px and below.
- Each chapter is `chronicle/<n>.html`: the chapter `<section class="chapter">` with its label (`Chapter seven · October 3312`), `<h1>` title, optional lead paragraphs and `<article class="entry">` entries, then a pager to the previous and next chapter. Chapter 1's previous link is `../chronicle.html#prologue`; the last chapter's next link is `../chronicle.html#now`.
- New entries go at the end of the highest-numbered chapter. Entry stamp: `<b>Mon 3312</b>`, GST start and shutdown times, location (`class="loc"`). The newest entry carries `id="latest"`: move it to each new entry, and keep the index's `a.latest` link (chapter and entry title) pointing at it.
- When an entry changes the latest chapter's date range or story, update its label on the chapter page and its date range and blurb in the index list. Also update the index's "Where she stands now" and the header's last-entry month.
- Start a new chapter only when the user asks: copy the latest chapter page to `chronicle/<n+1>.html`, add it to the index list with a blurb, and repoint the previous page's next link and the new page's pager.

## Hangar and gallery

- `hangar.html` has one `article.ship-active` panel per ship (`id` is the lowercased fleet ID, e.g. `ri-x05`). A ship's screenshot sits at the top of its panel as a zoom button; caption `<Model> <NAME> · <ID>`. Modules are a `ul.mods`: engineered ones as `<li><span class="mn">Module</span><span class="me">Engineer · <b class="g">G5</b> Blueprint</span></li>`, stock ones folded into one `li.stock` (`+N stock` over the names). The registry and role-code key sits under the Ships heading.
- Ship screenshots come from `%USERPROFILE%\Pictures\Screenshots\Elite Dangerous\Ships`, named `<Model> <NAME>.jpg` to match the panel heading. Copy them to `gallery/ships/<ship-name>.jpg` (lowercase, hyphens: `hymn-of-the-long-night.jpg`), resized to 1600x900 JPEG, quality about 82. Helios has no screenshot yet.
- `gallery.html` shows the concept sheets from `gallery/concepts/`; the flight-suit turnaround stays on the biography beside the physical record.

## Engineers and awards

- `engineers.html` renders from two arrays in its script: `engs` (`[name, grade 1-5, "Mon YYYY"]`, or `[name, "Invited"|"Known"|"Undiscovered", date]`) and `awards` (`[name, tier, percent, detail]`). Update the counts in the paragraph under each grid to match.

## Flight log

- `log.html` holds its data in its script: `sys`, `LEGS` and the legend get every leg; `SHOW` (10) limits the map, legend and jump list to the latest legs. The jump total counts all legs.
- Sections carry ids (`career`, `path`, `sessions`, `log-events`, `ledger`, `totals`) for the sticky "On this page" index; a new section needs an id and an index link. Order: flight path, sessions, log, ledger, career, totals. Career history is a timeline of `div.era` (`p.when` with the date range and a `<small>` count, then `div.what` holding a `details.efold` with the `h3` and entry count in its summary, then the list; only the latest era is open). The route, session and ledger notes sit in `<details class="notes">` as short paragraphs. Sessions and the event log render from the `sessions` and `log` arrays, folded by day, newest first, with the latest open; the latest-session link under the h1 fills itself from the last session. Charts redraw at their real width on resize, so give them no fixed sizes.
- After editing `log.html`, extract its `<script>` blocks and run them under Node with a DOM stub to confirm they run. Headless Edge returns blank output.

## Checking changes

- Serve the folder locally (`python -m http.server 8790 --bind 127.0.0.1`) and open it in the browser pane. The pane cannot drive `file://` pages.
- Check desktop and phone (375px) widths for sideways scroll (`scrollWidth` against `innerWidth`).
- To check links, crawl from `index.html` with `fetch(url, { cache: "no-store" })`: the pane caches pages, and a cached copy shows stale paths after a move.
