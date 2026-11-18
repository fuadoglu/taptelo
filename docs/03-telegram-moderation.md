# Telegram and moderation

## TG01 · Submission and identity

Accept a syntactically valid public @username, t.me/username or https://t.me/username. Normalize once on the server. Reconstruct the destination from validated parts. Reject private invites, individual message URLs, other hosts, app folders, unsupported parameters and personal profiles. A bot start parameter can be configured only on a reviewable bot revision and must match Telegram's documented limits. Use the direct https://t.me link from an HTML anchor. Test app handoff and browser fallback with real Android Chrome, iOS Safari and desktop browsers. A click records a click, not Telegram membership or a bot start.

Check an existing normalized username and known provider identity. If published show the profile and suggest correction, if pending give a generic duplicate state without leaking another person's submission token. An unavailable or failed preview still permits a manual review submission for syntactically valid links. The submitter sets category, content language and audience scope, with explicit unknown states where appropriate. Server generates random opaque status tokens and stores their digests. Contact is optional. Status pages use no-store and a no-referrer policy.

## TG02 · Provider capability matrix

| Capability | Channel/public group | Bot |
| --- | --- | --- |
| Public identity, title, photo | Bot API getChat where permitted and verified in a capability test. Record Telegram ID and chat type. | Bot API has no guaranteed arbitrary bot-by-username lookup. MTProto contacts.resolveUsername is a candidate capability only after terms, permission and shared-host testing. Admin can verify public destination and function manually. |
| Audience | getChatMemberCount where available. Save a dated observation. | Monthly active users only if an approved provider actually returns bot_active_users. Missing data is unknown. |
| Functional checks | Link and identity. Editorial recency may be recorded explicitly. | Admin records whether the described function actually responds. Link health is a separate fact. |
| Owner verification | Optional separate step with documented proof and review. | Separate ownership proof. Do not request the external bot token. |

Provider baseline is HTTP Bot API for approved channel/group metadata plus manual admin workflow for unsupported cases. Record which fields and sources may be displayed, refreshed and retained. Enable the automated provider only after the terms and sample checks in operations. MTProto is off by default and is not required for baseline operation. Use admin-supplied display metadata and an initials fallback when photo retrieval or redistribution is unavailable. Where permitted, proxy/cache only approved image assets on the server and never expose a provider token. No bulk Telegram crawl, message archive, member lists or auto-joining chats.

## TG03 · Moderation and ongoing health

The [publication contract](08-publication-contract.md) defines the overdue and grace boundaries, and the public outcomes of these decisions.

1. Place submissions in pending with preview provenance and duplicate signals. Rate-limit public submission and report endpoints. Allow editor corrections but require publish authority to approve.
2. Revalidate destination identity before approval. Record whether identity was API-confirmed or manually observed. Manual-only entries enter a dated manual review queue and cannot be marked automatically API-verified.
3. Link and image checks run on a schedule. Start with 24 h for prominent listings and 72 h for the rest, plus operator-triggered priority checks. Measure volume/time/rate limits before increasing coverage.
4. Timeout, rate limit and temporary provider failure preserve the last known good record and enter backoff. An identity mismatch disables public outbound links immediately and suspends the listing from search and index eligibility pending review. A confirmed permanent removal requires evidence or a valid takedown. Never infer deletion from one timeout.
5. A known identical ID with a new verified username updates the destination. If the provider cannot recover the new username, send to manual review. Never assign a new Telegram identity to an existing published URL simply because the username matches.
6. Record public reports and removal decisions, make urgent takedowns effective across origin/cache/list/search and keep a time-limited audit record. Publisher/admin badges, link validity and bot functional reviews have distinct labels.

Metric history: store the source, sampled_at and nullable numeric value. Show 7-day and 30-day deltas only when comparable observations exist within a documented sampling tolerance. Null never becomes 0. Detailed privacy-sensitive analytics stay outside the public catalog.

Official references: [Deep links](https://core.telegram.org/api/links) · [Bot API](https://core.telegram.org/bots/api) · [Username resolution](https://core.telegram.org/method/contacts.resolveUsername) · [Bot user fields](https://core.telegram.org/constructor/user) · [Bot platform terms](https://telegram.org/tos/bot-developers) · [Content licensing](https://telegram.org/tos/content-licensing)
