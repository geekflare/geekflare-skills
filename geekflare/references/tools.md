# Geekflare MCP tool reference

Parameter notes for the hosted Geekflare MCP server (`https://mcp.geekflare.com/{API_KEY}/mcp`). The live tool schema wins if anything here differs. Every tool requires `url` except `search`, which requires `query`.

## Search and content

### `search`

| Parameter | Values | Notes |
| --- | --- | --- |
| `query` | string, ≤2048 chars | Required |
| `limit` | 1–100, default 10 | |
| `source` | `web` (default), `news`, `images` | SERP mode accepts one source |
| `category` | `general`, `code`, `pdf`, `research`, `linkedin`, `wiki` | Ignored in SERP mode |
| `time` | `any`, `h`, `d`, `w`, `m`, `y`, or with a count (`h6`, `d7`) | Recency filter |
| `location` | ISO alpha-2 country code, default `us` | |
| `city` | e.g. `London,England,United Kingdom` | Must match the [supported cities list](https://cdn.geekflare.com/api-assets/geotargets-2026-08-12.json); overrides `location` |
| `device` | `desktop`, `mobile` | |
| `includeDomains` / `excludeDomains` | string arrays | |
| `format` | `json` (default), `markdown`, `html` | Ignored in SERP mode |
| `scrape` / `scrapeLimit` | boolean / 1–10 | Fetch content of top results; ignored in SERP mode |
| `groundedAnswer` | boolean | AI answer grounded in results; ignored in SERP mode |
| `serp` | boolean | Full Google first page: organic results, AI Overview, People Also Ask, related searches, pagination |

### `webScrape`

| Parameter | Values | Notes |
| --- | --- | --- |
| `format` | up to 3 of `markdown` (default), `markdown-llm`, `html`, `html-llm`, `text`, `text-llm`, `json` | `*-llm` variants strip boilerplate |
| `renderJS` | boolean | Omit to let Geekflare decide |
| `waitTime` | seconds | Wait after load for late content |
| `device` | `desktop`, `mobile` | |
| `blockAds` | boolean, default true | |
| `fileOutput` | boolean | Return a download URL instead of inline data (large pages) |
| `aiPrompt` | object, shape by `type` | `prompt` (query), `schema` (schema), `listing` (itemSchema, maxItems), `summary` (style, focus, maxLength), `sentiment` (aspects), `keywords` (maxKeywords, includeEntities). Costs extra credits |
| `extractionMode` | `default`, `cssSchema`, `xpathSchema`, `template` | Returns JSON |
| `extractionSchema` | object | `{ name, baseSelector?, fields: [{ name, selector, type, attribute?, fields? }] }` |
| `template` | `product`, `contact` | With `extractionMode: "template"` |

Example: extract product fields

```json
{ "url": "https://shop.example.com/item/123", "extractionMode": "template", "template": "product" }
```

Example: schema extraction with AI

```json
{
  "url": "https://example.com/pricing",
  "aiPrompt": {
    "type": "schema",
    "schema": { "plans": [{ "name": "string", "monthlyPrice": "string", "features": ["string"] }] }
  }
}
```

### `metaScrape`

Title, description, canonical URL, Open Graph, and Twitter Card tags. Options: `format` (`json`, `markdown`), `renderJS` (default true), `device`, `blockAds`, `fileOutput`.

### `brand`

Name, logos, colors, fonts, social profiles, links. `mode`: `standard` or `enriched` (adds company intelligence). `refresh: true` fetches fresh data instead of the cache. Fields can be missing for sites that do not publish them; report them as missing rather than guessing.

### `screenshot`

`fullPage`, `device`, `viewportWidth` (320–3840) / `viewportHeight` (240–2160), `scaleFactor`, `selector` (+ `fallbackToFullPage`), `delay` (0–10 s), `theme` (`light`, `dark`, `auto`), `type` (`png`, `jpeg`, `webp`), `quality`, `hideCookie` (default true), `blockAds`, `disableAnimations`, `removeBackground` (PNG), `highlightLinks` (outline links for vision models), `addTimestamp`, `inline` (image data instead of a hosted URL).

### `url2Pdf`

`format` (`a4` default, `letter`, `legal`, `a0`–`a6`), `orientation`, `margin` (`{top,right,bottom,left}` in mm), `scale` (0–2), `device`, `hideCookie`, `addTimestamp`.

## Website diagnostics

| Tool | Checks | Extra parameters |
| --- | --- | --- |
| `siteStatus` | Up/down and HTTP status | `followRedirect` |
| `dnsRecord` | A, AAAA, CNAME, MX, NS, SOA, TXT, CAA, SRV | `types` (array) |
| `dnsSec` | DNSSEC enabled and validating | |
| `tlsScan` | Supported protocol versions (SSLv2 → TLS 1.3), certificate details and expiry | |
| `redirectCheck` | Full redirect chain | |
| `brokenLink` | Broken links on the page | `followRedirect` |
| `mixedContent` | HTTP resources on HTTPS pages | `followRedirect` |
| `loadTime` | Page load time | `targetCountries` (≤3 ISO codes) for a per-location breakdown |
| `ttfb` | Time to first byte | |
| `httpHeader` | Response headers (security headers, caching) | |
| `httpProtocol` | HTTP/1.1, HTTP/2, HTTP/3 support | |
| `lighthouse` | Performance, SEO, accessibility, best practices | `device`, `followRedirect`, `parameters` (extra Lighthouse CLI flags) |
| `ping` | Latency | URL or IP |
| `mtr` | Network path and per-hop loss | |
| `openPorts` | Open ports, optional service detection | `topPorts` (50/100/500/1000/5000), `portRanges` (`80,443,1000-1010`), `detectServices`. Authorized hosts only |

Several diagnostics accept `proxyCountry` (ISO code) to run the check from another country, for example to compare availability by region.
