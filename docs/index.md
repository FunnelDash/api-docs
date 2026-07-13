# Dash.fi Internal API Reference

Private OpenAPI reference for Dash.fi's internal-facing REST APIs.

## What this is

This site hosts the OpenAPI reference for Dash.fi's REST APIs, for **internal staff and developers only**. It is private — access is gated to members of the Dash.fi organisation.

Each team publishes its own specs to the Scalar Registry from its own repository; this site composes them into one reference. Specs are **generated from source** and republished automatically when a team promotes to production.

## APIs

### Surface

- **Surface Web API** — the BFF consumed by the Dash.fi web app.
- **Surface Admin API** — administrative/backoffice endpoints.
- **Surface Internal API** — the service-to-service (machine-to-machine) contract, consumed by other Dash.fi services such as Core.

## Downloading the specs

Each API reference page has a built-in **Download OpenAPI Document** button — no workaround needed, and it stays behind the same login as the rest of the site.

## Freshness

Specs reflect the code at the last **production** promotion, not necessarily what is running in prod at this instant (partial deploys can lag). Treat this as the reference for the last promoted build.

## Contact

Questions about a specific API belong with its owning team. This site itself is shared engineering-docs infrastructure — see the repo `README.md` for conventions.
