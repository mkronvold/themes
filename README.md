# mkronvold/themes

Shared theme tokens for small web apps.

- `theme.json` is the source of truth.
- `theme.css` is generated from `theme.json`.
- Apps can consume either file, but `theme.css` is the recommended default for CSS-variable based apps.

## Commands

```sh
npm run build
npm run check
```

`npm run check` validates `theme.json` and fails if `theme.css` is stale.
