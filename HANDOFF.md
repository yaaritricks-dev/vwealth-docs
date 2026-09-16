# Vault Handoff

Aakhri update: 2026-09-16 (design sign-off / specification lock)

## Abhi kahan hain
Owner ne complete design approve kar diya. Naya native Kotlin/Compose app "Vault" hoga; old web "V-Wealth" naam/tax ke saath abhi untouched. Code/prototype branding is task me rename nahi ki. App icon identity "Shield V": exact canonical SVG ROADMAP.md me, dark ink #0d1409 + lime #a8e05f, shield 42%, V full; app/splash/notification/bubble, readable at 20px.
Native tax removed: owner says "wo CA ka kaam hai". Replacement only "CA ke liye export": select year, full-year CSV. Old web tax stays; prior E8-native-rewrite instructions superseded.
Approved preview source prototype/, Worker ab9e9013-34a1-4bf9-97e1-d3f7580af483 at private preview.noxrelay.in. Last checks: build + 24 unit cases, live 380 responsive cases (36 screens/5 widths/2 themes + USDT), 33-screen/99-state regression, capture/Save-hit-test and liability suites pass. No Kotlin or emulator installation yet. Preview/old web deployed behavior is unchanged by this docs task.
ROADMAP.md is authoritative. Exact bubble/widget specs, SVG, reference links and native feasibility gates are recorded there; short carry-forward below.

## Working tree
Owner authorized the complete approved prototype and documentation commit/push to both existing repos with message:
docs: lock design, rename to Vault, add bubble and widget specs
Private commit includes prototype source/tests/lockfile and ROADMAP.md, STATUS.md, HANDOFF.md; ignored screenshots/builds/node_modules, real data/backups and credentials never included.
This is the design-lock commit snapshot; resolve its exact hash from git log -1 --oneline (self-referential hash is not embedded in its own file). Parent before design lock: 5130a72 docs: finalize worker deployment handoff.
Public docs repo remains an independent history with ONLY ROADMAP.md and HANDOFF.md, no STATUS/code/assets. Each repo uses its existing dedicated deploy key. No force push or repo rename.
Handoff completion requires both worktrees clean and fetched origin/main matching local HEAD; verify with Git rather than trusting older chat status.

## Agla kaam
Vault native foundation / Compose interaction spike, not another full design round. First recheck emulator/KVM viability, Android build SDK/JDK/Gradle targets and permission/security gates; then use the approved screens and shared Quick Add.
Read-only host check: /dev/kvm absent on 2026-09-16. Do not claim the VPS emulator works until acceleration/boot/screenshot tests pass. Do not install/reboot/change production services without the relevant task authorization.
Owner requested emulator screenshots to reduce phone-review waiting. Real Samsung overlays/fingerprint/StrongBox/haptics/120Hz still require owner phones.
Repos, Worker names, bucket, hostnames and old running Rust app remain unchanged. Only noxrelay.in Cloudflare resources are in scope; unrelated domains/Worker forbidden. Production DB/backups untouched.

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
Design complete aur owner-approved; is docs-recording task me native build/emulator install shuru nahi hua. Native encryption, recovery, device security aur OS integrations abhi implementation work hain, working prototype unka production proof nahi. Tax/E8 rewrite native task nahi; sirf CA yearly CSV export. Artifact URLs saved but content fetch nahi hua.
Native spike gates: full-screen auto-hide only allowed permission se feasible hai ya nahi; OS-controlled overlay/notification behavior; foreground-service manifest needs; exact 20px Shield V; no-fingerprint capture with locked/rebooted E2EE vault; widget privacy/redaction; VPS KVM unavailable. Extra permission, weaker encryption ya new infrastructure silently approve mat maan lena.
Off-site backup destination, storage budget, crypto format/KDF/recovery and APK signing/update decisions abhi pending. Deployment token owner ke app-working milestone tak retained.

## Owner ke pending kaam
- Native spike me surfaced platform/permission/locked-capture decisions, if needed.
- VPS emulator ke liye virtualization/provider/alternative runner direction if current host cannot support it.
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
