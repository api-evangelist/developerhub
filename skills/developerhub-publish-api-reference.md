---
name: developerhub-publish-api-reference
description: >-
  Upload an OpenAPI specification to a DeveloperHub project from a CI/CD pipeline and publish it as
  a rendered API reference. Use whenever a build needs to keep a DeveloperHub API reference in step
  with the spec in the repository.
api: DeveloperHub.io API
version: 1.1.0
base_url: https://api.developerhub.io/api/v1
auth: X-Api-Key header
operations:
  - add_reference
  - publish_reference
  - read_reference_definition
  - list_versions
generated: '2026-09-06'
method: generated
source: openapi/_original/developerhub-openapi.yml, https://docs.developerhub.io/support-center/api-key
---

# Publish an API reference to DeveloperHub

This is the flow DeveloperHub documents for CI/CD: generate your specification with your own tools,
then push it to the project so the rendered reference never drifts from the build.

## Before you start

- Create a project API key under **Project Settings → API Keys**. Keys are per project and carry
  their own permission set.
- Send it as `X-Api-Key: <api-key>` on every request. There is no OAuth on this API. A missing key
  returns `403`; an invalid key returns `400` with the message `API Key is invalid`.
- The base URL is `https://api.developerhub.io/api/v1`.

## 1. Find the version to publish into

`list_versions` — `GET /version`

Returns every non-deleted version with its `id`, `slug`, `published` flag and `ordr`. Pick the `id`
of the version this build targets. Rate limit: 60 in 60 minutes, so cache it rather than calling it
per job.

## 2. Upload the specification

`add_reference` — `POST /version/{versionId}/reference`

This is the only `multipart/form-data` operation in the API. Send the spec as `file`.

```bash
curl --request POST \
  --url https://api.developerhub.io/api/v1/version/{versionId}/reference \
  --header "X-Api-Key: $DEVELOPERHUB_API_KEY" \
  --form "file=@./openapi.yaml"
```

Query parameters worth setting deliberately:

- `publish` — set `false` to import as a draft and publish in a later, explicit step. Leave it
  unset only when you want the build to go straight to readers.
- `show_try_it_out` — enable the Try It Out console on the reference.
- `allow_download` — let readers download the specification.
- `expandable` — render operations as expandable rows.

**This operation is the one replay-safe write in the API.** It upserts on the reference *title*: if
a reference with the same title already exists it is updated rather than duplicated, so a retried
job does not leave you with two references. Every other write in this API has no replay protection.

Rate limit: 300 in 60 minutes.

## 3. Publish the draft

`publish_reference` — `PUT /reference/{id}/publish`

Only needed if you uploaded with `publish=false`. Rate limit: 300 in 60 minutes.

## 4. Verify what is live

`read_reference_definition` — `GET /reference/{id}/definition`

Downloads the raw specification back, in JSON or YAML. Diff it against the file you pushed to
confirm the build landed. Returns `404` ("Draft not found") when the reference has no draft.
Rate limit: 600 in 1 minute.

## Errors to handle

All errors are `application/json` with a nested envelope
`{"error":{"message":"...","httpCode":N,"code":N}}` — **not** RFC 9457 problem+json.

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 | General exception, or an invalid API key (internal code `0`) | Check the key and the request body |
| 403 | No key, or the key lacks the permission | Supply a key for this project with the right permission |
| 415 | Wrong content type | This operation needs `multipart/form-data`; every other write needs `application/json` |
| 429 | Rate limited (internal code `9`) | Wait until the `X-RateLimit-Reset` unix timestamp — there is no `Retry-After` |

Every successful response carries `X-RateLimit-Limit`, `X-RateLimit-Remaining` and
`X-RateLimit-Reset`. Read them rather than counting requests yourself.
