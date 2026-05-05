---
title: "Event streaming in OpenAPI 3.2: What changed and why it matters"
url: "https://developerhub.io/blog/event-streaming-in-openapi-3-2-what-changed-and-why-it-matters/"
date: "Mon, 06 Oct 2025 08:47:55 GMT"
author: "Zaid Daba'een"
feed_url: "https://developerhub.io/blog/rss/"
---
<img alt="Event streaming in OpenAPI 3.2: What changed and why it matters" src="https://developerhub.io/blog/content/images/2025/10/openapi3.2-4.png" /><p>OpenAPI 3.2 brings native, first‑class ways to describe APIs that send data as a sequence of events instead of a single, monolithic payload. This is a big deal for real‑time apps—LLMs, analytics feeds, chat, logs, and anything that benefits from progressive rendering or lower perceived latency. At a high level, OpenAPI 3.2 formalizes “sequential” media types and lets you specify the schema of each item in the stream using a new itemSchema on a response media type. That means your documentation can be explicit about the shape of each event, not just the overall connection.</p><ul><li>New capability: Use itemSchema under content to define the structure of each streamed event.</li></ul><p>Supported sequential media types:</p><ul><li>SSE: text/event-stream</li><li>JSON Lines: application/jsonl</li><li>JSON Sequences: application/json-seq</li><li>Multipart Mixed: multipart/mixed</li></ul><p>This unlocks consistent tooling, clearer client expectations, and better validation for streaming APIs.</p><h3 id="a-quick-primer-how-streaming-differs-from-normal-responses">A quick primer: how streaming differs from “normal” responses</h3><ul><li>Normal response: One payload, one schema.</li><li>Streaming response: Many items over time; each item conforms to the itemSchema. The transport stays open while the server emits items; the client processes incrementally.</li></ul><p>Common patterns you’ll see:</p><ul><li>SSE for text/token streams in browsers</li><li>JSONL for structured event logs and incremental model outputs</li><li>Multipart for mixed binary/text chunks (e.g., speech + text)</li><li>Sentinel events like [DONE] to cleanly signal the end of a stream</li></ul><h3 id="example-describing-an-llm-s-streaming-api-sse-">Example: describing an LLM’s streaming API (SSE)</h3><p>Here’s a minimal but realistic OpenAPI 3.2 spec for a text‑generation endpoint that streams tokens via Server‑Sent Events. The server emits events as they become available; clients render tokens progressively.</p><!--kg-card-begin: code--><pre><code class="language-yaml">openapi: 3.2.0
info:
  title: LLM Streaming API
  version: 1.0.0
paths:
  /generate:
    post:
      summary: Stream generated text from the LLM
      description: |
        Streams model output incrementally using Server-Sent Events (SSE).
        Each event contains a token chunk; a sentinel signals the end.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [model, prompt]
              properties:
                model:
                  type: string
                  description: Model identifier (e.g., "gpt-4o-mini")
                prompt:
                  type: string
                  description: The input prompt for text generation
                max_tokens:
                  type: integer
                  minimum: 1
                  default: 256
                temperature:
                  type: number
                  minimum: 0
                  maximum: 2
                  default: 0.7
      responses:
        "200":
          description: Stream of generation events via SSE
          headers:
            Content-Type:
              schema:
                type: string
                enum: ["text/event-stream"]
          content:
            text/event-stream:
              itemSchema:
                oneOf:
                  - type: object
                    required: [event, data]
                    properties:
                      event:
                        type: string
                        enum: ["token"]
                        description: Event type
                      data:
                        type: object
                        required: [text, index]
                        properties:
                          text:
                            type: string
                            description: Token or text chunk
                          index:
                            type: integer
                            minimum: 0
                            description: Incrementing token index
                          logprobs:
                            type: number
                            nullable: true
                            description: Optional per-token log prob
                          finish_reason:
                            type: string
                            nullable: true
                            enum: ["stop", "length", "content_filter", null]
                  - type: object
                    required: [event, data]
                    properties:
                      event:
                        type: string
                        enum: ["summary"]
                      data:
                        type: object
                        properties:
                          usage:
                            type: object
                            properties:
                              prompt_tokens: { type: integer, minimum: 0 }
                              completion_tokens: { type: integer, minimum: 0 }
                              total_tokens: { type: integer, minimum: 0 }
                          model:
                            type: string
                  - type: object
                    required: [event, data]
                    properties:
                      event:
                        type: string
                        enum: ["done"]
                      data:
                        type: string
                        enum: ["[DONE]"]
        "400":
          description: Invalid request
          content:
            application/json:
              schema:
                type: object
                required: [error]
                properties:
                  error:
                    type: string
        "429":
          description: Rate limited
        "500":
          description: Server error</code></pre><!--kg-card-end: code--><p>Notes:</p><ul><li>text/event-stream matches the SSE transport browsers understand.</li><li>itemSchema with oneOf captures normal token events, an optional final summary, and the explicit end‑of‑stream sentinel.</li><li>Errors follow regular non‑streaming JSON shapes with standard HTTP codes.</li></ul><h3 id="variant-json-lines-jsonl-streaming-for-sdks-and-backend-clients">Variant: JSON Lines (JSONL) streaming for SDKs and backend clients</h3><p>If your clients prefer framed JSON instead of SSE, you can offer application/jsonl with the same itemSchema. Each line is one JSON object.</p><!--kg-card-begin: code--><pre><code class="language-yaml">responses:
  "200":
    description: Stream of generation events via JSON Lines
    content:
      application/jsonl:
        itemSchema:
          oneOf:
            - type: object
              required: [type, token]
              properties:
                type:
                  type: string
                  enum: ["token"]
                token:
                  type: object
                  required: [text, index]
                  properties:
                    text: { type: string }
                    index: { type: integer, minimum: 0 }
            - type: object
              required: [type, data]
              properties:
                type:
                  type: string
                  enum: ["done"]
                data:
                  type: string
                  enum: ["[DONE]"]</code></pre><!--kg-card-end: code--><h3 id="designing-a-great-streaming-contract">Designing a great streaming contract</h3><ul><li>Be explicit about termination: Include a clear sentinel (e.g., event: "done" with data: "[DONE]") so clients can stop reading deterministically.</li><li>Separate event kinds: Use oneOf to model different event shapes (tokens vs. summary vs. control).</li><li>Carry minimal context: Include a token index, optional finish_reason, and a final summary with usage for billing/UX.</li><li>Document timeouts and reconnection: For SSE you can also expose an SSE retry interval; for clients, document expected timeouts and backoff.</li><li>Error semantics: Prefer failing fast (non‑200) before starting the stream. If you must signal errors mid‑stream, reserve a control event type (e.g., event: "error") and describe it in oneOf.</li></ul><h3 id="client-expectations">Client expectations</h3><ul><li>SSE clients: Use EventSource in browsers or an SSE library on servers. Parse event and data fields.</li><li>JSONL clients: Read line‑delimited JSON; process each line as one event.</li><li>Backpressure: Streaming responses are pull‑driven by TCP; keep events small and frequent for smoother UX.</li><li>Idempotency: If you support retries, include id/cursor fields so clients can resume safely.</li></ul><h3 id="why-this-is-useful-for-llms">Why this is useful for LLMs</h3><ul><li>Faster first token: Users see output immediately, improving perceived latency.</li><li>Progressive rendering: Stream tokens as they’re generated; UIs don’t block.</li><li>Richer telemetry: Emit intermediary signals (e.g., tool_call, reasoning, usage) as distinct event types.</li><li>Interoperability: A standardized spec makes SDKs and gateways simpler to build and maintain.</li></ul><!--kg-card-begin: hr--><hr /><!--kg-card-end: hr--><p>See the OpenAPI 3.2 specification <a href="https://spec.openapis.org/oas/v3.2.0.html#complete-vs-streaming-content">here</a>.</p>
