# Gruvbox Dynamic

Dynamic Gruvbox-inspired Thunderbird, Firefox, and iTerm2 themes.

## Status

- `thunderbird/` contains the current working Thunderbird theme.
- `firefox/` contains a Firefox theme manifest with matching light and dark variants.
- `iterm2/` contains an iTerm2 color preset with matching light and dark variants.
- `research/` contains local reference material and is ignored by git.

## Thunderbird

The Thunderbird theme uses a static theme manifest together with a `theme_experiment` stylesheet to reach parts of the UI that are not covered by normal theme keys.

Files:

- `thunderbird/manifest.json`
- `thunderbird/gruvbox-mail-panels.css`

Current add-on id:

`gruvbox-dynamic.thunderbird@rodstrom.se`

Minimum tested Thunderbird version:

`150.0`

No `strict_max_version` is set.

## Local Development

1. Open Thunderbird.
2. Go to `Tools` -> `Developer Tools` -> `Debug Add-ons`.
3. Use `Load Temporary Add-on...`.
4. Select `thunderbird/manifest.json`.

For normal iteration, bump the patch version in `thunderbird/manifest.json` for each test build. That has been enough to break Thunderbird's cache during development.

If Thunderbird starts caching the stylesheet again, there is also a fallback debug workflow in `thunderbird/debug/` using UUID-named CSS files and a debug-only manifest.

## Packaging

Create the XPI from inside `thunderbird/`:

```bash
zip -r "output/gruvbox-dynamic-thunderbird-<version>.xpi" manifest.json gruvbox-mail-panels.css
```

Install it from Thunderbird:

1. Open `Add-ons Manager`.
2. Open the gear menu.
3. Choose `Install Add-on From File...`.
4. Select the generated `.xpi`.

## Firefox

The Firefox theme is currently manifest-only and does not use custom CSS.

Files:

- `firefox/manifest.json`

Current add-on id:

`gruvbox-dynamic.firefox@rodstrom.se`

Minimum tested Firefox version:

`150.0`

No `strict_max_version` is set.

## Firefox Local Development

1. Open Firefox.
2. Go to `about:debugging`.
3. Open `This Firefox`.
4. Use `Load Temporary Add-on...`.
5. Select `firefox/manifest.json`.

For normal iteration, bump the patch version in `firefox/manifest.json` for each test build.

Firefox does not currently need a custom stylesheet. Keep the Firefox package manifest-only unless there is a concrete UI gap that cannot be handled with theme color keys.

## Firefox Packaging

Create the XPI from inside `firefox/`:

```bash
zip -r "output/gruvbox-dynamic-firefox-<version>.xpi" manifest.json
```

Expected contents:

 - `manifest.json`

Keep Thunderbird-specific CSS and `theme_experiment` settings out of the Firefox package.

## iTerm2

The iTerm2 theme is a single `.itermcolors` preset that contains both light and dark color variants.

Files:

- `iterm2/gruvbox-dynamic.itermcolors`

## iTerm2 Local Development

1. Import `iterm2/gruvbox-dynamic.itermcolors` in iTerm2.
2. Apply the preset to a profile.
3. Toggle macOS appearance to verify both light and dark variants.

Lint the preset after edits:

```bash
plutil -lint "iterm2/gruvbox-dynamic.itermcolors"
```

## Notes

- Thunderbird requires an explicit `browser_specific_settings.gecko.id` for installable XPI themes.
- Firefox currently uses only manifest theme keys and no CSS.
- iTerm2 supports importing both light and dark variants from a single `.itermcolors` file.
- `strict_min_version` is currently set to the only version tested so far.
- `thunderbird/debug/` and `thunderbird/output/` are generated workflow folders and are ignored by git.
