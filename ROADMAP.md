# V-Wealth roadmap

Last updated: 2026-09-19

## Direction

V-Wealth is now a web-only product. The current Rust and React application remains the active product and will continue to receive focused performance, reliability and feature improvements.

Android is archived and has no active milestones, releases or delivery gates.

Android floating bubble / native capture app DROPPED - 19 Sept 2026, owner decision, web-only.

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

Android-era sync infrastructure retired.

## Next work

1. Improve operational resilience.
2. Continue privacy, dependency and reliability hardening.
3. Make startup and offline return-to-app behavior feel instant.
4. Add product features in owner-approved priority order.
5. Preserve the current visual language and strengthen isolated regression coverage with every change.

## Publication rule

The public documentation repository contains only `ROADMAP.md` and `HANDOFF.md`. Keep that exact two-file allowlist and do not force-push.
