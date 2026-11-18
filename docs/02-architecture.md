# Architecture and data

## A01 · Decisions

- ADR-01: Laravel 13 modular monolith, PHP 8.4 minimum, Blade SSR, vanilla progressive JavaScript and CSS. Composer lockfile in Git. Pin the supported concrete database versions in CI. MariaDB 10.11 LTS is primary. Advertise MySQL 8.4 support only when its integration suite passes.
- ADR-02: Standard shared hosting with HTTPS, web root pointed at public/, PHP CLI and Cron, no required daemon, Redis or production Node.js. Keep configuration, writable storage and dependencies outside the served document root. Build deploy artifacts in CI or locally.
- ADR-03: A relational source of truth with durable outbox and bounded scheduler. Current visibility lives in SQL. Derived caches, sitemap entries and counters can be rebuilt.
- ADR-04: Exact indexed identity and prefix lookup, SQL full-text and a controlled locale-aware term/alias table. Typo re-ranking is bounded and only applied to retrieved candidates. No external search server or scraper in the production baseline.
- ADR-05: Telegram enrichment is an adapter capability. Manual verification is a supported workflow. Missing capabilities yield unknown fields, not fabricated metadata or automatic publish.
- ADR-06: Public listing ID and publication status are independent of provider health, provider username, submission status and observed search index status.

Suggested modules under app/: Catalog, Discovery, Moderation, Telegram, SearchVisibility, Sponsorship, Privacy and Operations. Use Laravel routing, forms, validation, authorization, sessions, scheduling and migrations. Public, admin and CLI entry points reuse domain services. Admin mutations are authorized server-side.

## A02 · Data contract

| Entity | Minimum fields and constraints |
| --- | --- |
| listings | Internal ID, immutable public ID and frozen slug, kind, approved revision pointer, publication status, current public username, first-published and last material content timestamps |
| listing_revisions | Listing, editor/submitter provenance, name, summary, localized copy, category and attribute snapshot, source and approval timestamps. Pending changes do not leak into live data. |
| telegram_identities | Provider namespace, peer type and canonical decimal ID as a string, current username and verification time. Unique provider/peer identity. Provider-specific raw IDs require a tested normalization rule before merging. |
| telegram_usernames | Lowercase normalized username, claim/release time and identity connection. A unique active username mapping prevents duplicate destinations. Retired aliases never silently inherit a new owner's identity. |
| taxonomy and listing facets | Category keys, parents, localized labels, aliases, relations to listing country/city/language/topic. Version changes for invalidation. |
| submissions | Submitted link and type, review state, approved/rejected reason, holder token hash, optional restricted contact and expiry. |
| listing_health and observations | Health state, attempt and success timestamps, next due time, source, metric and nullable value. One idempotent metric sample per listing, metric and time bucket. |
| reports, decisions and audit_events | Reason, resolution, actor, timestamps, applicable retention. Restricted internal notes and identity information. |
| seo_pages and outbox_jobs | Normalized route and locale, publication eligibility and content version. Outbox contains unique event key, attempt count, not-before, lease and failure state. |
| sponsorships | Approved creative/destination, display locale, scope, run dates, status and placement. Separately aggregated impression and click counts. |

Use UTF-8/utf8mb4 and UTC. Use transactions and foreign keys. Make uniqueness enforcement portable. A separate current-username table with username as primary key is acceptable. Avoid locale or null-sensitive uniqueness assumptions. Keep click counters off the publication critical path.

## A03 · State and consistency

Use the [publication contract](08-publication-contract.md) as the authority for public policy outputs and visibility-version invariants.

- Submission: pending → approved or rejected. Edits are new pending revisions. Reopening is an explicitly audited action.
- Publication: draft → published → suspended or removed. Suspended can return to published after review. Removed URLs retain only permissible tombstone information.
- Health: unverified, healthy, temporarily unavailable, identity mismatch or confirmed unavailable. Search engine observation has its own unknown, indexed and excluded states.
- Approve with row lock, revision/identity checks and transaction. Update the approved revision, current visibility and outbox in that transaction. Concurrent approval of the same listing is idempotent. Public queries, sitemap and cache hits enforce visibility. For urgent suspension, old cached HTML and deep links must become unavailable immediately through versioned keys or an authoritative visibility check. Do not rely on eventual TTL for takedowns.
- After commit, localized route caches and impacted lists are rebuilt and a changed-URL notification is queued. Cache generation uses versioned keys and atomic file writes. Store the active sitemap version atomically. If a job fails mid-run it is safe to retry and jobs can be replayed by event key.
- PHP Cron drains bounded batches with a time deadline and overlapping-run protection. Fetch external resources outside DB transactions. Respect provider retry signals, back off with jitter and surface failed/exhausted jobs to admin. Recovery never publishes an unreviewed revision.
