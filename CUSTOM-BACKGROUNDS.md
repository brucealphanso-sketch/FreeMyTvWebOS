# Custom Backgrounds (this fork's additions)

This fork adds a background system on top of upstream Free My TV:

- **Per-app backgrounds.** The moment a tile on the shelf gets focus, the full
  screen crossfades to that app's own background image. Move focus away and
  it crossfades back.
- **Custom OS/home background.** A `default` image shows on first load and
  behind anything that doesn't have its own background set (the Customize
  button, weather widget, trending tabs, or any app you leave blank).
- **Slow zoom (Ken Burns).** Backgrounds gently zoom/pan while shown, togglable.
- **Darkening scrim.** A gradient overlay keeps the shelf, hero text, and
  trending cards readable over any photo, togglable.
- **On-TV toggles.** Customize > section 4 "Backgrounds" lets you turn Custom
  Backgrounds, the zoom effect, and the scrim on/off from the remote, no
  rebuild needed.

## How to set your own images

1. Put your photos in `app/assets/backgrounds/`. Landscape, 1920x1080 or
   larger, JPG/PNG/WebP, ideally under ~2MB each so they load instantly on
   the TV. This fork ships with placeholder gradient images (one per app)
   so everything works out of the box — just overwrite the files you want to
   replace with your own photos, same filenames.
2. Open `app/index.html` and find the `CUSTOM_BACKGROUNDS` block near the
   top of the `<script>` section (search for `CUSTOM BACKGROUNDS`). Each
   line maps an app id to a file path under `assets/backgrounds/`. Add,
   remove, or repoint entries as you like. Leave a value as `''` to use the
   theme gradient instead of a photo for that app.
3. Rebuild and reinstall (see the main README's install steps — this fork
   doesn't change those, only `app/`):
   ```
   ares-package app -o dist --no-minify
   ares-install --device tv dist/org.webos.freemytv_*.ipk
   ```
   or re-run `python scripts/install.py` if you're using the automated
   installer.

## Files changed from upstream

- `app/index.html` — background crossfade engine, `CUSTOM_BACKGROUNDS`
  config block, three new Customize-menu settings (Custom Backgrounds /
  Background Motion / Darkening Scrim), and the two background layers +
  scrim in the page markup/CSS.
- `app/assets/backgrounds/` — new folder, placeholder background images
  (replace with your own).
