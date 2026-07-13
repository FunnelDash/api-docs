# Documentation project instructions

## About this project

- This is the unified internal API documentation site for Dash.fi engineering, built on [Scalar](https://scalar.com) Docs.
- Site configuration lives in `scalar.config.json` (repo root). Validate with `npx @scalar/cli project check-config`.
- Prose/dev docs are Markdown/MDX under `docs/`.
- OpenAPI specs are **not** stored here. Each team publishes its specs to the shared Scalar Registry (`@dash-fi` namespace) from its own repo's CI; `scalar.config.json` references them by `namespace` + `slug`. See `README.md`.
- The site is deployed via Scalar GitHub Sync on merge to `main`, and gated (Private Docs, `@dash.fi` access group).

## Making changes

- **Change a spec** → change the source API in the owning team's repo; its CI republishes to the registry and Scalar auto-redeploys. Never add spec JSON to this repo.
- **Change docs/nav/landing** → edit `docs/` or `scalar.config.json` here and open a PR; merge to `main` deploys.
- **Onboard a team** → they publish `<team>-*` slugs to the `@dash-fi` registry, then a PR adds a nav group here referencing those slugs.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
