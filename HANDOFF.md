# V-Wealth handoff

Last updated: 2026-09-26

## Product direction

V-Wealth is a private, single-user web app with a Rust backend and React frontend. The owner approved a full visual redesign on 26 Sept 2026; money and data behaviour stay exact.

The web app is the active product. A thin Android companion app (26 Sept 2026) opens it full screen and adds a floating quick-add bubble, a home-screen widget, app shortcuts, a quick settings tile and share-to-app. It keeps no data of its own and can be downloaded only while signed in. Tapping the bubble opens a mini Quick add window over any app (bank, cash or crypto such as USDT, valued at the live price); the bubble has its own settings, stays away from banking and payment apps, keeps entries made offline until the phone is back online, and holds an add-only link that can never read an amount and can be unlinked from Vault. The app also sends the optional daily reminder as its own notification, without web push.

## Current product

20 Sep 2026: Hero numbers on Overview and Assets now show final values directly. Quick Add fixes shipped: warning sheets stay open, no automatic keyboard, accurate offline indicator. Vault shows a build ID for phone testing.

Crypto entry reliability improved across the app. Repeated attempts after a failed save no longer create duplicate entries.

Pending entries sync more reliably when you return to the app.

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
- Local backups encrypted
- Security hardening pass done
- Instant offline start done

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

26 Sept 2026 (later): shared text opens Quick add pre-filled, an entry can be split in two, the ledger finds entries by amount, merchants and payers show a daily trend, Overview flags a missing daily payout in the evening and recaps the last month early in the month, Reports and Tax stay readable offline, locking warns about unsent entries, and gold sales count as sales rather than income.

Frontend and backend regression suites passed after the work.

## Next direction

Next work should proceed in this order:

1. Operational resilience.
2. Privacy and reliability hardening.
3. Faster startup and a stronger offline experience.
4. Owner-prioritized product features.

All work should preserve exact money and crypto behavior, the approved redesign and isolated test practices.

## Publication rule

The public documentation repository contains only `HANDOFF.md` and `ROADMAP.md`. Keep that exact two-file allowlist and do not force-push.
