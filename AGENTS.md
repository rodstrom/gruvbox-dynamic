# AGENTS.md

## Thunderbird Theme Workflow

Canonical Thunderbird CSS source file:

- `thunderbird/gruvbox-mail-panels.css`

Tracked release manifest:

- `thunderbird/manifest.json`

Debug-only generated files live in:

- `thunderbird/debug/`

Packaged artifacts live in:

- `thunderbird/output/`

Do not edit the active UUID-named CSS file directly unless there is a very specific reason.
The normal workflow is:

1. Edit `thunderbird/gruvbox-mail-panels.css`.
2. Generate a fresh UUID filename.
3. Copy the source CSS to `thunderbird/debug/<uuid>.css`.
4. Copy `thunderbird/manifest.json` to `thunderbird/debug/manifest.json`.
5. Update `thunderbird/debug/manifest.json` so `theme_experiment.stylesheet` points to the new UUID CSS file.
6. Reload the Thunderbird debug add-on from `thunderbird/debug/manifest.json`.

Reason:

- Thunderbird has aggressively cached the `theme_experiment` stylesheet during development.
- Changing the CSS filename is the most reliable cache-busting method found so far.

## UUID CSS Files

UUID-named CSS files in `thunderbird/debug/` are generated cache-busting artifacts.

- Keep `gruvbox-mail-panels.css` as the source of truth.
- Treat UUID CSS files as generated working files.
- Keep the tracked release manifest pointing to `gruvbox-mail-panels.css`.
- Keep the debug manifest pointing to the active UUID CSS file.
- `thunderbird/debug/` should be ignored by git.

## Packaging

Package only when explicitly useful for testing or release work. Debug reloads are preferred during iteration.

Package from inside `thunderbird/`:

```bash
zip -r "output/gruvbox-dynamic-thunderbird-<version>.xpi" manifest.json gruvbox-mail-panels.css
```

Example:

```bash
zip -r "output/gruvbox-dynamic-thunderbird-0.0.2-rc7.xpi" manifest.json gruvbox-mail-panels.css
```

After packaging, verify contents:

```bash
unzip -l "output/gruvbox-dynamic-thunderbird-<version>.xpi"
```

Expected contents:

- `manifest.json`
- `gruvbox-mail-panels.css`

## Versioning

Manifest version currently in use:

- `0.0.2`

Use these suffix rules for package filenames:

- stable/package candidate: `0.0.2`
- release candidates: `0.0.2-rc1`, `0.0.2-rc2`, `0.0.2-rc3`, ...

Rules:

- Do not add `-rcN` to `manifest.json` unless there is a specific reason to test Thunderbird manifest-version caching.
- Normal RC iteration uses the same manifest version and changes only the XPI filename suffix.
- When making a real version bump, update `manifest.json` first, then name the XPI to match that version.
