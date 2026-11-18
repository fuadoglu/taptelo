# Product and UX

## P01 · Scope and users

Taptelo is a public catalog of independently moderated Telegram channels, public groups and bots. It supports four jobs: find a known object, find something by purpose and locality, open it in Telegram, or submit an object for review. v1 includes all three catalog types, link reports, saved items, content freshness, search, legal workflows and direct sponsorship. No public account is required. Saved items live on the current device. Admins and moderators authenticate separately. Private invites and personal Telegram profiles are outside the launch catalog.

Start editorial coverage with Azerbaijani job listings, rentals, education, local communities, deals and practical bots. A deployed market needs real approved, usable entries. Fixture-only listings are limited to local and staging environments. Do not claim coverage or announce partnerships without evidence.

## P02 · Locality and language

| Dimension | Meaning | Precedence |
| --- | --- | --- |
| Interface locale | UI, editorial copy and legal copy | Explicit localized URL, then saved manual preference on neutral entry, then browser language, then EN |
| Country scope | Audience geography shown in a browse view | Explicit route/filter, then saved manual choice on neutral entry, then trusted approximate country, then Global |
| Content language | Language of posts, group conversation or bot interaction | Explicit filter, then All languages |
| City | Audience geography where applicable | Explicit choice or a visible suggestion parsed from the query |

Country and content language are many-to-many listing attributes, with explicit unknown and international states. A target country describes intended relevance, not measured member geography. Global shows all public eligible entries. A guessed country is used only on the neutral entry when it has sufficient inventory. AZ and EN are release locales. TR and RU are configured extension targets and become public after translated UX, editorial pages and legal copy pass review. Country and content-language filters can cover regions for which the interface is unavailable. Use stable locale URLs. The neutral entry can suggest or activate initial preferences while exposing standard links to all released locales. Shared/public URLs render the same content for the same parameters. Store manual preferences in first-party device storage and document their use.

## P03 · Discovery and taxonomy

Each type has its own two-level taxonomy, one primary category per published profile and optional controlled topic labels. Categories have stable keys, localized labels, locale-specific aliases, a parent and publication status. Editors can merge a duplicate category with a route redirect. An object can additionally link to related profiles through a typed, editorially approved relationship.

Classify exact username/link lookup ahead of text search. An exact result can cross object type and geographic scope. Display its actual type. For generic queries keep the selected type and filters. Search tiers: exact title, title prefix, weighted tokens/aliases, bounded typo matching. Resolve city and category only with a high-confidence alias match and show removable chips. Never silently relax filters. Return a concrete fallback for zero results. Exact spelling and original display strings are preserved.

Maintain a reviewed query set in AZ and EN, including Azerbaijani dotted/dotless I, diacritic alternatives, mixed-language terms, abbreviations like IT/İT, Cyrillic place aliases, misspellings, precise ID and failed searches. Limit raw-query retention and redact probable personal information before analytics.

## P04 · Screen contract

| Screen | Mobile | Desktop |
| --- | --- | --- |
| Browse/search | Compact header with country and menu, full-width search, three tabs, filter button and removable chips, one-column results, bottom navigation | Search in header, three tabs, visible filter rail and a wide result list |
| Filters | Bottom sheet with result count and apply action. Back/close returns to prior stable state | Persistent filters with counts and URL updates |
| Profile | Avatar, name, short purpose and Telegram action in the first viewport | Same primary content with optional related profiles beside it |
| Submit | Link first, progressive preview and three choices, inline errors, single-column review | Same form with preview beside it |
| Status/report | Clear state and next available action. Private status links are non-indexable and non-cacheable | Same semantics |
| Saved | Device-local bookmarks, empty state and a path back to browsing | Same library on the current device |
| Moderation | Authenticated queue, review and publish actions remain usable on mobile | Queue, details and preview side by side where practical |

Color tokens: canvas #F7FAF9, surface #FFFFFF, text #15221F, muted text #52635F, action #006B5B, selected background #E7F5F0, dividers #DCE5E2. Use system typography, visible focus, text plus icon for meaningful states and 44 × 44 CSS pixel primary touch targets. Keep the first result visible at 390 × 844 on a typical browse page. At 320 px, retain usability, allow natural scrolling and avoid horizontal overflow. Test system text enlargement, keyboard navigation, screen reader names, reduced motion and WCAG 2.2 AA. Mobile bottom navigation yields to the keyboard and filter sheet.

Each card shows the avatar or initials, name, one original sentence of purpose, content language and type-specific metrics when available. The name opens the profile. The separately named action opens Telegram. Counts and badges never substitute for the purpose sentence. Preserve query, filters and scroll on browser back. Loading, empty, failed and stale states must leave a useful action available. Core navigation, forms and Telegram links work with JavaScript disabled.
