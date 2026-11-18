# Publication, health and indexing contract

This is the acceptance authority for route status, public visibility, CTA, cache behavior and sitemap. Use one domain policy to derive public decisions from the approved listing revision, publication state, identity evidence and the current time. Keep Google/Bing observations outside this policy. All views, sitemap generation, notifications and tests consume the same decision.

## Precedence and outputs

Read rows top to bottom. The first match wins. An eligible row still needs an original purpose sentence, the required category, sufficient localized content and an actual localized page before index eligibility. A published profile with incomplete content uses the limited row below, even when its link works. Use noindex in the response and leave public noindex routes crawlable. HTTP failures follow normal semantics without relying on robots metadata.

| First matching state | Public HTTP and robots | Telegram CTA | Discover/search | Sitemap and Greenpage |
| --- | --- | --- | --- | --- |
| Nonexistent listing, draft, pending-only or rejected-only | 404 | Hidden | Exclude | Exclude, red |
| Removed, tombstone not permitted | 404 | Hidden | Exclude | Exclude, red |
| Removed, tombstone permitted | 410. Generic removed message with no former profile or owner details. | Hidden | Exclude | Exclude, red |
| Suspended or identity mismatch | 200 noindex with generic unavailable copy. Effective suspension takes precedence over remaining provider data. | Hidden | Exclude | Exclude, red |
| Published but provider has confirmed destination unavailable | 200 noindex pending editorial resolution, with report action | Hidden | Exclude | Exclude, red |
| Published, no usable destination evidence or grace has expired | 200 noindex with last-check explanation | Hidden | Exclude | Exclude, amber |
| Published with usable evidence and incomplete indexable copy | 200 noindex | Visible | Include if listing has required functional catalog fields | Exclude, amber |
| Published with usable evidence, sufficiently localized content and no overdue check | 200, index eligible | Visible | Include | Include, green |
| Published with usable evidence, sufficiently localized content and overdue check | 200, index eligible | Visible | Include | Include, amber. Show last confirmed check date. |

An edit pending review does not change the live row of a previously published profile. A takedown or mismatch overrides all approved revisions, locales and cached representations. The report workflow may distinguish confirmed Telegram unavailability from an editorial permanent removal. Google indexing status is shown separately as indexed, excluded or unknown.

## Evidence and time boundaries

- Usable destination evidence means a successful matching API observation or an audited manual verification of the public destination. Manual verification records the observed URL, admin, evidence time and source. It cannot claim Telegram ID confirmation unless an approved provider confirmed the ID. Only pair a new username with an existing provider ID after an approved matching observation.
- Store timestamps in UTC. A check is due after 24 hours for prominent entries and 72 hours for other entries. Manual-only profiles follow the same due times in a manual queue. Existing valid evidence stays usable until **96 hours after its due time**. These are initial operational defaults. At the due timestamp the evidence is overdue. At the grace expiry timestamp the evidence becomes unusable.
- Timeouts, rate limits and ambiguous responses preserve the last evidence and schedule retries. A mismatch immediately takes precedence, even if a prior success is still fresh. A new successful observation resets the due time. A bot functional review and audience metric age do not modify link evidence.
- Capture now once per policy evaluation. Test boundary timestamps and replay with a frozen clock. Material edits to approved display copy or destination change lastmod. A retry, evidence refresh or index observation alone leaves lastmod intact.

## Atomic publication and withdrawal

Approval, withdrawal, username ownership and the outbox intent are committed transactionally. Derived output can lag after a publish, but no withdrawn listing or CTA may remain in a new response after the withdrawal commits. Use one visibility version across affected public artifacts, advance it in the same mutation transaction, and validate it before any origin cache hit. Take expiry deadlines into account even without a database mutation. A cached representation must expire by the next relevant evidence boundary.

Generate sitemap XML from the current visibility version and eligibility policy. Serve a current SQL-backed slice on cache/version mismatch, or return a transient 503 if current eligibility cannot be calculated. Never serve a superseded sitemap artifact after withdrawal. Public browse results can recompute or show a recoverable error when their current cache is unavailable. Start without edge caching of mutable HTML or XML. Enabling it requires evidence that the same withdrawal and expiry contract is preserved.

IndexNow dispatch is asynchronous after the new public state is observable. Its outage must not roll back publication or withdrawal. Search engine listings can lag the origin; admin observes external removal separately. For mandatory removals, verify all origin languages, category results, sitemap slices and former avatar access when the media itself was withdrawn. No cached raw provider payload is public.

## Fixed acceptance examples

| Case | Input | Required output |
| --- | --- | --- |
| PUB-01 | Published prominent listing, last verified at 2026-11-18T12:00Z, request at 2026-11-19T12:00Z, temporary Telegram timeout | 200, CTA present, discoverable, sitemap present, amber, unchanged lastmod |
| PUB-02 | Same fixture at 2026-11-23T12:00Z, no newer success | 200 noindex, CTA absent, excluded from discovery and sitemap |
| PUB-03 | Username now answers with another peer ID, with warm cache and two locales | Immediate suspension after commit, 200 noindex, no CTA, absent in both locale searches and current sitemap |
| PUB-04 | Matching peer ID has approved new username | Same local profile URL and ID, updated direct CTA and material lastmod |
| PUB-05 | Pending revision changes published title, no admin approval | Prior title and CTA remain public. No draft field appears in search, cached HTML or sitemap. |
| PUB-06 | Withdrawal replayed after a worker timeout | One withdrawal outcome, expected 404 or 410, no stale cards, no duplicate notification intents |
