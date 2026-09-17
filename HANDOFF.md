# Vault Handoff

Aakhri update: 2026-09-17 (Step 4 + Batch A owner phone sign-off; next Batch B)

## Current sign-off and next step (2026-09-17)

Owner tested APK 0.3.1 and signed off Step 4 + Batch A. Step 4 complete: local persistence, calendar date picker, readable dates and Ledger search/filters. Batch A complete: Accounts/opening balances/create-edit/reconcile/transfer/conversion/crypto detail; Liabilities; shared entity detail layout; live net worth.

Owner-reported phone acceptance: partial net worth with `2 accounts not set` and group-wise counts; Assets minus liabilities math; liability dates via calendar instead of free text; repaired Audit icon; entity detail opening plus tabs/filters/average/largest; persistence working. Codex did not directly inspect the physical phone. The historical missing-entry incident's cause was not established; current persistence is now owner-accepted, not evidence of a reconstructed past cause.

Standing owner-approved deviations: Home delta is real net-worth change against a real dated baseline, NEVER the prototype cashflow formula. Keep Money in/out separate; incomplete balances/history hide the full delta row and never invent historical values. Offline crypto keeps `Manual rate · not live`, preserves saved rate/timestamp and gives the approved unavailable-offline refresh response. These are approved decisions, not regressions to revert.

Partial-state rule: known total plus scoped `N accounts not set` / `N liabilities not set`; singular for one; one mixed Home line such as `2 accounts · 1 liability not set`. Unknown is not zero. Entirely unknown groups show `Not set`, complete groups have no missing-count copy. Mandatory pre-APK real-user walkthrough: fresh app, 3–4 UI-created accounts with mixed set/blank balances, liability, current/past entries, transfer/reconcile, stop/reopen and truthful partial/persistence checks. No pre-seeded financial fixture substitutes for the journey. JVM cold relaunch and actual OS force-stop are distinct evidence; light/dark partial screenshots and two consecutive full suites remain mandatory.

NEXT: Batch B — Reports, Subscriptions/Recurring, Targets/Goals. Existing basic report/entity links do not mark full Reports complete. ROADMAP scope and estimates remain unchanged. Encryption/biometric/sync and remaining OS/security gates are still separate later work.

## Accepted Batch A implementation and verification (2026-09-17)

Known-only totals now work across Accounts and Home, including the Accounts shortcut. Unknown balances/outstanding stay null and are counted, never silently set to zero. Exact approved copy uses account/liability nouns; mixed Home example is one line: `2 accounts · 1 liability not set`. Complete groups have no missing-count copy; entirely unknown groups show `Not set`. No liability records means an empty total of zero; an existing unknown liability remains unknown. Partial values do not generate full-worth baselines/deltas.

Liability deduction message is conditional on actual inclusion in Home net worth. Due dates use Quick Add's calendar, future dates enabled only for due dates; optional Clear date; no blank/invalid due label. Schema 3 preserves schema-1/2 data and allows unknown outstanding. Legacy free-text due strings remain stored without being presented as dates. Audit clock's malformed hands were repaired and native bitmap/package checked.

Mandatory APK walkthrough rule is in AGENTS.md. `RealUserWalkthroughTest` creates all financial data through actual MainActivity UI from an empty app: four new accounts with mixed availability, known/unknown liabilities, future due date/clear, current/past entries, transfer, reconciliation, audit and full activity/store teardown/relaunch. No pre-seeded financial fixture/direct repository writes drive it. Every stored entry/balance/debt/audit record survived reopening; All time found the previous-month entry. This JVM restart is not an actual phone OS process force-stop.

Missing-entry incident remains unproven on the owner's phone. Entries and balances use the same app-private database; 0941 APK confirms the same filename. Migrations and cold-reopen tests preserve both. Current-month cashflow/default Ledger filters exclude older months without deleting records. Do not claim reinstall caused the incident or that real lost data was recovered; no phone DB/install-history evidence was available.

Final verification: two back-to-back full offline suites each PASS 1,079/1,079, zero failures/errors/skips, 1,026 unchanged screenshots (330s/324s). All 96 source/config fingerprints match through assembly. Partial-state matrix has 90 cases, both themes and five widths. Standalone lint PASS, zero errors/three existing warnings. Combined initial recording/lint was terminated with exit 143 after tests/captures passed; standalone lint and both final suites succeeded. Full evidence is `/tmp/vault-batch-a-partial/` and STATUS.md.

CURRENT APK: `/opt/vwealth/android/app/build/outputs/apk/debug/vault-debug-0917-1118.apk`; 0.3.1 (5), 30525913 bytes; SHA-256 `c491a7e88fb074351706107b27beb6db2a0328760bdb9c28661ad0abbfdbd680`. Signed assembly passed in 103s. apksigner VERIFIED v2/one signer, same certificate as 0941; aapt2 ZERO requested permissions, package `in.noxrelay.vault`, minSdk 31/targetSdk 37. Corrected clock vector, schema 3 and approved copy are packaged. Owner phone acceptance received for 0.3.1; the current task explicitly authorizes the scoped commit and both non-force pushes. Plaintext test-storage boundary and remaining security gates stay in force.

## Batch A approved decisions and historical 0941 delivery (2026-09-17)

Batch A implementation is present: Accounts/opening balances/create-edit/reconcile/audit/transfer/crypto conversion and details; Liabilities/create-edit/debt detail/principal-only payoff and payments; shared entity details and live Home balances. Final verification/build evidence is recorded in STATUS.md; old 0755 APK is not Batch A. The owner reaffirmed offline crypto copy `Manual rate · not live` and `Market refresh unavailable offline. Saved rate and timestamp unchanged.` No network calls/permissions are authorized; retain the layout and original saved quote/timestamp.

OWNER-APPROVED PROTOTYPE DEVIATION: Home TOTAL NET WORTH must NEVER use the prototype's monthly income-minus-spend formula as its delta. Use current real net worth minus a real dated baseline. Baseline starts when opening balances are set; capture only actual daily observations and never invent past balances or fill missing dates. When history is insufficient hide the entire delta row, including arrow, number and period. Money in/out remains independent. Principal repayment normally preserves net worth because cash and outstanding debt both decrease equally.

Owner REJECTED the proposed daily-snapshot message because it exposed internal jargon and implied an unreliable timing promise. Exact approved replacement, now wired: `Net worth change will appear once there's earlier history to compare`. No pending copy decision remains. Offline crypto and Home option B remain approved. The standing stop-and-ask rule is in AGENTS.md: ask immediately, provide the current-state plain-text report and stop for the owner's answer; never queue questions and continue on assumptions.

Final Batch A verification: two consecutive full offline `:app:verifyRoborazziDebug --rerun-tasks` runs each passed 981/981 tests, zero failures/errors/skips and 934 unchanged screenshots. Wrapper elapsed 237s/227s; all 92 source/config hashes remained unchanged through both runs and signed assembly. Logs `/tmp/vault-batch-a-suite-1.log` and `/tmp/vault-batch-a-suite-2.log`; independent archived XMLs under `/tmp/vault-batch-a-final/run-1/` and `run-2/`. Ten approved-copy Home baselines changed; other 926 PNGs stayed byte-identical.

Historical 0941 phone-test APK: `/opt/vwealth/android/app/build/outputs/apk/debug/vault-debug-0917-0941.apk`, version 0.3.0 (4), 30509537 bytes, SHA-256 `73ed042bfec35a5ac80487c1c87b6b895fe6c2131742729bfb5d5f85c451b2ea`. Assembly passed in 95s with all 36 tasks executed. apksigner VERIFIED, v2, one signer; aapt2 confirms zero requested permissions. Approved copy is present in packaged DEX and rejected copy absent. Existing authorized external debug key reused. No commit/push, network, new permissions, encryption or biometric work. Owner phone acceptance remains pending; JVM verification does not supply phone sign-off.

## Historical Step 4 / phone audit (superseded by Batch A above)
Latest phone feedback: owner says 0712 still shows unknown net-worth balances plus a monthly delta. The retained 0712 APK DOES contain the correct fix: its SHA-256 matches the prior report, and DEX disassembly proves all three amounts and the only delta row use the same opening-balance null guard. Previous tests already included unknown balances WITH entries; do not claim an empty-only test caused this. The new real-MainActivity/persisted-SQLite screenshot regression also passed immediately on the starting source. Restoring only the historical unconditional row as a negative control reproduced ₹14,41,713 and failed the new test; correct source was then restored byte-for-byte. The actual phone cause is still unverified because the installed artifact/process was not inspected. Do not guess that the owner installed an older APK.

New regression covers synthetic persisted income/expense, absent balances, exact hint, no arrow/amount/month label, unchanged cashflow, Ledger return and Activity recreation. A first full attempt failed on 33 transaction-time pixels; the test timestamp was stabilized. Two subsequent consecutive full suites each passed 272/272 tests and 244 unchanged screenshot comparisons (126s/138s), with source fingerprints unchanged through APK assembly. All original 242 screenshot baselines stayed byte-identical. APK assembly had one unexplained exit 143; identical resource-capped retry passed in 58s. Full evidence and limitations are in STATUS.md and `/tmp/vault-networth-phone-audit/`.

New signed APK: `/opt/vwealth/android/app/build/outputs/apk/debug/vault-debug-0917-0755.apk`; version 0.2.1 (3), 30,000,216 bytes; SHA-256 `a47c71c0063905313a53ec5c6c4fb0f33ab3dbf89dcc61111244f379bb12d3ab`. apksigner VERIFIED, same debug signer as 0712, aapt2 zero permissions. All packaged entry contents except version-bearing AndroidManifest.xml are byte-identical to 0712; no additional UI behavior fix was invented. Version was incremented so Android App info can distinguish this build. Owner should retest this exact artifact and confirm 0.2.1 (3); phone acceptance remains pending. Real opening-balance setup is still future work. No commit/push authorization; no release signing.

Step 4 baseline (net-worth follow-up se pehle): owner ne explicitly kaha "preview mat dikhana direct app me daldo". Is Step 4 ke liye separate preview approval waived tha; implementation authorized thi, commit/push nahi. Local test persistence, calendar date picker aur Ledger implemented hain; initial record/build aur verification dono 249/249 pass, 218 unchanged screenshot comparisons; baseline lint zero errors. Exact commands/evidence STATUS.md me hain. Step 3 ka accepted visual design, spacing/tap feedback aur Shield V preserve kiye gaye hain. Actual phone test owner hi karta hai.

Step 4 runtime ab synthetic transaction history se seed nahi hota. Fresh install par entries/shortcuts empty hain; saved entries aur custom shortcuts local app restart ke baad reload hote hain. Home cashflow/recent activity, Ledger aur suggestions ek stored source use karte hain. Opening balances/assets/liabilities/graphs abhi implemented nahi, isliye unke sample totals ko real balance ki tarah dikhane ke bajay unset state hai. Account choices abhi existing fixed list hain. Ledger me search, combined month/mode/category/account/source/date-range filters, totals/count, grouped dates, empty states aur read-only entry details hain; full edit/delete/entity-detail workflows abhi future scope hain.

Storage: app-private no-backup `vault-step4-test.db`, SQLite schema 1, WAL + FULL synchronous writes. Atomic Save, request-ID replay protection, stable IDs, persisted three-second Undo and write-failure draft retention. Money integer paise; crypto integer 1e-8 units decimal TEXT me losslessly stored hain (supported range signed 64-bit se bada ho sakta hai), plus original quantity, integer rate and frozen INR/date. Merchant/note separate hain. Unsupported schema/corrupt file silently reset/delete nahi hoti. Zero new permissions; cloud/device-transfer exclusions explicit hain.

Ye plaintext TEST storage hai, encrypted vault nahi; phone checks synthetic/test entries se karo. Step 5 encryption/master password/Argon2id/biometric/sync abhi implemented nahi. `VaultDatabaseFactory`/`VaultDatabase` storage driver boundary aur `VaultRepository` contract SQL/domain/UI ko key handling se separate rakhte hain. Step 5 ko real encrypted driver aur verified plaintext conversion ya owner-approved test-data disposal chahiye; sirf key-source swap encryption ka substitute nahi.

Phone test gate: entry + note save -> force-stop/reopen -> same entry/amount/date in Home/Ledger; custom shortcut reopen; Ledger search/combined filters/clear; calendar month/year/past date/Cancel; historical crypto rate required + Save disabled; exact frozen INR after reopen; Save/Undo persists; both themes and short/tall screens. Phone-test APK `/opt/vwealth/android/app/build/outputs/apk/debug/vault-debug-0917-0627.apk`; SHA-256 `0e6096626b41f56eb27d9142d056ce94956f913ca333ade3113bfb0fccb699fe`. Signed v2, existing external debug certificate matched, zero permissions; version 0.2.0 (2). Exact evidence STATUS.md me hai. Owner sign-off ke bina commit/push nahi.

### Step 3 accepted baseline (historical)
Owner ne 2026-09-16 ko complete design lock kiya; Step 2 foundation ke baad ab Step 3 bhi phone par test karke sign-off de diya. Step 3 complete: approved design system (theme/colors/typography), paanch-tab navigation shell, Home aur shared Quick Add. Kotlin/Compose project `android/`, application ID `in.noxrelay.vault`; Geist fonts aur Lucide vectors locally bundled hain. Current entries/history synthetic in-memory fixtures hain; local persistence abhi nahi hai. Ledger/Accounts/Reports/More abhi placeholders hain.

Owner phone verification (owner-reported physical-phone checks; Codex direct device inspection ka claim nahi):
- Tall-screen Quick Add spacing sahi; dead gap khatam.
- Crypto past-date rate enforcement verified: historical date par rate maanga gaya aur Save disabled raha.
- Home, navigation, history-derived suggestions, toggle colors aur Indian number formatting sahi.
- Accent-tint tap feedback owner ne accept kiya.

Accepted follow-up: `VaultPressIndication` original Step 3 brief me nahi tha. Owner ne baad me app-wide tap-feedback change request aur phone par accept kiya: rounded theme-accent tint, maximum 10% opacity, 70ms ease-in / 160ms fade-out; focus/hover 5%. Resting layout aur hit targets unchanged.

Quick Add spacing rule: keypad + Save bottom anchored; flexible space amount ke dono taraf equally split, maximum 128dp per side. 640dp se chhoti height par zero; implementation 640dp par bhi zero rakhta hai. Cap ke baad extra space form/mode toggle ke upar jata hai; optional overflowing fields independently scroll, keypad/Save visible rehte hain.

Latest existing verification evidence: 190/190 JVM tests passed, zero failures/errors/skips; two consecutive verification runs each had 182 unchanged screenshot comparisons, zero added/changed/recorded. Coverage includes both themes, 320-430dp widths, 640dp short screens, tall 412x915/430x950/430x1400 layouts, 130% text, history/crypto/historical-rate states and press feedback. Signed debug APK assemble/signature checks passed (v2, one signer), packaged manifest zero permissions. This sign-off/docs/commit task did not rerun Gradle or change implementation.

Step 2 owner checks remain recorded: signed debug APK install/launch, no crash, expected foundation blank screen, Samsung Shield V adaptive launcher mask and zero permissions. Actual notification/status-bar 20px tint/readability and remaining phone-only gates are separate.

Emulator decision FINAL: is host par `/dev/kvm` absent aur CPU `vmx`/`svm` flags absent hain; KVM available nahi. Is host par emulator verification use nahi hogi aur emulator/system image/AVD kabhi install nahi karna. Approved screenshot verification Robolectric + Roborazzi se JVM par hogi. Is decision ko pending feasibility/provider question ki tarah reopen mat karna.

Debug signing: keystore `/root/vault-keys/vault-debug.keystore`, mode 600, repo ke bahar. External `/root/vault-keys/local.properties` bhi mode 600 hai; usme keystore path aur signing credentials hain. Build us file ka path `VAULT_SIGNING_PROPERTIES` environment variable se leta hai; keystore path build script me hardcoded nahi. Gradle execution-history cache bhi repo ke bahar hai. Is handoff me location/commands owner-authorized documentation hain; password, private key aur signing properties file kabhi commit/public copy nahi karni. Release signing key abhi nahi bani; wo alag explicitly approved step hai.

ROADMAP.md authoritative hai. Exact Shield V SVG, approved five-tab design, shared Quick Add, bubble/widget requirements aur security gates unchanged hain. Native tax replacement sirf yearly CA CSV export hai. Existing preview aur deployed old web/Worker behavior untouched hain.

## Commit and repository scope

Owner explicitly authorized the Step 4 + Batch A commit and both non-force pushes:
`feat(android): local persistence, Ledger, Accounts, Liabilities and entity detail`

Private vwealth scope: complete Android source/tests and Gradle configuration, android/README.md, AGENTS.md, STATUS.md, HANDOFF.md and ROADMAP.md. APKs, PNG outputs, local.properties, signing keys/properties, build directories and Gradle caches remain excluded. Preserve existing repository-local deploy-key configuration; never publish key contents.

Public vwealth-docs has its own history and receives ONLY HANDOFF.md and ROADMAP.md. No private source/history, STATUS.md, AGENTS.md or artifacts. Before push, verify the public staged change and full tree against this two-file allowlist and run artifact/credential checks. Completion gate: both working trees clean, freshly fetched origin/main equal local HEAD, public tree exactly those two files. Exact commit hashes and push results belong in the final report, not self-referential documentation.

## Agla kaam

Batch B — Reports, Subscriptions/Recurring, Targets/Goals. Step 4 and Batch A do not need another sign-off round; owner accepted 0.3.1. Keep the approved deviations, partial-state rule and mandatory pre-APK walkthrough. Do not silently change scope/estimates or start encryption/biometric/sync as part of this docs/commit/push task.

Historical preparation: local browser previews `/tmp/vault-step4-preview/` me banaye the; owner ne baad me unhe dikhane/approve karwane ka gate hata kar direct app implementation authorize ki. Project Knowledge STATE files is session me accessible nahi mili; actual repo HANDOFF/STATUS/ROADMAP/source aur owner's explicit Step 4 brief se implementation ki gayi. STATE ko read kiya hone ka claim nahi. Preview artifacts final native verification ka proof nahi hain.

Layout/screenshot iteration Robolectric + Roborazzi native graphics se JVM par hogi. Samsung-specific overlays, actual fingerprint/StrongBox, haptics, 120Hz/frame pacing aur OEM battery/background behavior owner phones par verify honge. Step 2/Step 3 phone checks ko in remaining gates ka sign-off mat samajhna.

Existing repos, Worker/bucket/hostnames, old Rust app, production DB/backups aur unrelated projects unchanged rahenge. Extra permissions, weaker encryption, new infrastructure aur release signing ko implicit approval nahi hai.

### Regenerate commands
Har Gradle invocation `gradle-safe` se: systemd scope MemoryMax=6G, CPUWeight=50, nice=10; Gradle 3g heap, Kotlin 1536m heap, two workers, parallel=false. SDK selection ignored `android/local.properties` me hi rahe; system-wide SDK environment settings nahi.

```sh
cd /opt/vwealth/android

# Signed debug APK
VAULT_SIGNING_PROPERTIES=/root/vault-keys/local.properties ./gradle-safe :app:assembleDebug

# Regenerate all 242 native JVM PNG captures
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
