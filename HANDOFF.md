# V-Wealth Handoff

Aakhri update: 2026-09-16 17:03 UTC

## Abhi kahan hain
Audit ke Batch 1-4 poore, test, deploy, owner-verify, local commit aur private GitHub push ho chuke hain.
ROADMAP.md authoritative cross-session sach hai. Target architecture Cloudflare Worker + R2 encrypted blob hai; history migrate nahi hogi, current balances se fresh start hoga, Noxyaari VPS par rahega. Android v2 reports/settings/reconcile complete hone se pehle VPS V-Wealth nahi hatega; blob web client nahi banega; tax alag pending hai.
Din 1 Worker backend complete aur production par live hai. Private `vwealth-vault-prod` bucket APAC hint/Standard ke saath live hai; direct bucket public access off, default multipart cleanup aur `vault/versions/` 90-day lifecycle verified hain. `vwealth-vault` Worker exact `vault.noxrelay.in` Custom Domain par deployed hai. Typecheck, 11/11 local Miniflare tests, dry-run aur live health/unsigned-auth smoke pass hain.
GitHub backup live hai. Poora repo private `yaaritricks-dev/vwealth` ke `main` par push aur fresh-fetch verify ho chuka hai. Independent public `yaaritricks-dev/vwealth-docs` repo ki fetched remote tree me sirf `ROADMAP.md` aur `HANDOFF.md` hain. Dono alag deploy keys use karte hain; koi force push nahi hua.

## Working tree
Worker implementation, deployment record aur docs owner-approved `feat: deploy encrypted vault worker` commit me ja rahe hain. Commit/push ke baad private `main` aur public two-file docs repo clean/fetch-verified hone chahiye.
Aakhri code commit: `6ef0ab3 Fix Batch 4 security gaps`; ye private repo ki pushed history me maujood hai.

## Agla kaam
Sabse pehle Cloudflare Dashboard me non-expiring deployment API token revoke karwao; revoke confirmation ke baad root-only local token file delete karo. Uske baad Roadmap Day 2: clickable HTML design prototype, private HTTPS preview aur owner screen-by-screen approval. Kotlin full app approval se pehle start nahi hogi.

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
- VPS par code/local tests; Wrangler/Miniflare local R2; phir staging aur production deploy.
- Worker R2 binding use karega; S3 credentials nahi chahiye.
- Existing Rust/Axum V-Wealth untouched rahega jab tak Android v2 complete aur owner removal approve nahi karta.

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
4. Blob `vault/versions/<revision>-<hash>.blob` immutable key par PUT.
5. `vault/current.json` pointer ko conditional R2 ETag CAS se publish.
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

### Time estimate
- Owner setup: 30-60 minutes.
- Scaffold/harness: 1-2 hours.
- Blob/history/storage routes: 3-4 hours.
- CAS/races: 2-3 hours.
- Device auth/enrollment: 3-5 hours.
- Lifecycle/health/failure tests: 2-3 hours.
- Staging deploy/smoke: 1-2 hours.
- Total: 10-16 focused hours; realistically 1-2 full days for A1-grade endpoint.

## Adhoora kuch hai?
Worker backend ka koi kaam beech me nahi. Production deploy, Custom Domain, required enrollment secret aur live smoke complete hain; production bucket me abhi koi vault/device object nahi hai. Pending operational cleanup: owner Cloudflare deployment token revoke kare, phir Codex local token file delete kare. Android client/device enrollment abhi bana nahi hai. Off-site backup aur storage-budget decision bhi pending hain.

## Owner ke pending kaam
- Cloudflare Access dashboard ka remaining setup/verification.
- Cloudflare Access ke liye Android-safe background authentication ka faisla.
- Storage alert ka logical budget/quota tay karna.
- Tax/E8 ka faisla baad me karna.
- R2 se alag off-site backup destination choose karna.
- Non-expiring Cloudflare deployment API token revoke karke Codex ko confirm karna.

## Session band karne ka niyam
Codex clear karne se pehle ye chaaron sach hone chahiye:
1. Koi kaam beech me nahi chhoda
2. Owner ne test kar liya aur sign-off diya
3. Commit ho gaya, working tree clean
4. STATUS.md aur HANDOFF.md dono update ho gayi
Agar in me se ek bhi nahi hua to clear mat karo.
