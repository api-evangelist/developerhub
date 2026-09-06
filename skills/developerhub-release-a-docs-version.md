---
name: developerhub-release-a-docs-version
description: >-
  Cut a new documentation version in DeveloperHub — clone the current one, check it for broken links,
  review what changed, then publish it — using the REST API. Use when shipping a product release that
  needs its own docs version.
api: DeveloperHub.io API
version: 1.1.0
base_url: https://api.developerhub.io/api/v1
auth: X-Api-Key header
operations:
  - list_versions
  - clone_version
  - get_version_broken_links
  - get_version_report
  - update_version
generated: '2026-09-06'
method: generated
source: openapi/_original/developerhub-openapi.yml
---

# Cut a new docs version

## 1. Find the version to branch from

`list_versions` — `GET /version`. Returns `id`, `name`, `slug`, `published` and `ordr` for every
non-deleted version. Rate limit: 60 in 60 minutes.

## 2. Clone it

`clone_version` — `POST /version/{id}/clone`

Copies the version **with its documentation sections and its API references** into a new one. The
clone arrives unpublished — the operation description is explicit that it is created as the first
version in the project and must be published with a follow-up `update_version`. Rate limit: 60 in 60
minutes, which is the real constraint on how often you can rehearse this.

Not idempotent: a retried clone produces a second copy. Capture the new `id` from the response.

## 3. Check it before anyone sees it

`get_version_broken_links` — `GET /version/{id}/broken-links`

Returns every broken link with a `severity` (`error` for links that will not resolve, otherwise a
warning), a machine-readable `issue` code, a human-readable `message`, and an `isDraft` flag telling
you whether the offending link is in a page's unpublished draft. Gate the release on
`severity == "error"`. Rate limit: 60 in 60 minutes.

`get_version_report` — `GET /version/{id}/report`

Reports every page in the version with `hasDraft`, `listed`, and who created and last updated it
(`createdBy`, `updatedBy`, `sourceCreatedBy`). Use `hasDraft` to find pages with unpublished changes
you are about to ship — or about to strand. Rate limit: 60 in 60 minutes.

## 4. Publish

`update_version` — `PUT /version/{id}` with `{"published": true}`. The same call also sets `name`
and `slug`. Rate limit: 3,600 in 1 hour.

**This is one of only two reversible writes in the API**: setting `published` back to `false`
unpublishes the version. There is no stated window — it works whenever you call it.

## Rate-limit budget for the whole flow

Every operation in this flow except `update_version` shares the same 60-in-60-minutes shape, so a
release rehearsal costs roughly four of those sixty calls per attempt. Read
`X-RateLimit-Remaining` from each response rather than counting; on `429` (internal code `9`) wait
until the `X-RateLimit-Reset` unix timestamp — no `Retry-After` is returned.
