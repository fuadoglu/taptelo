# Codex delivery plan

## Work packages

All packages below are pending implementation. Complete in dependency order. For each package, record requirement IDs, touched routes/data, verification commands and results, migrations, operational assumptions and unresolved evidence. The package's tests are part of completion. Keep production promotion out of ordinary development PRs.

| ID | Deliverable and dependencies | Completion evidence |
| --- | --- | --- |
| T01 Foundation | Laravel 13 / PHP 8.4 skeleton, locked dependencies, DB/CI, shared-host public root, web/CLI health, AZ/EN translation configuration, staging profile. | Clean install and migrate on primary DB, web smoke, docs and composer checks. |
| T02 Catalog data | T01. Listings, revisions, identity and username constraints, taxonomy/region/language, typed relationships, local fixtures. | Uniqueness and lifecycle integration tests; CLI seed explicitly disallowed in production. |
| T03 Moderation and trust | T02. Submission/revision moderation, status tokens, administrator MFA, audit, reports, privacy/takedown flow, outbox. | Approval races, pending isolation, duplicate flows and urgent removal are exercised. |
| T04 Browse and discovery | T02–T03. SSR public hubs, profiles, saved items, exact and relevant search, locale/country behavior, mobile/desktop UI. | Reviewed relevance fixture suite and responsive/accessibility acceptance tests. |
| T05 Telegram health | T03–T04. Provider contract, manual fallback, media, metrics/history, scheduled retries and link health. | Simulated timeout, rate limit, username rollover, mismatch, missing metrics, and real staging capability evidence when available. |
| T06 SEO pipeline | T04–T05. URL eligibility, sitemap, lastmod, hreflang, canonical, IndexNow, diagnostics and Search Console observation. | Crawl-based validation, queue replay, temporary suspension and permanent removal tests. |
| T07 Sponsorship and legal surfaces | T03–T06. Sponsor inventory, policy switches, consent enforcement, legal routes and operational privacy lifecycle. | No disallowed injection, opt-out behavior, real operator text review before production. |
| T08 Release qualification | T01–T07. Hosting preflight, backup and restore, perf matrix, moderation/content seed, SEO and incident runbooks. | Signed launch checklist with route checks, restore proof and measured budgets. |

## Red-team acceptance matrix

| ID | Failure attempted and required result |
| --- | --- |
| QA-01 | Open a shared AZ country link with different browser locale and visiting IP. Same localized page and content render. Neutral entry respects manual preferences. |
| QA-02 | Search known username through a conflicting scope/tab, plus AZ/RU-script aliases, short tokens and typo queries. Exact search wins globally, topic filters stay intact, zero-result actions are explicit. |
| QA-03 | Submit duplicates in parallel and approve the same item in two sessions. One active identity/username, one effective published revision, reproducible outbox replay. |
| QA-04 | Fetch pending, personal status and admin data through public routes, sitemap and caches. Public output exposes none of the private fields. |
| QA-05 | Simulate expired username, reassignment, rate limit and API outage. Retain good data for transient failure. Suppress an unsafe click and suspend on mismatch. Preserve stable listing ID. |
| QA-06 | Suspend a previously cached entry and remove an entry still in sitemap. Takedown reaches current public lists, cached routes and sitemap according to the visibility contract. Confirm removed status and no stale CTA. |
| QA-07 | Render without JavaScript and test mobile filters, screen-reader names, keyboard operations and back restoration with JavaScript. Public forms and direct links work in the baseline. |
| QA-08 | Crawl a real sitemap and cross-check HTTP, redirects, canonical, reciprocal hreflang, next-page links and internal discovery. Query pages with noindex remain out of sitemap. A harmless refresh leaves lastmod unchanged. |
| QA-09 | Open AZ-primary pages and consent-denied paths with monetization enabled in configuration. Google ad code stays absent where ineligible. Organic rank is identical whether a sponsor is scheduled or not. |
| QA-10 | Interrupt Cron mid-job, then run concurrently. Lease expires, retry is bounded, no replay duplicates. Restored DB/media pass the release smoke test. |
| QA-11 | Simulate a data deletion and restore an older staging snapshot. Deletion is reapplied and content stays withdrawn. |
| QA-12 | Measure real shared hosting under agreed load. Enforce 75 KB first-party compression budget and record search latency, crawlability and ad/layout performance. |

## First Codex task

Implement **T01 only**. Read the repository instructions and specifications, then scaffold the local application and CI around the selected PHP/MariaDB baseline. Create AGENTS-compatible commands for install, migrations and checks. Verify clean setup and record which checks ran. Use placeholder configuration values and fixture-only identities. Do not create production DNS, admin credentials or provider tokens. Stop at the T01 acceptance boundary and summarize prerequisites for T02.

References: [Laravel deployment](https://laravel.com/docs/13.x/deployment) · [Laravel releases](https://laravel.com/docs/13.x/releases) · [Codex repository workflow](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
