# PinkOut static website

Upload-ready HTML, CSS and JavaScript for **https://pinkout.asem.work/**. No React, Next.js, Node.js runtime, backend, build process, npm packages, analytics or external CDN requests are required. All imagery and Alexandria fonts are included.

## Upload

1. Extract the ZIP.
2. Upload the **contents of `website/`** into the document root for `pinkout.asem.work`. `index.html` should be directly inside that root. Keep the `assets/` and `en/` folders beside it.
3. Enable HTTPS, text compression and static-file caching on your server. `nginx.example.conf` provides an optional configuration; adapt its document-root path to your server. It does not replace your existing Nginx configuration.
4. Visit `/` for Arabic and `/en/` for English. The first visit defaults to Arabic. A language explicitly selected through the switch is remembered on the device.
5. After going live, submit `https://pinkout.asem.work/sitemap.xml` to your search-engine webmaster tools.

For local review, serve `website/` with any static-file server and open its localhost URL. For example, from that directory: `python3 -m http.server 8000`. This is only a local preview helper; production does not need Python or any application server. Directory-based language links are designed for HTTP hosting.

## Files you will edit

| File | Purpose |
|---|---|
| `index.html` | Complete Arabic page, text, image descriptions and Arabic metadata |
| `en/index.html` | Complete English page and metadata |
| `assets/tokens.css` | Alexandria font faces and shared brand, spacing, radius and motion tokens |
| `assets/style.css` | Layout, component styling and responsive rules |
| `assets/app.js` | Menu, galleries, dialogs, language preference, validation and optional API connections |
| `assets/config.js` | Empty API and app-store URLs, ready for your later integration |
| `sitemap.xml`, `robots.txt` | Crawl and language discovery |
| `llms.txt` | Concise public facts and pending-information summary for AI readers |

## Backend connection

The requested website is static. All three forms validate input, but **they do not submit, store or log personal details** while their endpoints in `assets/config.js` are empty. Valid preview submissions explicitly say that no data was sent or saved. No signup is falsely reported as successful.

When your backend is ready, set:

```js
window.PINKOUT_CONFIG = Object.freeze({
  waitlistEndpoint: '/api/waitlist',
  partnershipEndpoint: '/api/partnership',
  applicationEndpoint: '/api/application',
  appStoreUrl: '',
  googlePlayUrl: ''
});
```

Each configured form POSTs JSON with `Content-Type: application/json` and `Accept: application/json`. Same-origin credentials are included. Do not put secrets in this public file. If Laravel middleware requires a CSRF token, implement token acquisition and the appropriate header in `assets/app.js`, or configure your public endpoint strategy on the server.

| Form | Payload fields |
|---|---|
| Waitlist | `name`, `phone`, `email`, optional `city`, `age_range`, optional `interests` array, `consent: true`, `language` |
| Partnership | `company`, `name`, optional `job_title`, `email`, `phone`, optional `website`, `type`, `message`, `consent: true`, `language` |
| Talent application | `name`, `email`, `phone`, optional `website`, `type`, `message`, `consent: true`, `language` |

Phone input supports international numbers and converts Arabic/Persian digits to Latin digits in the submitted payload. The server must perform its own validation, rate limiting, consent handling, storage and access controls. A form reports success **only** when the server returns a successful HTTP response containing:

```json
{"success": true}
```

Other responses, malformed JSON, network failures and a 15-second timeout produce a localized error and keep the entered fields available for retry. The exact waitlist success message from the brief is already included for connected submissions.

## Before accepting real registrations

Publish the approved privacy policy and terms in **both** HTML pages. The current dialogs honestly state that these policies are pending and that static forms do not transmit personal details. Once collection is enabled, replace those notices and update the form-availability paragraph in `llms.txt`.

The exact address, opening date, child and companion policies, social handles and business email remain unconfirmed. Add them only after approval. No countdown, invented map, sponsor logos, booking, checkout or membership features were added.

All programme cards are marked as ideas/examples. General entry is presented as a planned **SAR 35**, not a live ticket offer. App imagery is marked as a concept preview.

## When the app launches

Add the actual HTTPS App Store and Google Play URLs to `assets/config.js`. Store badges become working download links for the correct store domains. Change the app-launch call to action and coming-soon copy in both HTML pages as appropriate. Replace the labelled QR placeholder with a real QR asset after its destination is confirmed. Social placeholders are text, not invented links.

## Performance and search

- Static content is immediately available to browsers, search crawlers and AI readers without JavaScript rendering.
- Responsive WebP versions use `srcset`, explicit dimensions and native lazy loading.
- The hero is prioritized and responsive; lower sections and dialog imagery are lazy-loaded.
- Four local Alexandria font weights use `font-display: swap`. No external font or image services are needed.
- No JS libraries, scroll hijacking, autoplay video, tracking scripts or application bundles.
- Both pages include titles, descriptions, canonical URLs, language alternatives, Open Graph text metadata and JSON-LD for the organization, website, page and visible FAQ.
- `llms.txt` is an additional readable summary, not a guarantee of AI-search inclusion. Search indexing, rankings and performance scores depend on the live server and device.

Keep `index.html`, `en/index.html`, JSON-LD and `llms.txt` consistent when changing facts. If the domain changes, update canonical, language links, schema, sitemap, robots and llms URLs together.

## Visual sources and licensing

The Option 2 logo is extracted from the supplied PinkOut PDF as a transparent vector. Destination renders, programme reference images and app mockups are extracted from that PDF and compressed; no additional generated or stock imagery was needed. Alexandria’s included SIL Open Font License is at `website/assets/alexandria-OFL.txt`.

## Verification

See `QA.md` for completed source and interaction checks, asset sizes, and the browser-testing limitation. Responsive rules cover the design-system widths, but browser layout, device rendering, Core Web Vitals, Lighthouse scores and full WCAG conformance were not verified here.
