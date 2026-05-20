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

Normal workflow:

1. Edit `thunderbird/gruvbox-mail-panels.css`.
2. Bump the patch version in `thunderbird/manifest.json` for each test iteration.
3. Reload the theme or package a new XPI if needed.

Reason:

- Thunderbird cache invalidation has proven reliable when the manifest version changes.
- Use patch version bumps for test iterations.

Fallback workflow if Thunderbird starts caching the stylesheet again:

1. Edit `thunderbird/gruvbox-mail-panels.css`.
2. Generate a fresh UUID filename.
3. Copy the source CSS to `thunderbird/debug/<uuid>.css`.
4. Copy `thunderbird/manifest.json` to `thunderbird/debug/manifest.json`.
5. Update `thunderbird/debug/manifest.json` so `theme_experiment.stylesheet` points to the new UUID CSS file.
6. Reload the Thunderbird debug add-on from `thunderbird/debug/manifest.json`.

## UUID CSS Files

UUID-named CSS files in `thunderbird/debug/` are generated cache-busting artifacts.

- Keep `gruvbox-mail-panels.css` as the source of truth.
- Treat UUID CSS files as generated working files.
- Keep the tracked release manifest pointing to `gruvbox-mail-panels.css`.
- Keep the debug manifest pointing to the active UUID CSS file.
- `thunderbird/debug/` is a fallback path and should be ignored by git.

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

- `0.0.3`

Use semantic versioning with these rules:

- patch: increment for every test iteration and cache-busting package
- minor: increment for commit-worthy milestones
- major: increment only for deliberately large changes

Examples:

- test iterations: `0.0.3`, `0.0.4`, `0.0.5`
- commit-worthy milestones: `0.1.0`, `0.2.0`
- major changes: `1.0.0`

Rules:

- Update `manifest.json` when testing so Thunderbird sees a new version.
- Package filenames should match the manifest version unless there is a specific reason not to.
- RC suffixes are optional and only needed when you deliberately want RC-style naming.
