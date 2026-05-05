---
title: "The New QUERY Method in OpenAPI 3.2: A Small Addition That Clears Up a Big Ambiguity"
url: "https://developerhub.io/blog/the-new-query-method-in-openapi-3-2-a-small-addition-that-clears-up-a-big-ambiguity/"
date: "Mon, 27 Oct 2025 09:15:46 GMT"
author: "Zaid Daba'een"
feed_url: "https://developerhub.io/blog/rss/"
---
<!--kg-card-begin: html--><div style="background-color: #effbff; border: 2px solid #c6f1ff; border-radius: 6px; padding: 8px 16px; width: 100%; margin-bottom: 24px;">🎉 Now supported in DeveloperHub</div><!--kg-card-end: html--><img alt="The New QUERY Method in OpenAPI 3.2: A Small Addition That Clears Up a Big Ambiguity" src="https://developerhub.io/blog/content/images/2025/10/openapi3.2-5.png" /><p>One of the quieter updates in <strong>OpenAPI 3.2</strong> is support for a new HTTP method: QUERY.</p><p>It’s small, but it clarifies something that’s been fuzzy in API design for years — how to represent read-only, query-style operations that aren’t strictly GET.</p><h3 id="why-query-exists"><strong>Why QUERY Exists</strong></h3><p>Traditionally, REST APIs have used GET for anything that retrieves data. But GET comes with a few assumptions baked in by the HTTP spec:</p><ul><li>Requests must be <strong>idempotent</strong> (no side effects).</li><li>Requests can’t have a <strong>body</strong>.</li><li>Parameters have to fit into the <strong>URL query string</strong>.</li></ul><p>That last point has caused trouble.</p><p>Some APIs need to send complex filters, nested objects, or large request payloads — all while keeping the operation read-only.</p><p>GET doesn’t allow that.</p><p>POST does, but using POST for read-only queries violates the spirit of HTTP semantics and breaks cacheability.</p><p>The new QUERY method is meant to fix that gap.</p><h3 id="what-query-does"><strong>What QUERY Does</strong></h3><p>QUERY is a <strong>read-only method that allows a request body</strong>.</p><p>That means you can write operations like:</p><!--kg-card-begin: code--><pre><code class="language-yaml">paths:
  /search:
    query:
      summary: Search for users
      description: Performs a read-only query with a complex filter
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                filters:
                  type: object
                  properties:
                    country:
                      type: string
                    active:
                      type: boolean
      responses:
        '200':
          description: Search results
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'</code></pre><!--kg-card-end: code--><p>This example describes a <strong>search endpoint</strong> that can take a JSON body instead of a long, hard-to-parse URL query string — while remaining strictly read-only.</p><h3 id="why-it-matters"><strong>Why It Matters</strong></h3><p>For API consumers, this makes specs and SDKs more predictable.</p><p>For API authors, it allows <strong>expressive query operations</strong> without semantic compromises.</p><p>Here’s what changes in practice:</p><ul><li><strong>Cleaner API design:</strong> You no longer need to overload POST just because your query has a JSON payload.</li><li><strong>Better caching:</strong> Since QUERY is explicitly read-only, it’s easier to implement caching semantics safely.</li><li><strong>Improved documentation:</strong> Docs and SDKs can clearly label query endpoints as non-mutating, even with request bodies.</li></ul><h3 id="tooling-and-adoption"><strong>Tooling and Adoption</strong></h3><p>This is where things get interesting.</p><p>While OpenAPI 3.2 now recognises QUERY, many <strong>web servers, frameworks, and HTTP libraries</strong> don’t — at least not yet. The method is new to the specification world, but not yet part of the <strong>core HTTP vocabulary</strong> most runtimes expect.</p><p>That means you can document QUERY operations today, but whether they’ll <em>run</em> as-is depends on your stack:</p><ul><li>Many frameworks will reject unknown methods until they’re explicitly supported.</li><li>API gateways and reverse proxies (like NGINX or Cloudflare) might block them unless configured to pass them through.</li><li>SDK generators and clients will need to update their request builders to handle QUERY gracefully.</li></ul><p>So in practice, QUERY is more of a <strong>forward-looking signal</strong> than a production-ready feature right now.</p><p>Teams can start by using it in their <strong>OpenAPI specs</strong> to clarify intent — even if they continue serving those endpoints via POST temporarily. Over time, as web servers and tooling catch up, these definitions will become executable, not just descriptive.</p><h3 id="a-step-toward-clarity"><strong>A Step Toward Clarity</strong></h3><p>The addition of QUERY isn’t about introducing something new — it’s about naming what developers have already been doing.</p><p>Many APIs have had “read-only POST” endpoints for years. Now, with OpenAPI 3.2, there’s a consistent, standards-based way to describe them.</p><p>It’s another small example of how OpenAPI evolves thoughtfully: by turning common patterns into first-class citizens.</p>
