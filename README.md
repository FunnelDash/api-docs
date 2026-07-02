# api-docs

Shared **internal** API-documentation site for Dash.fi engineering. Hosts each team's OpenAPI specs behind Mintlify Auth (org members only). Surface is the first tenant; other teams (e.g. Core) plug in under their own subdirectory.

## Layout

```
api-docs/
├── internal/                 # ← the Mintlify project watches THIS subdirectory
│   ├── docs.json             # site config: nav groups (one per spec), theme, playground
│   ├── index.mdx             # internal landing page
│   ├── logo/ , favicon.svg   # branding
│   └── <team>/               # one directory per owning team
│       └── *.json            # OpenAPI specs (CI-generated — see below)
└── public/                   # reserved for a future public docs project (not in use)
```

Current tenants:

- `internal/surface/` — `web-api.json`, `admin-api.json`, `internal-api.json`, pushed by Surface CI.

## Conventions

- **Watched branch:** `main` (unprotected). A push to `main` triggers a Mintlify redeploy.
- **Mintlify project:** a single project in the Dash.fi Mintlify org, visibility **Private → Authenticated**, watching subdirectory `internal/`.
- **Domain:** `internal-api-docs.dash.fi`.
- **Access = Mintlify org membership.** On the current (Starter) plan there is no read-only role, so every enrolled member can also edit other projects in the org, including `help.dash.fi`. Enrol deliberately and **remove membership on offboarding**.

## Generated specs — do not hand-edit

Files under `internal/<team>/*.json` are **CI-owned artefacts**. Each team's CD pipeline regenerates and force-pushes them on production promotion. Any manual edit is lost on the next publish. To change a spec, change the source API and let the pipeline republish.

For Surface, the publisher is the `publish-api-docs` job in `dashfi/surface`'s `.github/workflows/cd.yml`, which pushes with a fine-grained PAT (`API_DOCS_GITHUB_REPO_TOKEN`, `contents:write` on this repo only).

## Local preview

```bash
cd internal
npx mint@latest dev        # http://localhost:3000
npx mint@latest validate   # CI-style validation
```
