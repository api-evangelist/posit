---
name: Publish and deploy content to Posit Connect
description: Create a content item, upload a deployment bundle, deploy it, and wait for the deploy job to finish.
api: openapi/posit-connect-openapi-original.json
operations: [createContent, uploadContentBundle, buildContentBundle, deployContentBundle, getJob]
---

# Publish and deploy content to Posit Connect

Use the Posit Connect Server API (base path `/__api__`) to publish content
(a Shiny app, Quarto/RMarkdown doc, Jupyter notebook, Plumber/FastAPI API,
or static site) programmatically.

## Auth
All requests send an API key in the header:

```
Authorization: Key <YOUR_API_KEY>
```

Keys are per-user; the caller's permissions apply.

## Steps
1. **Create the content placeholder** — `POST /v1/content` (`createContent`).
   Send a `name` (and optional `title`, `access_type`). The response returns the
   content `guid` you use for every following step.
2. **Upload a bundle** — `POST /v1/content/{guid}/bundles` (`uploadContentBundle`).
   The body is a gzipped tar archive containing your app plus a `manifest.json`
   describing dependencies. The response returns a bundle `id`.
   (The CLI `rsconnect write-manifest` generates the manifest for you.)
3. **Deploy the bundle** — `POST /v1/content/{guid}/deploy` (`deployContentBundle`)
   with `{ "bundle_id": "<id>" }`. This starts an async deploy and returns a
   `task_id`/job reference.
4. **Poll the job** — `GET /v1/content/{guid}/jobs/{key}` (`getJob`) until the
   job completes. Surface job status/log lines to the user.

## Conventions & errors
- Resources are addressed by `guid`. See `conventions/posit-conventions.yml`.
- No idempotency key exists — do not blindly retry `deployContentBundle`; check
  the existing job first.
- Errors return `{ code, error, payload }` (not RFC 9457). Handle `401`
  (bad key), `403` (not permitted), `404` (unknown guid), `409` (conflict).
  See `errors/posit-problem-types.yml`.
