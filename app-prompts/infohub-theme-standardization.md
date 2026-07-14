# Prompt: update InfoHub to use mkronvold/themes

Update `mkronvold-wtg/infohub` to use the shared theme standard from `mkronvold/themes`.

## Current app context

InfoHub is a Next.js docs platform with Tailwind CSS:

- Next `16.2.9`
- React `19.2.4`
- TypeScript
- Tailwind CSS 4 via `@tailwindcss/postcss`
- Current theme variables are in `src/app/globals.css`
- Current toggle is `src/components/theme-toggle.tsx`
- Current bootstrap script is in `src/app/layout.tsx`
- Current storage key is `infohub-theme`
- Current supported theme IDs are only `dark` and `light`

## Goal

Replace InfoHub's local two-theme color system with the canonical `--theme-*` CSS variables from `mkronvold/themes/theme.css`. Do not add a separate adapter package or a parallel theme model. Update InfoHub's own CSS variables/selectors so they use the shared token names directly.

## Source of truth

Use these files from `mkronvold/themes`:

- `theme.json` for theme IDs, labels, modes, aliases, and token values.
- `theme.css` for generated CSS variables.

The canonical theme IDs, ordered from brightest to darkest, are:

`sunshower`, `light`, `sepia`, `summer`, `autumn-light`, `spring`, `coyote`, `autumn`, `coyote-dark`, `pine`, `midnight`, `forest`, `guinness`, `ocean`, `obsidian`.

Preserve `dark` as a legacy alias for `ocean` if existing local storage values may still contain it.

## Implementation guidance

1. Import, copy, or vendor `theme.css` from `mkronvold/themes` into InfoHub, preferably near `src/app/globals.css`.
2. Remove the hardcoded dark `:root` and `[data-theme="light"]` color values from `src/app/globals.css`.
3. Replace InfoHub's local variables with canonical values:
   - `--background` -> `--theme-bg`
   - `--foreground` -> `--theme-text`
   - `--muted` -> `--theme-muted-text`
   - `--border` -> `--theme-border`
   - `--panel` -> `--theme-surface`
   - `--panel-strong` -> `--theme-surface`
   - `--panel-soft` -> `--theme-surface-muted`
   - `--accent` -> `--theme-accent`
   - `--accent-strong` -> `--theme-active-border`
   - `--accent-soft` -> `--theme-active-bg`
   - `--highlight` -> `--theme-mark-bg`
4. Where text is rendered on topbar/chrome surfaces, use:
   - `--theme-chrome`
   - `--theme-chrome-text`
   - `--theme-chrome-muted-text`
5. Update `src/components/theme-toggle.tsx` from a two-state light/dark toggle to a canonical theme cycle or selector.
6. Update `src/app/layout.tsx` bootstrap logic:
   - Accept all canonical theme IDs.
   - Migrate stored `dark` to `ocean`.
   - Keep a safe default, preferably `ocean` or the user's system preference mapped to `light`/`ocean`.
7. Preserve InfoHub's existing docs layout, GitHub auth/device login behavior, markdown rendering, admin authoring flows, and status routes.
8. Avoid theme-specific hardcoded color literals in components after migration. Prefer the canonical variables in CSS.

## Validation

Run existing InfoHub checks:

```sh
npm run lint
npm run build
```

Visually verify at least:

- `sunshower`
- `light`
- `summer`
- `autumn-light`
- `ocean`
- `obsidian`

Pay special attention to the sticky topbar, search suggestions, status tabs, markdown body, admin authoring editor, and login/device panels.
