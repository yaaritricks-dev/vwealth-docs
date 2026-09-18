# V-Wealth roadmap

Last updated: 2026-09-18

## Direction

Owner ne 2026-09-18 ko naya Vault Kotlin/Compose Android app band karne ka faisla liya. Android implementation ab active product, fallback plan ya pending roadmap nahi hai. Aage ka product V-Wealth hi hai: existing Rust/Axum backend aur React/Vite PWA ko upgrade, polish aur app-like experience me improve kiya jayega.

Android ka final source aur history private repository ki branch `archive/android-kotlin` aur tag `android-v0.5.3-archive` par preserved hai. Archive point commit `122e160998c402cbe2d4e48fa8ec15bf88a16fd7` hai. Main branch par `android/` aur Android-only build/config instructions nahi rahenge. `/opt/android-sdk` aur `/root/vault-keys/` is decision ka hissa nahi hain aur untouched rahenge jab tak owner alag faisla na de.

## Android archive record

Archive hone tak native app me Kotlin/Compose foundation, Vault design system, five-tab navigation with Option B indicator, Home, Quick Add, Ledger, persisted local storage, Accounts, liabilities, transfers, reconciliation, crypto conversion/detail, shared entity detail, Reports charts/heatmap/breakdowns, recurring schedules, Pending inbox, goals, management/archive/trash flows, search, CA CSV export, settings shells, motion polish, tap feedback, predictive back, haptics, Robolectric/Roborazzi coverage aur real-user-style JVM walkthrough ban chuke the. Batch B aur Batch C ko owner ne phone par accept kiya tha; final motion build ka host-JVM verification hua tha. Security/encryption, biometric unlock aur production sync complete nahi hue the aur ab koi Android pending item nahi hai.

## Active product: V-Wealth

Current foundation:

- Rust 2021, Axum and SQLite backend in `src/`.
- React 18, Vite and Tailwind frontend in `ui/`; generated production build in `dist/`.
- Single-user PWA served by the Rust process, bound privately to `127.0.0.1:8791` and exposed only through its dedicated Cloudflare tunnel.
- Integer-paise money arithmetic, exact crypto quantities and frozen INR-at-receipt semantics remain mandatory.
- Production data/config in `data/` and `.env` remain private and outside test scope.
- `worker/` remains preserved as V-Wealth infrastructure; no removal or production change is implied by the Android archive.
- `prototype/` remains the approved design reference and must not be deleted.

## Next work

1. Audit the current V-Wealth PWA against the approved `prototype/` and prioritize gaps without an unapproved redesign.
2. Improve phone usability, responsiveness, installability and app-like navigation while keeping the current Rust/Axum deployment model.
3. Polish existing ledger, accounts, assets, liabilities, analytics, targets, tax-estimate and AI-provider flows.
4. Preserve security boundaries: server-side keys, Argon2id/TOTP/session protections, private bind and tunnel-only exposure.
5. Add or strengthen isolated backend/frontend regression coverage before deployment; never test against live `data/`.
6. Use `scripts/deploy.sh` only with explicit deployment authorization and retain the backup-first deployment flow.

There are no active Android milestones, APK deliveries, Gradle tasks, encryption steps, device-sync steps or Android release gates in this roadmap.

## Repository and publication rules

The complete source remains in the private `yaaritricks-dev/vwealth` repository. The public `yaaritricks-dev/vwealth-docs` repository contains only `ROADMAP.md` and `HANDOFF.md`; keep that exact two-file allowlist and independent history. Do not force-push either repository. Git commits and production/service changes still require explicit owner authorization.
