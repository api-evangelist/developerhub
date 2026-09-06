---
name: developerhub-publish-documentation-page
description: >-
  Create, edit and publish a page in a DeveloperHub documentation section through the REST API, using
  the draft/publish model so nothing reaches readers before it is meant to. Use when writing or
  updating DeveloperHub docs pages programmatically.
api: DeveloperHub.io API
version: 1.1.0
base_url: https://api.developerhub.io/api/v1
auth: X-Api-Key header
operations:
  - list_versions
  - list_documentation
  - create_page
  - update_page
  - publish_page
  - get_page
  - read_page
  - delete_page
generated: '2026-09-06'
method: generated
source: openapi/_original/developerhub-openapi.yml, https://docs.developerhub.io/support-center/editor-mcp-server
---

# Write a DeveloperHub page through the API

## Before you start

- Page bodies are **Markdoc in DeveloperHub's own dialect**, which is not interchangeable with
  generic Markdoc. Do not write one from general knowledge: install the provider's own
  `write-markdoc` skill (`npx skills add developerhub-io/dh-skills`) or read
  `skills/developerhub-write-markdoc.md` in this repository first. A non-canonical shape saves, then
  silently drops content on the next round-trip.
- Authenticate with `X-Api-Key: <api-key>` against `https://api.developerhub.io/api/v1`.

## 1. Locate the section

`list_versions` — `GET /version`, then `list_documentation` — `GET /version/{id}/documentation`.

You need the documentation section's `id` to create a page in it. Both are limited to 60 calls in 60
minutes, so resolve them once per run.

## 2. Create the page

`create_page` — `POST /documentation/{id}/page`

The page **starts unpublished**, so a create is safe: readers do not see it until you publish. The
body accepts a title, and optionally a parent page (pages nest) and a `categoryTitle`. Rate limit:
10,800 in 1 hour.

There is no idempotency key on this API. A retried `create_page` produces a **second page** — record
the returned `id` before you retry anything.

## 3. Write the body

`update_page` — `PUT /page/{id}` with `title`, `slug`, `content` and `message`.

Two different things happen in this one call, and the difference matters:

- **`content` is written to the page's draft.** Readers keep seeing the published version until you
  publish. This is your rehearsal.
- **`title` and `slug` are NOT drafted.** They take effect immediately, on published pages included.
  Changing a slug changes the page's URL — check what links to it first, and run
  `get_version_broken_links` afterwards.

Rate limit: 10,800 in 1 hour.

## 4. Publish

`publish_page` — `PUT /page/{id}/publish`. Publishing is always a separate, explicit step. Rate
limit: 10,800 in 1 hour.

## 5. Read it back

- `read_page` — `GET /page/{id}` with a `format` parameter, when you have the id. Returns
  `contentDraft` and `contentPublished` side by side, either of which can be null. Rate limit: 600
  in 1 minute.
- `get_page` — `GET /page` when you only have slugs. Requires `page_slug` plus either `version_id`
  or `version_slug`, and either `documentation_id` or `documentation_slug`. Rate limit: 60 in 1
  minute — noticeably tighter than the id-based read, so prefer `read_page` in a loop.

## Deleting

`delete_page` — `DELETE /page/{id}`.

**This cannot be undone.** There is no restore window on the API, no trash, and no confirmation step
at the REST layer (the Editor MCP tool asks for the page's slug back as a guard; the REST operation
does not). Read the page first and keep the body if you may need it. The only 30-day restore
DeveloperHub publishes belongs to GitHub Sync, not to this API.

## Errors

Nested vendor envelope, not problem+json: `{"error":{"message":"...","httpCode":N,"code":N}}`.
`403` no/insufficient key, `400` bad request or invalid key, `415` send `application/json`,
`429` rate limited (internal code `9`) — back off to `X-RateLimit-Reset`, there is no `Retry-After`.
