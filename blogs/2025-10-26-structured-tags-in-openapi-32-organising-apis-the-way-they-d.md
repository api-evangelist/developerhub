---
title: "Structured Tags in OpenAPI 3.2: Organising APIs the Way They Deserve"
url: "https://developerhub.io/blog/structured-tags-openapi-3-2/"
date: "Sun, 26 Oct 2025 21:00:37 GMT"
author: "Zaid Daba'een"
feed_url: "https://developerhub.io/blog/rss/"
---
<img alt="Structured Tags in OpenAPI 3.2: Organising APIs the Way They Deserve" src="https://developerhub.io/blog/content/images/2025/10/openapi3.2-3.png" /><p>One of the most underrated additions in <strong>OpenAPI 3.2</strong> is the new support for <strong>structured, hierarchical tags</strong>.</p><p>It doesn’t sound like much at first — tags have existed since the earliest OpenAPI versions. But this change quietly solves one of the most persistent pain points in large-scale API documentation: structure.</p><h3 id="the-problem-with-flat-tags"><strong>The Problem With Flat Tags</strong></h3><p>Before 3.2, OpenAPI only supported a flat list of tags. Each operation could reference one or more tags, and documentation tools would group endpoints accordingly.</p><p>That was fine for small APIs. But as soon as you had dozens or hundreds of operations, the tag list turned into a scrollable wall of names:</p><!--kg-card-begin: code--><pre><code>users
users-profile
users-notifications
payments
payments-webhooks
analytics
analytics-metrics
analytics-reports</code></pre><!--kg-card-end: code--><p>Developers ended up overloading tag names to imply hierarchy — adding hyphens, prefixes, or categories — and relying on their doc viewer to make sense of it. The spec didn’t help you structure it; you just hoped your conventions held up.</p><h3 id="what-changes-in-3-2"><strong>What Changes in 3.2</strong></h3><p>With <strong>OpenAPI 3.2</strong>, the Tag Object can now be <strong>nested</strong> and <strong>grouped</strong>. Tags can belong to categories, subcategories, or other tags — allowing documentation tools and SDK generators to represent APIs in logical hierarchies.</p><p>An example might look like this:</p><!--kg-card-begin: code--><pre><code class="language-yaml">tags:
  - name: users
    description: Endpoints related to user management
    summary: Users
  - name: profile
    description: Profile operations
    summary: Profile
    parent: users # &lt;--- Makes profile a child of users
  - name notifications
    description: User notifications
    summary: Notifications
    parent: users
    
  - name: payments
    description: Payment processing and billing</code></pre><!--kg-card-end: code--><p>Docs tools can then visualise these relationships as nested menus, collapsible sections, or grouped navigation trees.</p><p>Tags are still used the same way as before:</p><!--kg-card-begin: code--><pre><code class="language-yaml">paths:
  /users/{id}/profile:
    get:
      summary: Get user profile
      tags: [profile]
      responses:
        '200':
          description: Returns a user profile</code></pre><!--kg-card-end: code--><p>View the <a href="https://spec.openapis.org/oas/v3.2.0.html#tag-object">Tag Object</a> full details in OpenAPI 3.2 Specification.</p><h3 id="why-it-matters"><strong>Why It Matters</strong></h3><p>For large organisations, this change brings real clarity.</p><ul><li><strong>Better navigation:</strong> You can now organize endpoints by domain, feature, or business area, not just alphabetically.</li><li><strong>Cleaner docs:</strong> Readers get a structured hierarchy that mirrors how the product is built.</li><li><strong>Smarter SDKs:</strong> Code generators can use tag structure to create namespaces or class groupings automatically.</li></ul><p><strong>Fewer naming hacks:</strong> No more manual prefixing to simulate folders or sections.</p><p>It’s a small spec change with a big downstream effect — especially for teams maintaining large or modular APIs.</p><h3 id="a-small-step-toward-more-maintainable-apis"><strong>A Small Step Toward More Maintainable APIs</strong></h3><p>Structured tags don’t change how your API behaves — they change how it’s <strong>understood</strong>.</p><p>That’s an equally important part of API design.</p><p>This update is about clarity at scale. And for teams that manage complex platforms or multiple product areas, that clarity can make all the difference.</p><h3 id="comparison-with-x-taggroups"><strong>Comparison with x-tagGroups</strong></h3><p>Before OpenAPI 3.2, the only way to organise tags hierarchically was through the <code>x-tagGroups</code> extension, an unofficial convention popularised by tools.</p><p>While it worked well in some ecosystems, it was never part of the OpenAPI standard.</p><p>That meant support was inconsistent: some documentation tools rendered tag groups beautifully, while others ignored them entirely.</p><p>With <strong>structured tags</strong> now built into OpenAPI 3.2, this kind of grouping is finally <strong>standardised and portable</strong>.</p><p>You no longer have to rely on vendor-specific extensions or risk losing structure when switching tools.</p><p>For API designers and documentation teams, that means one thing: <strong>your tag hierarchy will now look the same everywhere.</strong></p>
