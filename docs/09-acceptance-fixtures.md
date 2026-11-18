# Acceptance fixtures and evidence

## Search contract

Use [search.json](fixtures/search.json) in T04 as a versioned regression corpus. Identifiers are test keys and usernames are inert fixtures. Tests stub integrations and use the frozen clock. Published fixture entries have fresh manual verification and valid approved content unless an individual test overrides this. Import the listed fields through normal test builders and use the locale and filter state in each case. Never publish these records in production.

Ordered prefix means each listed ID appears in that exact order at the head of the organic results. Excluded IDs never appear in the organic result group. Exact result is a separate pinned card. Filters retained means the original type, country and language values survive the request, including when a pinned result is of another type. Inferred objects must be returned as removable chips and used as filters. Compare accent and case-insensitive aliases after a consistent Unicode normalization policy, while preserving original display text. The fixture lists examples, so the search index must normalize both stored aliases and incoming queries. Use a bounded Damerau-Levenshtein or equivalent transposition-capable comparison for candidate reranking. A fallback count is the number of results after the specified one-filter change. Do not alter the current filter unless the user selects the fallback.

The fixture tests measure expected outputs against the implementation. Validate these labels with real moderator-reviewed catalog examples before launch and extend coverage for target locales as they open. Relevance metrics are measured only after a corpus exists.

## Requirement traceability

| Package | Required test IDs and contract |
| --- | --- |
| T01 | Foundation commands and HTTP checks in [07](07-foundation-acceptance.md). |
| T02 | QA-03 schema uniqueness subset and PUB-05 revision isolation at the data boundary. |
| T03 | QA-03 concurrent review, QA-04 privacy and PUB-05/PUB-06 lifecycle outcomes. Run with stubbed identity providers. |
| T04 | QA-01, QA-02 and QA-07. SEARCH-01…09, responsive atlas and browser checks. |
| T05 | QA-05 and PUB-01…04. Advance a frozen clock through due/grace boundaries and simulate provider error and mismatch. |
| T06 | QA-06, QA-08 and all PUB cases through HTTP, current sitemap and browse. Use warm origin caches for PUB-03 and PUB-06. |
| T07 | QA-09 and privacy request processing including the application part of QA-11. Verify ineligible ad contexts by inspecting the rendered response. |
| T08 | QA-10…12 on staging. Actual restoration, repeat deployment and measured hosting acceptance. Redeploy the immutable artifact before validating recovery. |

## UX checkpoints

The [responsive wireframe atlas](10-wireframe-atlas.html) defines layout and visible states. It contains synthetic examples. Acceptance uses real implementation routes once available.

| ID | Input | Required observation |
| --- | --- | --- |
| UI-01 | 390 × 844 browse, normal text, four filters available | Search, tabs, current scope and at least the first organic card appear before scroll. 320 px has no horizontal overflow. |
| UI-02 | No matching English content, 2 Azerbaijani/Russian matches after language reset | Preserve current selection. Offer removal of the single limiting filter with the accurate count. |
| UI-03 | Open filters, select language, press Escape or cancel, reopen, apply | Focus returns to trigger. Cancel leaves prior state. Apply changes URL and results. Count describes draft filter state. |
| UI-04 | Telegram enrichment disabled or times out | Valid public link remains in the form. Manual review is available and submission stays private. |
| UI-05 | Moderation verifies pending profile then approves it | The prior preview stays private until commit. Public profile, listing visibility, sitemap and audit history reflect the appropriate lifecycle state. |
| UI-06 | Open a profile from results, use browser back on the same device | Restore filters and list position. Catalog profile and Telegram CTA remain separate accessible actions. |

For each implementation PR, record commit, package, linked IDs, actual test commands and results, staging gates not yet verified and the next dependency. Do not use a checklist tick as a substitute for execution evidence.
