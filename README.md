# DeveloperHub

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

DeveloperHub is a hosted developer documentation platform that enables teams to create beautiful API references, user guides, and knowledge bases. It features auto-generated API documentation from OpenAPI specifications, built-in versioning, full-text search, custom domains, and a WYSIWYG editor. DeveloperHub provides a complete developer portal solution with support for multiple documentation projects, custom branding, and SEO optimization. The platform is itself programmable: a published OpenAPI 3.2 contract covers 25 operations for pages, versions, documentation sections, API references, reader access, project export and changelog posts, and two first-party remote MCP servers expose the same surface to agents — a read-only reader server on every customer's docs site and an OAuth-protected editor server.

**Areas:** API Reference · Developer Portal · Documentation · Knowledge Base · OpenAPI · Docs as Code · MCP · Agent Skills

## DeveloperHub on APIs.io

Everything profiled for DeveloperHub is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/developerhub/](https://apis.io/providers/developerhub/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [DeveloperHub Documentation Platform](https://apis.io/apis/developerhub/platform/) — DeveloperHub provides a hosted platform for creating developer documentation including API references auto-generated from OpenAPI specs, user guides with a…
- [DeveloperHub Documentation API](https://apis.io/apis/developerhub/developerhub-documentation-api/) — Lists the documentation sections inside a version of a DeveloperHub project — 1 operation.
- [DeveloperHub Project API](https://apis.io/apis/developerhub/developerhub-project-api/) — Project-level operations — export, search, whole-project dump, audit log and users. Export and audit are enterprise-plan only — 5 operations.
- [DeveloperHub Reader Access API](https://apis.io/apis/developerhub/developerhub-reader-access-api/) — Grants, lists and revokes access for invited readers of a private documentation project, keyed on email address — 3 operations.
- [DeveloperHub Changelog API](https://apis.io/apis/developerhub/developerhub-changelog-api/) — Creates and lists changelog posts from Markdoc content, with cursor pagination and named API-key permissions (changelog.edit, changelog.read) — 2 operations.
- [DeveloperHub MCP Servers](https://apis.io/apis/developerhub/developerhub-mcp/) — Two first-party remote MCP servers over Streamable HTTP. The reader server runs at the /mcp route of every customer docs site and exposes one read-only…
- [DeveloperHub Page API](https://apis.io/apis/developerhub/developerhub-page-api/) — Operations for creating, reading, updating, publishing, and deleting pages.
- [DeveloperHub Reference API](https://apis.io/apis/developerhub/developerhub-reference-api/) — Operations for adding, reading, and publishing API reference specifications.
- [DeveloperHub Version API](https://apis.io/apis/developerhub/developerhub-version-api/) — Operations for listing, updating, cloning, and reporting on project versions.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [developerhub-mcp.yml](https://apis.io/servers/developerhub/developerhub-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Developerhub Agentic Access](https://apis.io/agentic-access/developerhub/developerhub-agentic-access/)

#### Collections (7)

Postman and Open Collection request collections.

- [DeveloperHub.io API](https://apis.io/collections/developerhub/open-developerhub/)
- [DeveloperHub.io Documentation API](https://apis.io/collections/developerhub/open-developerhub-documentation-api/)
- [DeveloperHub.io Documentation Pages API](https://apis.io/collections/developerhub/open-developerhub-pages-api/)
- [DeveloperHub.io Documentation Project API](https://apis.io/collections/developerhub/open-developerhub-project-api/)
- [DeveloperHub.io Documentation Reader Access API](https://apis.io/collections/developerhub/open-developerhub-reader-access-api/)
- [DeveloperHub.io Documentation References API](https://apis.io/collections/developerhub/open-developerhub-references-api/)
- [DeveloperHub.io Documentation Versions API](https://apis.io/collections/developerhub/open-developerhub-versions-api/)

#### Overlays (7)

OpenAPI overlays, including conversational phrasing for each operation.

- [Developerhub Changelog Api Overlay](https://apis.io/overlays/developerhub/developerhub-changelog-api-overlay/)
- [Developerhub Documentation Api Overlay](https://apis.io/overlays/developerhub/developerhub-documentation-api-overlay/)
- [Developerhub Pages Api Overlay](https://apis.io/overlays/developerhub/developerhub-pages-api-overlay/)
- [Developerhub Project Api Overlay](https://apis.io/overlays/developerhub/developerhub-project-api-overlay/)
- [Developerhub Reader Access Api Overlay](https://apis.io/overlays/developerhub/developerhub-reader-access-api-overlay/)
- [Developerhub References Api Overlay](https://apis.io/overlays/developerhub/developerhub-references-api-overlay/)
- [Developerhub Versions Api Overlay](https://apis.io/overlays/developerhub/developerhub-versions-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Developerhub Authentication](https://apis.io/security/developerhub/developerhub-authentication/)
- [Developerhub Domain Security](https://apis.io/security/developerhub/developerhub-domain-security/)
- [Developerhub Vulnerability Disclosure](https://apis.io/security/developerhub/developerhub-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Developerhub Scopes](https://apis.io/scopes/developerhub/developerhub-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Developerhub Plans Pricing](https://apis.io/plans/developerhub/developerhub-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Developerhub Rate Limits](https://apis.io/rate-limits/developerhub/developerhub-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Developerhub Finops](https://apis.io/finops/developerhub/developerhub-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://developerhub.io)
- [Documentation](https://docs.developerhub.io)
- [Getting started](https://docs.developerhub.io/support-center/getting-started)
- [API reference](https://docs.developerhub.io/api/ref)
- [Pricing](https://developerhub.io/pricing)
- [Sign up](https://app.developerhub.io/signup)
- [Support](https://docs.developerhub.io/support-center/contact-us)
- [Blog](https://developerhub.io/blog)
- [Terms of service](https://developerhub.io/terms)
- [Privacy policy](https://developerhub.io/privacy)
- [GitHub](https://github.com/developerhub-io)
- [Status](https://status.developerhub.io)

## API Evangelist rating

**Kin Score 56.0** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 51.0 |
| Contract governance | 18.2 |
| Operational transparency | 68.4 |
| Developer ergonomics | 32.6 |
| Access clarity | 69.7 |

**Agent readiness 60.5** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | unknown |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 40.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/developerhub/](https://apis.io/providers/developerhub/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for DeveloperHub: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/developerhub/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
