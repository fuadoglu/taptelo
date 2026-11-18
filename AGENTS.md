# Taptelo repository instructions

- Read README.md and the relevant docs before changes. Begin with the first unfinished package in docs/06-delivery.md unless the user names another package. Keep one coherent change per PR.
- Product invariants: public discovery without accounts, three distinct catalog types, admin approval before publication, direct HTTPS Telegram links, stable public URLs and one responsive PHP application. Use shared-host-compatible dependencies.
- The relevant specification owns its domain. In a conflict, ADRs in docs/02-architecture.md govern implementation constraints. Report the conflict and update the affected documents in the same change.
- Do not invent provider results, legal identity, translated copy, test outcomes or deployment evidence. A runtime fallback must have an explicit, tested state. Keep real credentials, submitted private data and production dumps out of Git.
- Pin dependencies. Use server-rendered Blade views, progressive enhancement, SQL migrations and deterministic provider fixtures. Add acceptance coverage mapped to the requirement IDs in docs/06-delivery.md. Run the checks affected by the change and record what was actually run.
- Treat changes to public URL contracts, identity, consent, legal removal or deployment prerequisites as explicit design changes. Update the corresponding spec, tests and rollback notes together.
- Do not publish a production deployment or enable a paid provider without the applicable release inputs. Local tests and committed fixtures can proceed while those inputs are pending.
