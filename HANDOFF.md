# V-Wealth handoff

Last updated: 2026-09-18

## Owner decision

Owner ne 2026-09-18 ko Vault Kotlin/Compose Android app band kar diya. Aage sirf existing V-Wealth Rust/Axum + React PWA ko upgrade aur polish karke app-like product banana hai. Android ke liye koi active implementation, APK, encryption, biometric, sync ya release task pending nahi hai.

## Android archive

Final Android state private repository me preserved hai:

- Branch: `archive/android-kotlin`
- Tag: `android-v0.5.3-archive`
- Archive commit: `122e160998c402cbe2d4e48fa8ec15bf88a16fd7`
- Archive commit message: `feat(android): batch B, batch C, motion polish and nav option B`

Archive point tak Kotlin/Compose foundation, Vault design system, five-tab Option B navigation, Home/Quick Add/Ledger, durable local finance storage, accounts/liabilities/transfers/reconcile/crypto flows, shared entity detail, Reports/heatmap/breakdowns, recurring/Pending/goals, management/archive/trash/search, CA CSV export, settings shells, motion/tap feedback/predictive back/haptics aur extensive Robolectric/Roborazzi/JVM walkthrough coverage ban chuke the. Batch B/C owner-phone accepted the; motion polish host-JVM verified tha. Native encryption, biometric unlock aur production sync complete nahi hue. Main branch se Android source aur Android-only repository instructions hata diye gaye; history branch/tag me recoverable hai.

`/opt/android-sdk` aur `/root/vault-keys/` ko nahi chhedna. `/opt/noxyaari` aur uski services V-Wealth repository work se alag hain aur untouched rehni chahiye.

## Active system

V-Wealth ka active source and runtime:

- `src/`: Rust/Axum API, authentication, analytics, assets, prices, targets, tax estimate and AI-provider modules.
- `ui/`: React/Vite/Tailwind frontend source.
- `dist/`: generated production frontend; hand-edit nahi karna.
- `worker/`: preserved Cloudflare Worker code.
- `web/`: obsolete but intentionally retained legacy frontend.
- `prototype/`: approved design reference; retain it.
- `scripts/`: backup, restore, regression, end-to-end and deployment tooling.
- `data/` and `.env`: sensitive production state/config; inspect or mutate only when a task explicitly requires it.
- `bin/vwealth`: deployed binary.

Production shape remains one private single-user PWA and one Rust process. Backend `127.0.0.1:8791` par bind hota hai aur dedicated Cloudflare tunnel se hi public route milta hai. Money integer paise me, crypto exact 1e-8 units me aur received income ka INR value frozen rehta hai. Tax output estimate label ke saath hi rehna chahiye; AI keys server-side rahengi.

## Next direction

Next work V-Wealth upgrade/polish hai: approved `prototype/` ko reference bana kar current PWA gaps audit karo, phone UX aur app-like behavior improve karo, existing finance flows polish karo, security/data invariants preserve karo aur isolated tests se verify karo. UI screen redesign bina explicit owner permission ke mat karna. Production deployment, tunnel/firewall/service change ya live database access ko is archive decision se authorization nahi milti.

## Repository rules

Private source remote `yaaritricks-dev/vwealth` hai. Public docs remote `yaaritricks-dev/vwealth-docs` hai aur usme sirf `ROADMAP.md` aur `HANDOFF.md` allowed hain. Public docs ki history independent rakho, exact two-file allowlist verify karo aur force-push mat karo.
