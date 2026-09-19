# V-Wealth handoff

Last updated: 2026-09-19

## Product direction

V-Wealth is a private, single-user web app with a Rust backend and React frontend. The active direction is to keep improving the web product without an unapproved visual redesign.

Android is archived and is not an active product, fallback plan or pending delivery.

Android floating bubble / native capture app DROPPED - 19 Sept 2026, owner decision, web-only.

## Current product

The live app includes:

- Overview and analytics
- Ledger and filtering
- Quick Add for income and spending
- Bank, cash and crypto holdings
- Transfers and crypto conversion
- Targets and progress tracking
- Tax estimates
- AI-assisted insights
- CSV export
- Settings, recovery and Trash/restore flows

Liabilities and EMI tracking are not part of the current live web app.

## 19 September progress

Owner approved both Quick Add performance rounds on the target phone:

- Faster keypad registration and isolated amount rendering
- Durable optimistic fiat saving with retry-safe Undo
- Deferred dashboard refresh work
- Smaller initial app payload through lazy loading
- Immediate, independent multi-touch keypad press feedback
- Pixel-identical idle Quick Add appearance in light and dark themes
- Encrypted off-site daily backup active

Frontend and backend regression suites passed after the work.

## Next direction

Next work should proceed in this order:

1. Operational resilience.
2. Privacy and reliability hardening.
3. Faster startup and a stronger offline experience.
4. Owner-prioritized product features.

All work should preserve exact money and crypto behavior, the current approved visual language and isolated test practices.

## Publication rule

The public documentation repository contains only `HANDOFF.md` and `ROADMAP.md`. Keep that exact two-file allowlist and do not force-push.
