# Gruvbox Dynamic

Dynamic Gruvbox-inspired mail themes.

## Status

- `thunderbird/` contains the current working Thunderbird theme.
- `firefox/` is reserved for a future Firefox version.
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

If stylesheet caching gets in the way, changing the stylesheet filename in `manifest.json` has been the most reliable cache buster so far.

## Packaging

Create the XPI from inside `thunderbird/`:

```bash
zip -r gruvbox-dynamic-thunderbird.xpi manifest.json gruvbox-mail-panels.css
```

Install it from Thunderbird:

1. Open `Add-ons Manager`.
2. Open the gear menu.
3. Choose `Install Add-on From File...`.
4. Select the generated `.xpi`.

## Firefox Plan

Planned next steps for `firefox/`:

1. Extract the shared Gruvbox palette into a small documented source file.
2. Create a Firefox theme manifest with matching light and dark variants.
3. Decide whether Firefox should stay manifest-only or become a dynamic extension.
4. Keep Thunderbird-specific thread pane overrides out of the Firefox package.

## Notes

- Thunderbird requires an explicit `browser_specific_settings.gecko.id` for installable XPI themes.
- `strict_min_version` is currently set to the only version tested so far.
