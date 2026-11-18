# Production operations

## O01 · Shared hosting baseline

Preflight confirms PHP 8.4+ with CLI and web versions aligned, PDO MySQL, required Laravel extensions, mbstring, curl, intl and an approved image processing library where needed. Confirm MariaDB 10.11 LTS or integration-tested MySQL 8.4, HTTPS, URL rewriting, document root public/, write access only to intended paths, private config/storage, DB backup and restore, Cron and outbound HTTPS to enabled providers. Composer may run outside production. Configure the host to run the Laravel scheduler every minute where available. A slower supported Cron must pass queue and freshness targets before promotion. Use bounded single-writer runs and observable heartbeats. Cloudflare country headers are optional and trusted only through a verified edge path. Fall back to Global when uncertain. Make edge HTML caching opt-in and prove purge authority before enabling it.

Targets on representative pages: 75 KB maximum compressed first-load HTML + first-party CSS + first-party JS, measuring a cold load. Measure images and ads separately. Mobile field p75 targets are LCP ≤2.5 s, INP ≤200 ms and CLS ≤0.1. Staging performance budgets are tested on real hosting with realistic fixture volume, a cold/hot cache, and recorded device/network/load conditions. Target p95 cached TTFB ≤500 ms and search response ≤500 ms under the agreed hosting load benchmark. Define the benchmark concurrency from a measured host baseline before production. Record any actual failure and tune before launch.

## O02 · Access and privacy

Use framework auth, administrator MFA, role-based authorization, CSRF protections, session rotation, scoped cookies and a private admin audit trail. Test that public routes and generated artifacts cannot expose pending items, tokens, raw provider payloads or private contact data. Apply bounded content and rate limits, validated link destinations, output escaping and media size/type controls. Use approved-provider-only outbound requests. No arbitrary URL fetch. Keep secrets in host configuration with restricted access. Backups are encrypted and access-controlled.

Publish operator/contact, Terms, Privacy, Cookies/Preferences, Moderation & Listings, and Report/Takedown pages in released locales. Your Privacy notice must match actual fields, purposes, legal bases, recipients, transfers and retention. Implement access, correction, deletion and objections where applicable, with a case queue, versioned decisions and verified requester handling. Document appeal and takedown routes. Review GDPR territorial applicability, applicable DSA obligations and Azerbaijan registration/transfer requirements with actual operator and target markets. A public link or avatar can still identify a person. Avoid collecting Telegram message content or member identities for discovery.

Proposed retention, subject to owner/privacy approval before production: unconfirmed/rejected submissions and related contact 90 days after closure, ordinary request logs 30 days, security/audit events 180 days unless a documented legal/incident hold requires otherwise. Statistics contain aggregate values, with no member identities. Keep backups on a declared retention schedule and reapply deletion records following restoration. Cookie and device-storage purposes are disclosed. Nonessential analytics or advertising stays off until the configured applicable consent and provider rules are met. Keep a provider inventory and review it on change.

## O03 · Release and recovery

1. CI builds an immutable artifact from the locked revision and records checksums, dependency/test results and migrations. Do not include .env, test fixtures as live content, or dumps.
2. Deploy to staging with production-like web root and Cron. Verify provider permissions with approved sample objects and record unsupported fields and tested fallback behavior.
3. Record database and media backup, and prove restoration in staging. Apply additive/backward-compatible migrations before changing the active application version. Have a maintenance procedure for incompatible schema changes and shared hosting without atomic symlinks.
4. Test the public route, a moderated submission, publish/search/sitemap consistency, urgent takedown, rollback, cron heartbeat, noindex rules, no secret leakage, iOS/Android links and error logs. Use nonproduction data for test writes.
5. Promote with domain and TLS checks, production config, admin MFA and verified operator/legal pages. Verify crawl and sitemap endpoints, Search Console ownership, and advertisements only if enabled. Record verification evidence. Roll back the code artifact and restore cache; restore the database only through the recovery runbook with operator approval and preservation of newer writes/deletions.

Monitor HTTPS/5xx, queue backlog and age, cron last success, Telegram error and mismatch counts, cache/sitemap age, backup age and search coverage. Alerts go to the configured operator. Log identifiers and outcomes, with redaction and retention.

## O04 · Required production inputs

| Input | Owner decision or evidence | Blocks |
| --- | --- | --- |
| Operator identity and jurisdiction | Legal entity or operator name, registration country, public contact, target launch markets and review of legal text/retention | Public production launch |
| Domain and hosting | Registered domain under operator control, shared-host provider/plan and preflight result | Deployment, DNS/TLS and search-property verification |
| Telegram access | Approved operator-owned Bot API credential outside Git, sample permissions and redistribution scope. Optional MTProto policy and evidence if ever enabled. | Live automatic metadata only |
| Editors and editorial seed | Administrator account and MFA, moderation owner, real approved listings in priority topics, reviewed AZ/EN copy | Public production launch |
| Ads and search providers | Verified search properties and operator accounts. Paid creative and consent config where relevant. | External measurements and monetization |

Development proceeds against local configuration and deterministic fixtures. A production launch gate must be marked pass, not applicable with a reason, or pending. An unavailable optional live provider leaves the documented manual or disabled mode in effect.
