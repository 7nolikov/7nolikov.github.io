# Project layout overrides

These files override templates from `hugo-theme-til v0.6.0` (the theme is unmaintained
since Dec 2024 and is incompatible with Hugo >= 0.154 out of the box). They are the
minimal forks needed to build on current Hugo. Re-evaluate / delete them if the theme
is updated or replaced.

- `partials/head.html` — `site.Author` was removed in Hugo >= 0.155; uses
  `site.Params.author.name` instead (the site sets `[params.author]` in `hugo.toml`).
- `404.html` — same `site.Author` removal; uses `site.Params.author.email`.
- `partials/svg/*.html` — every theme svg partial does `{{ .class | default "…" }}`,
  but the theme's render hooks call them with a non-map context. Hugo >= 0.154 errors on
  `.class` of a string, and under concurrent rendering (the theme's
  `functions/linkable-pages.html` force-renders every page's `.Content`) that error
  manifests as a silent build hang. Each file guards the access with
  `(and (reflect.IsMap .) .class)`.

Verified: with these overrides, Hugo 0.162.1 builds the full site (116 output files),
matching the previous Hugo 0.137.1 output except for content-fingerprint hashes.
