# Rislyss

Character site for CMDR Rislyss (Sarah Waters), an Elite Dangerous commander. Static HTML, served by GitHub Pages at https://praclarush.github.io/rislyss/ from `main`.

## Layout

| Path | Contents |
|---|---|
| `index.html` | Biography: Pilots Federation personnel file |
| `chronicle.html` | Chronicle index: prologue, where she stands now, and the chapter list |
| `chronicle/<n>.html` | One page per chronicle chapter, numbered from 1 |
| `log.html` | Flight log built from the ship's journals |
| `hangar.html` | One panel per ship with its screenshot |
| `gallery.html` | Concept-sheet lineup with lightbox zoom |
| `medical.html`, `psychological.html` | Pilots Federation medical and psychological records |
| `gallery/ships/` | Hangar screenshots, named after the ship (`hymn-of-the-long-night.jpg`), 1600×900 |
| `gallery/concepts/` | Character concept sheets |
| `gallery/profile/` | Portrait and cockpit video |

Every page is self-contained: styles and scripts are inline, and only the fonts load from Google Fonts. `.nojekyll` turns off Jekyll processing.

## Publishing

Commit and push `main`; Pages rebuilds in about a minute.
