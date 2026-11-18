# SEO and monetization

## S01 · Public route contract

Canonical origin is https://taptelo.com after domain control and HTTPS verification. Test HTTP and www redirection. Use lowercase stable ASCII slugs. Locale is the first path segment.

| Class | Route pattern and example | Index policy |
| --- | --- | --- |
| Type hub | /az/channels, /az/groups, /az/bots | Index eligible when useful and populated |
| Scope hubs | /az/channels/country/azerbaijan, /az/channels/language/ru, /az/bots/category/pdf-tools | Index eligible if approved, populated and distinct |
| Editorial collection | /az/channels/collections/baki-it-vakansiyalari | Curated scope, useful copy and active results |
| Listing | /az/channels/baki-it-vakansiyalari-1842 | Approved, sufficient content, healthy link and stable identity evidence |
| Browse query | /az/channels/search?q=kiraye&country=az | noindex, excluded from sitemap. Use static eligible hubs for crawl discovery. |
| Pagination | /az/channels?page=2 | Unique links and self canonical for valid crawlable pages. Out-of-range returns 404. |
| Admin, drafts and status | /admin and restricted status routes | Authentication or opaque holder token as appropriate, noindex and no-store |

Type paths are fixed across locales. Localize titles, meta descriptions and main editorial copy. Keep an immutable listing ID and frozen slug. Redirect an intentionally changed slug to its one canonical URL. Avoid parallel route variants for one collection. Link type hubs, scope hubs and related profiles through standard HTML anchors. Breadcrumb markup mirrors visible navigation. Structured data describes only visible, supported content and does not imply rich-result eligibility.

## S02 · Discovery and freshness

The [publication contract](08-publication-contract.md) owns HTTP, CTA, sitemap and Greenpage decisions for listings.

Return working HTTPS 200 pages with meaningful main content and same essential mobile and desktop HTML. Set canonical URLs to the intended host and emit reciprocal hreflang only between genuine localized equivalents. Use a neutral x-default only for a real fallback destination. Do not redirect explicit locale or scope URLs from IP or device preferences. Unapproved or unknown public routes return 404. A temporarily suspended profile has a useful status page with noindex and no broken Telegram CTA. A permanently removed entry returns 410 or 404 when a distinguishable tombstone is inappropriate. Public filter/search pages that use noindex must remain crawlable enough to read that directive. Authentication protects private content.

Publish only canonical, eligible URLs in sitemap XML. Group sitemap files by locale/type when useful. Use accurate lastmod from material content or link changes. Link and stat probes alone do not update lastmod. Invalidate impacted hubs and locales on change. Submit initial sitemap through Google Search Console and Bing Webmaster Tools at setup. Batch IndexNow updates for participating engines after origin state is visible. Do not apply Google Indexing API to directory pages.

Greenpage is a Taptelo operational state, derived from eligibility plus link health. Show that state, last success and engine observations separately. Google visibility is observed via Search Console with quota-aware sampling and prioritized monitoring of anomalous and key pages. A missing observation is unknown. Admin dashboard should show freshness failures, canonical mismatches, soft 404 symptoms, crawl/index coverage and affected route types. Discovery, crawl, index and rank are distinct outcomes.

## R01 · Monetization

Default revenue is direct contextual sponsorship on AZ country/category lists. Start with one visibly labeled block after the sixth to eighth organic result on sufficiently long eligible lists. Use a dedicated placement separate from cards and Telegram CTAs. Disable profile, exact search, empty, submission, report and legal placements by default. Desktop sidebar is a separately measured experiment. Reserve space to prevent layout shifts. Paid links use rel=sponsored. Sponsor selection never changes organic ranks or bypasses moderation.

Enable AdSense only on genuinely eligible, supported-language pages after account approval and configured consent rules. Azerbaijani-primary pages carry no Google ad code. Check the page's actual primary content language, regardless of UI locale. Switch third-party advertising off when regional policy or consent prevents it. Track aggregate sponsor views and clicks with clear denominators. Compare revenue per 1,000 eligible sessions with Telegram outbound rate, mobile performance and layout stability.

Official references: [Google Search Essentials](https://developers.google.com/search/docs/essentials) · [Sitemaps](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap) · [Localized pages](https://developers.google.com/search/docs/specialty/international/localized-versions) · [IndexNow](https://www.indexnow.org/documentation) · [AdSense language policy](https://support.google.com/adsense/answer/9727) · [Ad placement](https://support.google.com/adsense/answer/1346295)
