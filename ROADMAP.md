# V-Wealth roadmap

Last updated: 2026-09-26

## Direction

V-Wealth is now a web-only product. The current Rust and React application remains the active product and will continue to receive focused performance, reliability and feature improvements.

The web app is the active product. An Android capture companion is planned as a later phase.

Android capture app with a floating bubble: dropped on 19 Sept, revived by the owner on 26 Sept 2026 as a later phase.

## Current live scope

The live web app currently provides:

- Overview analytics and net-worth tracking
- Ledger search and filters
- Fast income and spending capture
- Bank, cash and crypto holdings
- Transfers and crypto conversion
- Targets and progress tracking
- Tax estimates
- AI-assisted insights
- CSV export
- Settings, recovery and Trash/restore flows

Liabilities and EMI tracking are not current live features.

## Completed performance milestone

The owner approved two Quick Add performance rounds on the target phone:

- Instant keypad registration and isolated amount updates
- Optimistic, retry-safe fiat saving
- Deferred non-critical refresh work
- Lazy loading for heavier application code
- Direct per-pointer keypad feedback with independent multi-touch behavior
- Preserved light and dark visual output

Encrypted off-site daily backup active.

Local backups encrypted.

Security hardening pass done.

Instant offline start done.

Digital Gold v1 live

Recurring entries v1 live

Daily reminder + app shortcuts live

Budgets + Reports live

Milestones, Wrapped, budget pace live

Android-era sync infrastructure retired.

26 Sept 2026 redesign live: calmer light and dark themes, glass header and dock, one-hand Quick add, smooth selection and screen motion, own pull-to-refresh, keyboard-safe layouts, and redesigned Overview, Ledger, Assets, Vault, Reports, Tax and AI screens.

Merchants and payers: picking one in Quick add fills the category or source and the usual account, with totals per merchant for the month, six months, a year and all time, a money-flow view of where income came from and where it went, and Ledger search across merchant, category and account names.

Recurring payments explain when an automatic payment could not run, and rules that could never run are refused.

Quick add keeps its Confirm button above the navigation on every screen size and font size, and offers merchants, payers, categories and sources in one smart row.

Crypto entry reliability — shipped.

Offline sync reliability — shipped.

## Next work

Next: Quick Add sync reliability, keypad gesture handling, then polish slices S2 to S6.

1. Improve operational resilience.
2. Continue privacy, dependency and reliability hardening.
3. Make startup and offline return-to-app behavior feel instant.
4. Add product features in owner-approved priority order.
5. Keep the new visual language consistent and strengthen isolated regression coverage with every change.

## Publication rule

The public documentation repository contains only `ROADMAP.md` and `HANDOFF.md`. Keep that exact two-file allowlist and do not force-push.
