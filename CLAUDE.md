# Free My TV (webOS) — Bruce's fork

Ad-free custom home screen for LG webOS TVs, packaged as an `.ipk`.
Fork of `danielthejoker18/FreeMyTvWebOS`; this fork's remote is
`brucealphanso-sketch/FreeMyTvWebOS`. Fork-only features (per-app backgrounds,
Ken Burns, scrim) are described in `CUSTOM-BACKGROUNDS.md`.

## Layout

- `app/index.html` — the entire app: HTML, CSS and vanilla JS in one ~2,500-line
  file. No framework, no bundler, no npm. Nearly all work happens here.
- `app/appinfo.json` — webOS manifest. `version` drives the ipk filename.
- `app/assets/backgrounds/` — background images, mapped in the
  `CUSTOM BACKGROUNDS` block of `index.html` (search that string).
  Default shelf apps: `DEFAULT_FAVORITES` in `index.html`.
- `app/webOSTV.js` — LG's official library. Do not edit.
- `scripts/build.py` — packages `app/` into `dist/<id>_<version>_all.ipk`
  (uses `ares-package` if present, else a pure-Python packager; also rewrites
  tar timestamps so webOS 5.x accepts the ipk — keep that step).
- `scripts/install.py` — finds the TV via SSDP, pairs SSH, installs the ipk.
- `docs/` — upstream's GitHub Pages preview. It is NOT kept in sync with `app/`;
  don't update it unless asked.
- `build-local/` — gitignored personal builds that may contain private photos.
  Never commit or publish anything from it.

## Workflow

1. Edit `app/index.html` (or assets).
2. Rebuild: `python scripts/build.py`
3. Commit the source change **and** the rebuilt `dist/*.ipk` together — the
   committed ipk is what users download.
4. Install to the TV: `python scripts/install.py` (TV must be in Developer Mode
   with Key Server on; it prompts for the 6-character passphrase).

Bump `version` in `appinfo.json` only for real releases; the README links to the
ipk by filename, so update those links when the version changes.

## Constraints

- Target is a 1920×1080 TV screen driven by a remote (D-pad + Magic Remote
  pointer). Every interactive element must be reachable and visibly focused
  with arrow keys.
- TVs are low-powered: prefer CSS transforms/opacity for animation, avoid
  heavy filters outside "Standard" performance mode, keep images ≲2 MB.
- Must run on old webOS web engines (webOS 3.0+ ≈ old Chromium): stick to the
  ES5-style JS already used in the file (`var`, plain functions) — no build step
  will transpile newer syntax.
- Escape any text from third-party APIs (TMDB, TVMaze, Open-Meteo) before
  inserting into the DOM.
- Settings persist in `localStorage` under `fmtv_*` keys.

## Testing

No test suite. Preview locally by serving `app/` (e.g.
`python -m http.server 8000 -d app`) and viewing at 1920×1080 — the
`webapp-testing` skill can screenshot it. Luna calls (app launching, installed
app list) only work on a real TV; the browser preview shows a notice instead.
