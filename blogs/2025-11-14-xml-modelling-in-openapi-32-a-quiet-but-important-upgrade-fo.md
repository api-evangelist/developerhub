---
title: "XML Modelling in OpenAPI 3.2: A Quiet but Important Upgrade for Enterprise Teams"
url: "https://developerhub.io/blog/xml-modelling-in-openapi-3-2-a-quiet-but-important-upgrade-for-enterprise-teams/"
date: "Fri, 14 Nov 2025 14:54:07 GMT"
author: "Zaid Daba'een"
feed_url: "https://developerhub.io/blog/rss/"
---
<!--kg-card-begin: html--><div style="background-color: #effbff; border: 2px solid #c6f1ff; border-radius: 6px; padding: 8px 16px; width: 100%; margin-bottom: 24px;">🎉 Now supported in DeveloperHub</div><!--kg-card-end: html--><img alt="XML Modelling in OpenAPI 3.2: A Quiet but Important Upgrade for Enterprise Teams" src="https://developerhub.io/blog/content/images/2025/11/openapi3.2.png" /><p>JSON may dominate modern API design, but XML remains deeply embedded in many enterprise systems. Banks, insurers, logistics networks, public sector systems, and large internal platforms all continue to rely on XML for structured, strongly typed data.</p><p>OpenAPI had XML support before, but it was limited. It worked for simple examples, but it could not accurately represent real enterprise XML formats with namespaces, attributes, wrapped arrays, or mixed content.</p><p>OpenAPI 3.2 changes that. It introduces clearer, more expressive XML modelling that finally lets teams describe their XML in a faithful and predictable way.</p><h2 id="why-xml-needed-attention"><strong>Why XML Needed Attention</strong></h2><p>OpenAPI 3.1 could express a few basics, but it fell short when modelling XML such as:</p><ul><li>elements that mix attributes and child content</li><li>namespaced payloads</li><li>text content inside complex types</li><li>wrapped arrays with strict naming</li><li>schemas where element names differ from property names</li></ul><p>These gaps forced teams to maintain parallel XSDs, rely on proprietary extensions, or manually document XML behaviour in prose.</p><p>OpenAPI 3.2 closes many of these gaps.</p><h2 id="1-clear-namespace-support"><strong>1. Clear Namespace Support</strong></h2><h3 id="before-openapi-3-1-"><strong>Before (OpenAPI 3.1)</strong></h3><p>There was no clean way to define a namespace prefix. Tools guessed from XML examples or ignored the namespace entirely.</p><!--kg-card-begin: code--><pre><code class="language-yaml">xml:
  namespace: "http://acme.com/payments"</code></pre><!--kg-card-end: code--><p>No way to say which prefix to use. No way to ensure generated XML matched the contract.</p><h3 id="now-openapi-3-2-"><strong>Now (OpenAPI 3.2)</strong></h3><p>Namespaces can define both the URI and the prefix, allowing accurate generation and parsing.</p><!--kg-card-begin: code--><pre><code class="language-yaml">xml:
  namespace:
    uri: "http://acme.com/payments"
    prefix: "pay"</code></pre><!--kg-card-end: code--><p>This is essential for enterprise XML that relies heavily on namespaced elements.</p><h2 id="2-more-accurate-representation-of-attributes"><strong>2. More Accurate Representation of Attributes</strong></h2><h3 id="before"><strong>Before</strong></h3><p>Attributes were possible but poorly defined. Adding both attributes and text content in the same structure often confused tools.</p><p>For example, this structure could not be expressed reliably:</p><!--kg-card-begin: code--><pre><code class="language-xml">&lt;amount currency="USD"&gt;120.50&lt;/amount&gt;</code></pre><!--kg-card-end: code--><h3 id="now"><strong>Now</strong></h3><p>OpenAPI 3.2 supports well defined attribute modelling alongside text content.</p><!--kg-card-begin: code--><pre><code class="language-yaml">type: object
xml:
  name: amount
properties:
  currency:
    type: string
    xml:
      attribute: true
  value:
    type: string
    xml:
      text: true</code></pre><!--kg-card-end: code--><p>The result closely mirrors the real XML rather than reducing everything to elements.</p><h2 id="3-proper-handling-of-wrapped-arrays"><strong>3. Proper Handling of Wrapped Arrays</strong></h2><h3 id="before-1"><strong>Before</strong></h3><p>Wrapped arrays were supported, but only at a basic level, and always assumed generic wrapping. Complex wrapped structures were impossible to describe.</p><p>For example, this common structure:</p><!--kg-card-begin: code--><pre><code class="language-xml">&lt;items&gt;
  &lt;item&gt;One&lt;/item&gt;
  &lt;item&gt;Two&lt;/item&gt;
&lt;/items&gt;</code></pre><!--kg-card-end: code--><p>was supported, but:</p><!--kg-card-begin: code--><pre><code class="language-xml">&lt;inventory&gt;
  &lt;products&gt;
    &lt;product&gt;...&lt;/product&gt;
  &lt;/products&gt;
&lt;/inventory&gt;</code></pre><!--kg-card-end: code--><p>could not be modelled with its multiple layers of specific element names.</p><h3 id="now-1"><strong>Now</strong></h3><p>OpenAPI 3.2 allows precise control of wrapper element names.</p><!--kg-card-begin: code--><pre><code class="language-yaml">type: array
xml:
  name: products
  wrapped: true
items:
  xml:
    name: product</code></pre><!--kg-card-end: code--><p>More complex multi level wrappers now work predictably.</p><h2 id="4-more-faithful-complex-types-with-text-content"><strong>4. More Faithful Complex Types with Text Content</strong></h2><h3 id="before-2"><strong>Before</strong></h3><p>Mixed content was not representable. Any element containing both text and child elements was impossible to model correctly.</p><p>For example:</p><!--kg-card-begin: code--><pre><code class="language-xml">&lt;note priority="high"&gt;This is a message &lt;bold&gt;sent today&lt;/bold&gt;&lt;/note&gt;</code></pre><!--kg-card-end: code--><p>This simply could not be expressed.</p><h3 id="now-2"><strong>Now</strong></h3><p>OpenAPI 3.2 introduces clearer rules for text content inside objects.</p><!--kg-card-begin: code--><pre><code class="language-yaml">type: object
xml:
  name: note
properties:
  priority:
    type: string
    xml:
      attribute: true
  content:
    type: string
    xml:
      text: true
  bold:
    type: string</code></pre><!--kg-card-end: code--><p>Tools can now produce mixed content more consistently.</p><h2 id="5-cleaner-examples-with-namespaces-and-structure"><strong>5. Cleaner Examples with Namespaces and Structure</strong></h2><p>XML examples can now be rendered exactly as intended.</p><h3 id="before-3"><strong>Before</strong></h3><p>Examples were often rendered incorrectly because tools lacked information about prefixes, attribute ordering, or nested wrappers.</p><h3 id="now-3"><strong>Now</strong></h3><p>An example such as:</p><!--kg-card-begin: code--><pre><code class="language-yaml">examples:
  simple:
    value: |
      &lt;pay:transaction xmlns:pay="http://acme.com/payments"&gt;
        &lt;pay:amount currency="USD"&gt;120.50&lt;/pay:amount&gt;
      &lt;/pay:transaction&gt;</code></pre><!--kg-card-end: code--><p>can be generated, documented, and validated correctly.</p><h2 id="why-this-is-important-for-enterprise-teams"><strong>Why This Is Important for Enterprise Teams</strong></h2><p>These improvements mean:</p><ul><li>fewer separate XSDs to maintain</li><li>fewer vendor extensions</li><li>more predictable generated XML</li><li>documentation that finally matches real world XML</li><li>less confusion for partners integrating with legacy systems</li><li>better long term compatibility for platforms that cannot switch to JSON</li></ul><p>It is a practical modernisation of XML within OpenAPI, not a theoretical tweak.</p><h2 id="a-step-toward-real-enterprise-support"><strong>A Step Toward Real Enterprise Support</strong></h2><p>OpenAPI has been strongly focused on JSON for years, but OpenAPI 3.2 broadens the specification to reflect the reality inside many large organisations.</p><p>By strengthening XML modelling, it gives developers a clear, standardised way to describe the formats they actually use.</p>
