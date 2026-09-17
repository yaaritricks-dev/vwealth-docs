# Vault Handoff

Aakhri update: 2026-09-17 (Step 2 owner sign-off; Android foundation complete)

## Abhi kahan hain
Owner ne 2026-09-16 ko complete design lock kiya aur ab Step 2 test karke sign-off de diya. Vault Android foundation complete hai: SDK `/opt/android-sdk`, Kotlin/Compose Gradle project `android/`, application ID `in.noxrelay.vault`, signed debug APK aur Robolectric + Roborazzi JVM screenshot harness with 18 captures. App me abhi intentionally blank themed scaffold hai; koi product screen ya naya design variant nahi.

Owner phone verification complete: signed debug APK install hua, launch hua, crash nahi hua, expected blank screen dikhi; Samsung launcher par Shield V adaptive icon masking sahi dikhi; permissions screen par zero permissions confirm hue. Ye owner-reported physical-phone verification hai, Codex ke direct device inspection ka claim nahi.

Build verification: `assembleDebug` passed; `apksigner verify` passed (v2, one signer); packaged manifest me zero permissions. Native-graphics screenshot suite: 18 passed, zero failures/skips, PNG dimensions aur pixel content checked. Shield V 20/24/48/108dp on light/dark surroundings aur empty scaffolds at 320/360/390/412/430dp widths in both themes. Actual notification/status-bar 20px tint/readability gate abhi separate hai.

Emulator decision FINAL: is host par `/dev/kvm` absent aur CPU `vmx`/`svm` flags absent hain; KVM available nahi. Is host par emulator verification use nahi hogi aur emulator/system image/AVD kabhi install nahi karna. Approved screenshot verification Robolectric + Roborazzi se JVM par hogi. Is decision ko pending feasibility/provider question ki tarah reopen mat karna.

Debug signing: keystore `/root/vault-keys/vault-debug.keystore`, mode 600, repo ke bahar. External `/root/vault-keys/local.properties` bhi mode 600 hai; usme keystore path aur signing credentials hain. Build us file ka path `VAULT_SIGNING_PROPERTIES` environment variable se leta hai; keystore path build script me hardcoded nahi. Gradle execution-history cache bhi repo ke bahar hai. Is handoff me location/commands owner-authorized documentation hain; password, private key aur signing properties file kabhi commit/public copy nahi karni. Release signing key abhi nahi bani; wo alag explicitly approved step hai.

ROADMAP.md authoritative hai. Exact Shield V SVG, approved five-tab design, shared Quick Add, bubble/widget requirements aur security gates unchanged hain. Native tax replacement sirf yearly CA CSV export hai. Existing preview aur deployed old web/Worker behavior untouched hain.

## Working tree
Owner ne Step 2 sign-off ke baad following commit aur dono existing repos par non-force push authorize kiya:
`feat(android): Vault Kotlin foundation, signed debug harness, Roborazzi screenshots`

Private commit scope: Android source, Gradle wrapper/configuration/version catalog, .gitignore, AGENTS.md, STATUS.md, HANDOFF.md aur ROADMAP.md. APK/PNG outputs, local.properties, keystores, build directories, Gradle caches, real data/backups aur credentials excluded hain.
Public docs repo ki apni separate history me sirf ROADMAP.md aur HANDOFF.md jayengi; private history/code/assets/STATUS kabhi nahi. Existing repo-scoped deploy keys use karo; force push nahi.
Completion gate: dono worktrees clean, fetched origin/main == local HEAD, public HEAD exactly two allowed files, aur outgoing commit artifact/secret checks pass. Exact commit hashes final Git verification/report se lo; self-referential hash is file me embed nahi hai.

## Agla kaam
Step 3 — approved design system (theme/colors/typography), paanch-tab navigation shell, Home aur shared Quick Add. Existing locked design implement karna hai; naya design round, screen variant, scope change ya estimate change nahi.

Layout/screenshot iteration Robolectric + Roborazzi native graphics se JVM par hogi. Samsung-specific overlays, actual fingerprint/StrongBox, haptics, 120Hz/frame pacing aur OEM battery/background behavior owner phones par verify honge. Step 2 phone checks ko in remaining gates ka sign-off mat samajhna.

Existing repos, Worker/bucket/hostnames, old Rust app, production DB/backups aur unrelated projects unchanged rahenge. Extra permissions, weaker encryption, new infrastructure aur release signing ko implicit approval nahi hai.

### Regenerate commands
Har Gradle invocation `gradle-safe` se: systemd scope MemoryMax=6G, CPUWeight=50, nice=10; Gradle 3g heap, Kotlin 1536m heap, two workers, parallel=false. SDK selection ignored `android/local.properties` me hi rahe; system-wide SDK environment settings nahi.

```sh
cd /opt/vwealth/android

# Signed debug APK
VAULT_SIGNING_PROPERTIES=/root/vault-keys/local.properties ./gradle-safe :app:assembleDebug

# Regenerate all 18 native JVM PNG captures
VAULT_SIGNING_PROPERTIES=/root/vault-keys/local.properties ./gradle-safe :app:recordRoborazziDebug --rerun-tasks

# Verify APK signature
/opt/android-sdk/build-tools/37.0.0/apksigner verify --verbose /opt/vwealth/android/app/build/outputs/apk/debug/app-debug.apk
```

APK output: `/opt/vwealth/android/app/build/outputs/apk/debug/app-debug.apk`.
Screenshot output directory: `/opt/vwealth/android/app/build/outputs/roborazzi/`.
Without external signing properties, the debug build stays unsigned; no fallback key is generated. Release signing remains disabled.

## Locked native capture specification (read with ROADMAP)
Bubble reference: https://claude.ai/artifact/UCpKpBfyWPQcBVExuNydhA
- Persistent Shield V overlay; special permission only draw over other apps.
- Drag anywhere; nearest-edge snap on release; position remembered.
- Idle half-tuck + reduced opacity, full on touch.
- Tap shared Quick Add; long-press last shortcut with keypad ready.
- Drop on bottom ✕ zone: snooze 1 hour.
- Back dismisses only sheet; underlying app stays, Vault dashboard does not launch.
- Auto-hide in full-screen video/game/camera, subject to privacy-safe spike verification. No Usage Access/Accessibility/foreground app inspection.
- Settings: on/off; S/M/L; idle opacity; edge tuck; instant save default OFF + 3-second Undo; hide-full-screen toggle.
- Sheet uses app's toggle/amount/shortcuts/category/account/keypad/Save and exact validation. No mode toast; no message covers Save. Existing anti-double-save and frozen crypto INR rules retained.

Widget/tile reference: https://claude.ai/artifact/DFnJ9NJqBtxBYXk1QTbkXt
- 4x2 default: net worth, 3 owner-selected shortcuts, Add.
- 2x2 pair: net worth + small chart; today's spend + Add.
- 4x1: monthly income/spend + Add.
- No hardcoded shortcuts; widget shortcut goes directly to capture sheet, no main-app launch.
- QS tile directly opens Quick Add; fingerprint only for full app, not capture. This does not authorize bypassing encrypted-vault/keyguard protections; locked/rebooted capture design is a spike gate.
Both artifact contents could not be fetched in this documentation pass; URLs/owner text preserved, no claim of visual inspection.

CA export: year + full data CSV only; explicit date boundaries, exact amounts/crypto, CSV formula/escaping tests. Locally generated plaintext export only on owner's explicit action, not a server-readable vault or unencrypted backup pipeline.

## Approved design invariants
Light default; Settings Light/Dark/System persisted. Fixed raised five-tab nav. Floating Add only Home/Ledger, 160px end spacer; no competing Add on entity/form/primary-action pages. Entity pages share month/lifetime/trend/filter/average/largest/entries layout. Liability has outstanding/paid/EMI/conditional payoff/payment history; current prototype estimate is principal-only, not a production amortization engine.
Quick Add has fixed visible keypad/Save, compact side-by-side crypto rate/value, exact quantities/frozen INR, no mode toast, inline messages, post-save Undo. Expense terracotta and Income green only in toggle/amount; Save always green. No unapproved screen redesign.

## Din 1 - Worker + R2 ka poora plan

### Owner ko Cloudflare par kya setup karna hai
1. Cloudflare dashboard me login.
2. Billing me payment method add karna.
3. Workers & Pages me Workers Paid plan activate karna (minimum $5/month).
4. Storage & Databases > R2 > Overview me R2 subscription checkout complete karna.
5. Private R2 Standard bucket banana, suggested name `vwealth-vault-prod`; APAC location hint; public access/custom public domain OFF.
6. `vault/versions/` prefix ke liye 90-day lifecycle deletion rule rakhna.
7. Production sync hostname choose karna, jaise `sync.<owner-domain>`; Worker origin ke liye Custom Domain use hoga.
8. Interactive Cloudflare Access sync API par nahi lagana. Admin/enrollment surface owner-only Access ke peeche hogi; `/v1/*` Android background API device-key signatures use karegi. APK me Cloudflare service token nahi jayega.
9. Short-lived scoped API token banana: Workers Scripts Write/Edit, Workers R2 Storage Write, Account Settings Read, aur selected zone par Workers Routes Write; ideally 24-48 hour expiry aur VPS IP restriction.
10. Token chat/git/output me paste nahi karna. Next session ek hidden-prompt command dega jo use VPS par repo ke bahar root-only secret file/environment me rakhega.

### Owner se kya chahiye
- Cloudflare Account ID
- Zone ID
- Domain name
- Chosen sync hostname
- R2 bucket name
- Admin Access allow-policy email/identity provider
- Workers Paid aur R2 enabled confirmation
- Scoped short-lived API token, secure VPS injection ke through
- Storage alert ka logical budget/quota
- R2 primary store se alag off-site backup destination ka decision

Global API key, R2 S3 keys, master password, Recovery Kit, DEK ya production financial data nahi chahiye.

### Worker code kahan aur kaise banega
- Directory: `/opt/vwealth/worker/`
- Files: `package.json`, lockfile, `wrangler.jsonc`, `src/index.ts`, `test/`, `README.md`
- TypeScript Worker; Wrangler pinned dev dependency; global install nahi.
- VPS par code/local tests; Wrangler/Miniflare local R2; owner-approved direct production deploy (staging skipped).
- Worker R2 binding use karega; S3 credentials nahi chahiye.
- Existing Rust/Axum V-Wealth untouched rahega jab tak full Vault v1 complete aur owner removal approve nahi karta.

### Endpoint design
- `GET /health`: public minimal service/version health; vault metadata nahi.
- `HEAD /v1/vault`: authenticated current revision, ETag, size, upload time.
- `GET /v1/vault`: authenticated latest encrypted blob; ETag/revision; `Cache-Control: no-store`.
- `PUT /v1/vault`: primary device only; `If-Match` current ETag, first upload par `If-None-Match: *`; revision current+1; stale write `412`; read-only write `403`.
- `GET /v1/vault/versions`: authenticated retained revision metadata.
- `GET /v1/vault/versions/{revision}`: authenticated historical encrypted blob. Client verify/decrypt karega; restore verified old data ko new revision ke roop me upload karega.
- `GET /v1/storage`: authenticated blob/version/image sizes aur budget usage.
- `POST /v1/devices/enroll`, `GET /v1/devices`, `DELETE /v1/devices/{id}`: enrollment/list/revocation.

### Device authentication
- Phone hardware-backed signing key generate karega.
- Worker public key aur `primary`/`read-only` role rakhega.
- Signature method, path, revision, timestamp, request ID aur body SHA-256 cover karegi.
- Write role server enforce karega; UI disable enough nahi.
- Cloudflare service token APK me nahi jayega.

### Write flow
1. Device signature, timestamp/request ID aur role verify.
2. Current pointer + ETag read.
3. `If-Match`/`If-None-Match` aur revision current+1 validate.
4. Immutable revision blob PUT (exact shipped key/canonical signature protocol worker/README.md se lo).
5. `vault/current.blob` current object ko conditional R2 ETag CAS se publish.
6. CAS fail par `412`; current vault unchanged. Orphan object lifecycle se expire hoga.
7. Success par revision + ETag return; client last acknowledged revision store karega.
8. Version objects 90 din baad lifecycle se delete honge.

### Owner-approved implementation decisions
1. Blind Worker ke paas DEK nahi hoga. Worker structure, size/checksum aur device signature verify karta hai; AEAD authentication client decrypt par hogi aur failure par client blob reject karega.
2. R2 me filesystem rename nahi hota. Implemented atomic equivalent: immutable revision object PUT, phir `vault/current.blob` conditional CAS.

### Off-site backup
- Production R2 bucket primary storage hai; wahi off-site backup nahi.
- Separate provider/account ya periodic encrypted offline export chahiye.
- Destination owner choose karega.

### Validation
- Local fake-R2 unit/integration tests.
- Same ETag ke do concurrent PUT: one success, one `412`.
- Stale revision, read-only role, invalid signature/checksum rejection.
- Interrupted upload/pointer failure ke baad old vault readable.
- Historical retrieval, lifecycle, no-store, size limit, storage accounting.
- Full local Miniflare/security/CAS tests aur Wrangler dry-run; staging skip karke separate owner approval ke baad direct production smoke test.

### Original Day 1 time estimate (completed; native estimate nahi)
- Owner setup: 30-60 minutes.
- Scaffold/harness: 1-2 hours.
- Blob/history/storage routes: 3-4 hours.
- CAS/races: 2-3 hours.
- Device auth/enrollment: 3-5 hours.
- Lifecycle/health/failure tests: 2-3 hours.
- Staging deploy/smoke: 1-2 hours.
- Total: 10-16 focused hours; realistically 1-2 full days for A1-grade endpoint.

## Adhoora kuch hai?
Design complete aur owner-approved; Android Step 2 foundation aur limited owner-phone verification signed off. Step 3 design system/nav/Home/Quick Add next hai. Native encryption, recovery, device security aur OS integrations abhi implementation work hain, working prototype unka production proof nahi. Tax/E8 rewrite native task nahi; sirf CA yearly CSV export. Artifact URLs saved but content fetch nahi hua.
Native spike gates: full-screen auto-hide only allowed permission se feasible hai ya nahi; OS-controlled overlay/notification behavior; foreground-service manifest needs; exact 20px system-notification Shield V; no-fingerprint capture with locked/rebooted E2EE vault; widget privacy/redaction. Emulator decision final hai: JVM screenshots use karo, emulator install nahi. Extra permission, weaker encryption ya new infrastructure silently approve mat maan lena.
Off-site backup destination, storage budget, crypto format/KDF/recovery and release APK signing/update decisions abhi pending; debug signing complete hai. Deployment token owner ke app-working milestone tak retained.

## Owner ke pending kaam
- Native spike me surfaced platform/permission/locked-capture decisions, if needed.
- Cloudflare Access remaining setup/Android-safe authentication decision.
- Storage alert budget aur R2 se alag encrypted off-site backup destination.
- Native phone checks: Samsung overlays, actual fingerprint/StrongBox, haptics, 120Hz, battery/background behavior; full app acceptance baad me.
- App working hone ke baad retained deployment token revoke karna.
- Design approval ab pending NAHI hai.

## Session band karne ka niyam
Codex clear karne se pehle ye chaaron sach hone chahiye:
1. Koi kaam beech me nahi chhoda
2. Owner ne test kar liya aur sign-off diya
3. Commit ho gaya, working tree clean
4. STATUS.md aur HANDOFF.md dono update ho gayi
Agar in me se ek bhi nahi hua to clear mat karo.
