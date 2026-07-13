# api-docs

Shared **internal** API-documentation site for Dash.fi engineering. Renders every team's OpenAPI specs — plus shared developer docs — as one unified [Scalar](https://scalar.com) Docs site, gated to Dash.fi staff. Surface is the first tenant; other teams (e.g. Core) plug in by publishing to the shared registry.

## How it works

This repo is the **Scalar Docs project** for the unified site. It does **not** hold the OpenAPI specs. Instead:

1. Each team's CI generates its specs and publishes them to the shared **Scalar Registry** under the `dash-fi` namespace, with a team-prefixed slug (e.g. `surface-web-api`).
2. `scalar.config.json` in this repo references those registry entries by `namespace` + `slug` and composes them into one navigation, grouped by team.
3. When a referenced spec is republished, **Scalar redeploys this site automatically** — no push to this repo is needed for a spec change.

```
surface/ CI ─┐
core/    CI ─┼─ scalar registry publish → Registry (namespace: dash-fi)
  …          ┘                                   │ (auto-redeploy on change)
                                                 ▼
api-docs/ (this repo — Scalar Docs project) ── Git sync on merge ──► gated Scalar site
```

## Layout

```
api-docs/
├── scalar.config.json   # the whole site: nav (grouped by team), theme, logo, registry refs
└── docs/                # shared Markdown/MDX developer docs
    └── index.md         # landing page
```

No `*.json` specs live here anymore — they come from the registry.

## Scalar setup

- **Team / namespace:** a single `Dash.fi` team (one billing unit) owning the `dash-fi` namespace. All teams publish here; per-team separation is by slug prefix, not by namespace.
- **Docs project:** connected to `FunnelDash/api-docs` via Git sync; `scalar.config.json` at the repo root. Deploys on merge to `main`.
- **Domain:** `internal-api-docs.dash.fi`.
- **Access:** the Docs project is **Private**, restricted to an Access Group for the `@dash.fi` email domain (no SSO required on the Pro plan).
- **Downloads:** each API page exposes a native **Download OpenAPI Document** button, gated behind the same login.

## Publishing specs (per team)

Each team publishes from its **own** repo's CI. Surface's publisher is `.github/scripts/publish-api-specs.sh` in `dashfi/surface` (invoked by the `publish-api-docs` workflow on prod promotion), which runs:

```bash
scalar auth login --token "$SCALAR_TOKEN"
scalar registry publish <spec>.json \
  --namespace dash-fi \
  --slug surface-<web-api|admin-api|internal-api> \
  --version <info.version> --private --force
```

`SCALAR_TOKEN` is a Scalar CI token for the `dashfi` team (repo secret). Specs are **registry-owned artefacts** — do not commit them here; change the source API and let the pipeline republish.

## Onboarding a new team (e.g. Core)

1. In the team's repo, add a CI step running `scalar registry publish --namespace dash-fi --slug core-<api> …` with a `SCALAR_TOKEN` for the `Dash.fi` team.
2. Open a PR to this repo adding a **Core** nav group in `scalar.config.json` that references the `core-*` slugs.

One registry, one bill, one site.

## Local preview

```bash
npx @scalar/cli project check-config scalar.config.json   # validate config
npx @scalar/cli project preview scalar.config.json        # local preview (registry refs need auth)
```

To preview against local spec files instead of the registry, point a route's `filepath` at a local `.json` rather than `namespace`/`slug`.
