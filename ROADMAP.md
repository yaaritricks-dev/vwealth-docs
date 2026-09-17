# Vault Roadmap

Ye file sach hai. Koi bhi faisla, correction ya galti yahan likhi jayegi. Chat ya session ki baatein yaad nahi rakhi jayengi. Agar koi cheez is file ke khilaf lage to pehle owner se poocho, apne aap mat badlo.

Ye owner ka approved plan hai. Har session ki shuruaat me AGENTS.md ke saath ye bhi padhna.

## Design locked - Vault (owner sign-off: 2026-09-16)

Owner ne complete private prototype ka design approve kar diya hai. Approved baseline: `vwealth-preview` version `ab9e9013-34a1-4bf9-97e1-d3f7580af483`, source `prototype/`. Niche ke naye branding/capture/export decisions native build specification hain; inhe already implemented Android features mat samajhna.

- Naya Android app naam: Vault. Kotlin/Compose app isi naam se banegi.
- Is task me code, preview branding, repository names, service names, Worker/R2 resources ya hostnames rename nahi karne.
- Purana running web V-Wealth hi rahega jab tak owner uska removal approve na kare.
- Design review complete hai; naye unrequested design rounds nahi. Agla engineering step emulator feasibility + Compose interaction spike hai, phir full native build.
- Tax calculator Vault me nahi hoga. Owner ka faisla: "wo CA ka kaam hai". Sirf "CA ke liye export": saal chuno, poore saal ka data CSV me. Purane web/preview ka tax code abhi untouched; E8 rewrite native release requirement nahi hai.
- Existing approved Light-default UI, Dark/System choices, capture safeguards, entity details, liability detail, no-overlap rules aur privacy constraints barkarar hain.

### Logo - Shield V (exact canonical SVG)

```svg
<svg viewBox="0 0 100 100">
  <path d="M50 10 L84 24 V52 C84 71 69 84 50 91 C31 84 16 71 16 52 V24 Z" fill="none" stroke="#a8e05f" stroke-width="7" stroke-linejoin="round" opacity="0.42"/>
  <path d="M34 40 L50 66 L66 40" fill="none" stroke="#a8e05f" stroke-width="10" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```

- Background `#0d1409` (dark ink); stroke `#a8e05f` (lime).
- Shield opacity 42%; V full opacity. Paths, stroke widths, caps/joins aur proportions wahi; apna logo variant nahi banana.
- App icon, splash, notification icon, floating bubble - sab Shield V.
- 20px par readability mandatory, especially notification/status bar. Android system icon masking/tint aur 42% shield visibility ko emulator/phone par verify karna; platform adaptation ki zaroorat ho to owner ko dikhao, canonical SVG chupke se mat badlo.

### Floating bubble - approved full specification

Reference: https://claude.ai/artifact/UCpKpBfyWPQcBVExuNydhA
Reference improvement ke liye hai, exact copy ka bandhan nahi; mool interaction nahi badlega. Is documentation pass me artifact content fetch nahi hua; URL aur owner-written spec recorded hain, reference visually inspected claim nahi hai.

- Screen ke upar persistent Shield V bubble.
- Owner-authorized special permission sirf "draw over other apps". Usage Access, Accessibility, notification listener, SMS, clipboard ya foreground-app spying nahi.
- Drag kahin bhi; release par nearest edge snap. Position yaad rahe.
- Idle par aadha edge me tuck aur opacity kam; touch par poora visible.
- Tap: wahi shared Quick Add sheet khule.
- Long-press: last-used owner shortcut directly prefilled, keypad ready. Last shortcut deleted/unavailable ho to blank Quick Add, stale account me save nahi.
- Neeche ✕ target par drop: 1 ghanta snooze, permanent off nahi.
- Back: sirf sheet dismiss; Vault dashboard/launcher na khule, underlying app wahi rahe.
- Full-screen video/game/camera me automatically hide karna product requirement hai; privacy-safe implementation ki feasibility niche spike gate me hai.
- Sheet: Expense/Income toggle, amount, owner shortcuts, category, account, keypad, Save - app wala same capture component/validation. Mode-switch toast nahi, Save kabhi covered nahi, duplicate save guard aur frozen crypto INR semantics same.

Bubble Settings:
- On/off
- Size S / M / L
- Idle visibility / opacity
- Edge tuck on/off
- Instant save on/off, default OFF: shortcut + amount, 3-second Undo. Approved idle/countdown/Pause safeguards shared capture se reuse; incomplete amount par auto-save nahi.
- Hide in full-screen on/off

### Widgets + Quick Settings tile

Reference: https://claude.ai/artifact/DFnJ9NJqBtxBYXk1QTbkXt
Is documentation pass me artifact content fetch nahi hua; owner-written spec source of truth hai.

Teen widgets:
1. 4x2 default: net worth upar; neeche owner ke 3 selected shortcuts + Add.
2. 2x2 jodi: ek me net worth + small chart; doosre me today's spend + Add.
3. 4x1 strip: this month's income/spend + Add.

Shortcuts owner-configurable, hardcoded merchants nahi. Shortcut/Add tap directly mini Quick Add khole, main app/dashboard khole bina entry ban sake. Amount/validation ke bina chupchaap transaction post nahi hoga.

Quick Settings tile: tap par direct Quick Add. Owner ka unlock rule: capture ke liye fingerprint prompt nahi; full Vault app kholne par fingerprint. Ye capture-only access hai, full decrypted vault/history access dene ki permission nahi. Locked/rebooted/read-only cases ke secure protocol ko niche gate ke hisaab se implement karna.

### CA ke liye export (Tax replacement)

- Sirf year selection + poore selected year ka CSV; tax calculation, regimes, deductions, tax explanation ya E8 port nahi.
- Export phone par locally; owner ki explicit export/share action se. CSV plaintext financial data hoga, encrypted backup ka replacement nahi; Worker ko plaintext nahi bhejna.
- Year selector exact start/end dates dikhaye. Calendar year vs Indian financial year label/range CSV contract me build ke waqt saaf karna; ambiguous year silently choose nahi karna.
- Exact paise/crypto quantities aur frozen INR values preserve; stable columns, CSV escaping/formula-safety aur count/date-boundary tests. Extra CA dashboard ya tax engine scope me nahi.
- Purane V-Wealth web ka tax feature abhi nahi hatana. Approved HTML prototype me old illustrative Tax screen historical design artifact hai; native Vault me CA export replace karega.

### JVM native verification — Robolectric + Roborazzi (approved; emulator decision final)

- Measured host facts: `/dev/kvm` absent aur CPU `vmx`/`svm` flags absent; is VPS par KVM acceleration available nahi. Emulator is host ka viable/approved verification path NAHI hai. Emulator/system image/AVD kabhi install nahi karna; ye pending feasibility/provider question nahi hai. Is section ka final decision document ke older emulator-first references ko supersede karta hai.
- Approved screenshot method: Compose ko Robolectric `GraphicsMode.NATIVE` + Roborazzi se JVM par PNG render karke verify karna; har layout fix ke liye owner phone ka intezaar nahi. Step 2 harness complete: 18 captures, exact dimensions/pixel assertions, logo 20/24/48/108dp aur empty scaffold widths 320/360/390/412/430dp, light/dark.
- 320-430dp layouts, Light/Dark/System, dialogs, keypad, input errors, navigation/back, rotation/insets aur accessibility/font-scale ke JVM-testable checks isi harness me add honge. Ye future coverage hai; Step 2 ke 18 foundation captures ko in sab checks ka completed proof mat kehna. JVM screenshot result ko actual OS/device behavior ka substitute mat kehna.
- Owner phones: Samsung-specific overlays across apps, actual fingerprint/StrongBox behavior, haptics, 120Hz/frame pacing aur OEM battery/background behavior. Ye phone-only gates unchanged hain.
- Owner Step 2 phone sign-off: signed debug APK install/launch successful, no crash, expected blank screen, Samsung launcher par Shield V adaptive icon masking correct, permissions screen par zero permissions. Actual notification/status-bar 20px tint/readability aur remaining phone-only gates abhi separate verification hain.
- Synthetic test data only; production DB/backups ko JVM harness ya test device me copy nahi karna. No emulator/AVD, VM/service changes ya release signing authorization is verification method se infer nahi karna.

### Native spike gates - approved intent, implementation proof pending

Ye owner ke decisions ko reverse nahi karte; code shuru karte waqt inhe silently guaranteed/solved mat maan lena:
- "Always over every app": Android overlay visibility OS control karta hai; protected apps/system surfaces ke restrictions bypass nahi karne. Full-screen detection bina Usage Access/Accessibility/foreground-app inspection ke verify karna; overlay geometry/insets ko perfect app/video detection mat kehna. Unsupported cases milein to owner ko limitation dikhao. [Android overlay contract](https://developer.android.com/reference/android/view/WindowManager.LayoutParams#TYPE_APPLICATION_OVERLAY).
- Bubble lifecycle ko foreground service/ongoing notification chahiye ho sakti hai. Exact manifest/service type aur any extra permission pehle explain/approve; "sirf draw over" ke naam par unapproved permission add nahi. [Foreground service requirements](https://developer.android.com/develop/background-work/services/fgs/service-types).
- No-fingerprint capture versus locked encrypted DB: existing reboot/master-password/auto-lock rules weaken nahi karne. Capture-only encrypted inbox/write-only approach aur pending/unavailable account context decide/test; master key permanently unlocked rakhna approved nahi. OS keyguard restrictions bhi respect karne hain. [Quick Settings locked-device guidance](https://developer.android.com/develop/ui/views/quicksettings-tiles).
- Widget net worth/spend launcher par dikhega: lock-state redaction/cached summary protection, read-only device capture rejection, stolen-phone exposure aur first-unlock behavior threat model me resolve karna. Widget/bubble se history/export/full vault bypass nahi.
- Exact Shield V 20px/system-notification rendering and VPS emulator viability must be measured, not promised.

## Maqsad
Ek personal finance app jo (a) entry banana 3 second ka kaam bana de, (b) bhooli hui entry khud pakad le, (c) server hack ho jaye to bhi data na khule.

## Latest scope faisla - full app v1, web retire hoga
Owner ne capture-only v1 aur reports/tax/settings ko web par rakhne wala split cancel kar diya hai. V1 ab poora personal finance app hoga. Complete redesigned prototype ab approved hai; pehla saat-screen draft superseded hai. Latest Tax exclusion/CA export decision upar authoritative hai.

App v1 ka main scope:
- Capture: quick add, user-made templates, share sheet, Pending inbox, floating bubble, Quick Settings tile aur home-screen capture widget; pichhli entries se amount suggestions.
- Home: aaj ka kharcha, monthly income/spend summary, net worth, recent entries aur pending/reminder overview.
- Reports aur analytics: monthly income, monthly spend, kitna aaya/kitna gaya, trends, charts, heatmap, source breakdown aur projections. Saara hisaab phone par, offline.
- Har merchant/category/source/account/goal ka shared detail-page layout: current month + lifetime, month-by-month trend, previous-month amount/percentage, dated entries, 1M/3M/6M/1Y/Lifetime filters, monthly average aur largest entry/date. Ledger/search/report names aur category chart segments direct detail kholenge; transaction amount se individual entry. Capture templates/pickers selection apna capture behavior rakhegi, template history ka alag raasta. Account income/spend/net flow current balance se alag; goal allocations spending nahi hain. Undated opening savings ko invented monthly history mat banana.
- Ledger aur filters: month, category, account, source aur custom date range; filters saath me apply aur clear ho sakein.
- Search: merchant, note aur entry dhoondhna; search results par bhi relevant filters.
- Subscriptions aur recurring: monthly auto-pay schedules, bill reminders, EMI, rent aur repeat hone wali entries; due/upcoming, pause/resume, skip aur Pending se confirm. Auto-pay yahan existing payment schedules ki tracking hai; app se actual bank debit/payment initiate karna is scope me nahi hai.
- Accounts: bank, cash, crypto; balances, transfer, conversion aur reconcile.
- Liabilities: credit card, loan aur udhaar; outstanding debt, repayments aur net worth me karz ghatana. Shared detail-page style me outstanding, paid-so-far, monthly EMI/payment, payoff estimate, dated payments aur declining outstanding chart. Prototype principal-only estimate interest/fees/new borrowing exclude karta hai aur ye assumption visible rahega; production amortization ko is simplified demo se complete nahi maana jayega. Ye purane Phase 5 se ab v1 me hai.
- Entry management: create/edit/delete, Trash/restore, galat account se sahi account me move, aur audit trail me purani value/history preserve karna. Ye purane Phase 5 se ab v1 me hai.
- Targets/goals: create/edit, allocation, progress, ETA, pause/resume aur completion.
- CA ke liye export: selected year ka complete CSV. Tax app se excluded; old web tax untouched.
- Settings: device management, sync status/outbox, storage, lock/biometric, appearance, encrypted backup/restore aur Recovery Kit.
- Bhooli entry pakadna: weekly balance reconcile, 3 din entry na ho to nudge, aur raat ko zero-spend check.
- Motivation, streak aur net-worth widget: purane Phase 6 se ab v1 me.

AI insights, AI Q&A aur AI tax explanation retire karne ka pehle wala faisla barkarar hai. Native tax calculator bhi ab excluded hai; local charts/reports aur CA CSV export remain in scope.

## Hosting ka faisla - Cloudflare Worker + R2
V-Wealth VPS chhod ke Cloudflare Worker + R2 par jayega. Naye plan me server sirf encrypted blob rakhta hai, koi calculation nahi karta; isliye Rust/Axum/SQLite server ki zaroorat nahi rahegi. Owner akela hai, isliye VPS maintenance ka jhanjhat khatam karna hai.

Fayde:
- Koi OS patch, reboot, SSH ya service restart nahi
- Worker owner ke paas wale Cloudflare edge par request serve karega. R2 bucket ko APAC location hint milega; placement best-effort hai, Mumbai guarantee nahi. Europe VPS ka ~97ms overhead kam hone ki expectation hai, par actual p95 phone se measure hoga.
- Self-managed VPS/app downtime khatam; Cloudflare outage ka risk phir bhi rahega
- Blob history hum immutable revision-key objects se implement karenge. R2 automatic date-based object-version restore nahi deta; lifecycle rule 90 din baad purane revision objects delete karega.
- Workers Paid ka minimum $5/month hai. R2 storage/operations ka billing alag hai; expected tiny usage free tier ke andar rehna chahiye, par $5 hard all-inclusive cap nahi hai.

Seemayein jo tay hain aur V-Wealth ko nahi chubhti:
- Blob endpoint intentionally milliseconds me khatam hoga. Workers Paid ka default CPU budget 30 seconds hai; HTTP request ka hard 30-second wall-time limit nahi hai.
- Lagatar chalne wala kaam Worker par nahi chalega. Noxyaari VPS par hi rahega kyunki wo Telegram se 24/7 juda hai.
- Bhaari calculation Worker par nahi hogi; naye system ka calculation Android app par hoga.

Production target:
- Private R2 bucket: `vwealth-vault-prod`, Standard storage, APAC location hint
- Worker hostname: `vault.noxrelay.in`
- Owner ka naya faisla: deployment API token Android app build ke dauran root-only file me rakha jayega, kyunki Worker updates aayengi. App chalne ke baad owner revoke karega; abhi revoke/delete nahi karna.
- Staging skip hoga. Full local Miniflare/CAS/security tests ke baad owner approval se seedha production deploy hoga.
- Worker `vwealth-vault` production par deploy ho chuka hai; `vault.noxrelay.in` Custom Domain live hai. Public health minimal hai aur baaki vault routes device signature maangte hain.
- Worker ko DEK nahi milega. Wo envelope structure, size/checksum aur device signature verify karega; AEAD authentication client decrypt ke waqt hogi.
- R2 publish sequence: immutable revision object PUT, phir current pointer conditional CAS. R2 rename par kabhi bharosa nahi karna.

## GitHub source backup aur public docs
- Poora V-Wealth code aur uski history `yaaritricks-dev/vwealth` private GitHub repository me backup hogi.
- Sirf `ROADMAP.md` aur `HANDOFF.md` `yaaritricks-dev/vwealth-docs` public repository me jayengi, taaki owner ka AI assistant plan padh sake.
- Public docs repository alag Git repository aur alag history hogi. Private repository ki branch, subtree ya filtered history use nahi hogi.
- Dono repositories ke liye alag repo-scoped SSH deploy key hogi. Private keys repository ke bahar mode 600 me rahengi.
- Pehle push se pehle full-history secret scan, historical screenshots ka privacy review, tracked-path/ref/fsck checks aur public repository ki exact two-file allowlist mandatory hai.

## Fresh start - purani entries migrate nahi hongi
- Naye app me purani 489 entries nahi jayengi. Sirf current bank, cash aur crypto balances dale jayenge.
- Purana data current web/VPS par apni jagah rahega jab tak owner removal approve na kare.
- Owner purane ledger ka CSV export offline rakhega.
- Isliye plaintext-to-encrypted entry migration ya historical cutover nahi hoga.

## Security ka core faisla - E2EE
Bitwarden wala model:
- Master password sirf owner ke dimaag me. Kabhi server pe nahi jata.
- Argon2id se master key banti hai, phone pe.
- Master key se DEK unlock hota hai. DEK se saara data encrypt.
- Server ke paas sirf encrypted blob. Naam, amount, merchant - kuch padhne layak nahi.
- Dono Samsung phone same master password se same DEK paate hain. Sync automatic.

Iska natija: naye system ka saara calculation phone app pe hoga. Cloudflare Worker + R2 sirf encrypted blob store karenge. Purana web VPS par full app v1 ready aur owner-approved hone tak chalega; blob-based web client nahi banega.
Data abhi 156 KB / 489 rows hai, isliye ye realistic hai.

Recovery Kit: 24-word phrase, kagaz pe, locker me. Digital kahin nahi.
Master password bhoola aur kit kho gayi = data hamesha ke liye gaya. Koi recover nahi kar sakta.

## Capture - sab owner ke haath se, 3 second me
1. Floating bubble - screen ke kinare, hamesha. Tap karo, amount daalo, done.
   Bubble me owner-made Swiggy/Blinkit/Petrol template tap karne se merchant/category/account bharenge; phir sirf amount.
   Foreground app detect nahi hoga. Context sirf user-selected template ya shared screenshot se aayega.
2. Quick Settings tile
3. Home screen widget - amount pad seedha
4. Share sheet - payment screenshot share karo, phone pe hi parse hoga
5. Recurring - rent, EMI, subscription apne aap Pending me

Amount suggestions pichle entries se - do tap me ho jaye.

## Bhooli hui entry pakadne ka jaal
- Hafte me ek baar balance reconcile: "Kotak me kitna hai?" - farak dikha ke bataye ki entry chhooti hai
- 3 din entry na ho to nudge
- Roz raat "aaj ka kharcha 0 hai - sach me?"
Ye sab bina kuch padhe kaam karta hai.

## Jo hum NAHI karenge - ye kabhi mat suggest karna
- SMS padhna
- Notification listener
- Usage access / background me taakna
- Clipboard access
- Accessibility service
- Server pe plaintext financial data
- Play Store pe publish (sideload only)

Owner ne ye saaf mana kiya hai. Koi bhi feature ye permission maange to pehle owner se poochna.

## Phases

Phase 0 - Neev (ho chuka hai)
Batch 1-4 audit fixes: data loss, financial calculations, daily flows, security.
Bacha hua: Cloudflare Access dashboard pe.

Phase 1 - Worker + R2 aur backup bahar
Cloudflare Worker + R2 foundation, encrypted blob storage aur encrypted off-site backup. Noxyaari VPS par rahega; V-Wealth ka naya backend VPS par nahi hoga.

Phase 2 - Encrypted blob architecture

Kaise kaam karega:
- Asli database phone pe local rahega. Saara calculation, analytics, charts - sab phone pe, instant aur offline.
- Cloudflare Worker + R2 sirf encrypted blob rakhenge. Unhe koi financial calculation nahi karni. Wo tijori hain, dimaag nahi.
- Dono phone wahi blob se sync karenge. Blob-based web client nahi banega.
- Server andha hai - E2EE jitna hi surakshit, par banane me kaafi aasan kyunki server ko per-record versioning, merge logic ya calculation nahi karni, par blob-level revision aur compare-and-swap zaroori hai.

Crypto:
- Master password se Argon2id (phone pe) se master key
- Alag random DEK jo data encrypt karta hai. DEK master key se wrapped, server pe stored.
- Server ke paas: encrypted blob + encrypted DEK + KDF salt. Password, master key ya plaintext data kabhi nahi.
- Recovery Kit: 24-word phrase, DEK ka alag wrapper. Kagaz pe, locker me. Digital kahin nahi.

Naya phone:
- App install, master password, DEK unlock, blob download, data wapas. Data dono jagah hai: phone pe local DB, server pe aakhri successfully synced version.
- App me "remove this device" ka option hoga. Ye sirf aage ka server access rokta hai; purane phone pe jo data already utar chuka hai wo nahi mitega.
- Asli revocation ke liye DEK rotate karke poora blob dobara encrypt karna padega.

Roz ka unlock:
- Master password sirf teen mauke pe: pehli baar setup, naya phone, phone reboot ke baad pehli baar.
- Baaki har baar sirf fingerprint. Master key Keystore/StrongBox me wrapped rehti hai.
- App background me jaye to 1 minute baad auto-lock (settings se badal sake).
- Face unlock nahi - Samsung ka face Android me crypto-grade nahi hai.

Do phone ka jhagda:
- v1 me ek device primary (S25U) likhega, doosra read-only. Conflict ka sawal hi nahi.
- Baad me dono me likhna khola ja sakta hai per-record versioning ke saath.

Blob safety - ye sab mandatory hai:
- Har blob ka monotonic revision number
- Upload pe If-Match / compare-and-swap - stale blob reject ho
- Atomic publish: immutable revision object pehle PUT karo, phir current pointer ko conditional CAS se publish karo; R2 me rename nahi hota
- Pichle kuch blob versions immutable rakho
- Client apna last acknowledged revision yaad rakhe
- AEAD authentication fail ho to blob reject
- Read-only device ka rule server enforce kare, sirf UI button disable karna kaafi nahi

Jo ab NAHI chahiye (purana E2EE plan ka hissa tha, ab zaroorat nahi):
- Shared calculation core WASM/FFI - naye system ka calculation native Android app me hoga; blob-based web client nahi banega
- 7-step protocol migration - ab simple hai

Golden test vectors phir bhi chahiye - Rust reference fixtures aur Android ka hisaab match hona chahiye.

### E2EE ke khule faisle
- AI feature retire hoga. AI insights, Q&A aur tax explanation nahi chahiye. Abhi code se nahi hatana; implementation phase me safely remove karna.
- Purana plaintext web/DB/backups owner ke removal approval tak VPS par rahenge. Unki verified deletion ya re-encryption ke bina purane system ka D6 khula rahega.
- Cloudflare Access + Android background sync: browser cookie background sync ke liye nahi chalega, aur APK me service token daalna safe nahi. Access configure karne se pehle ye tay karna hai.

Phase 3 - Full Android app v1
Upar diya poora main scope ek hi v1 release ka hissa hai. Feature modules ko kram se build/test karna hai; capture-only build ko finished v1 nahi kehna.
BiometricPrompt + BIOMETRIC_STRONG + Keystore/StrongBox, fingerprint primary.
Sideload, Play Store nahi. Samsung S25U + doosra Samsung.

Phase 3 - Full app quality aur design

Quality bar:
- Ultra-premium, world-class native app. Kotlin + Jetpack Compose, Material 3; Samsung One UI ke saath fit. Basic/lightweight dashboard mockup quality acceptable nahi. Koi WebView wrapper ya hybrid nahi.
- 120Hz animations (Samsung S25U), haptic feedback, instant open (zero loading screen), gesture navigation, One UI ke saath fit.
- Offline-first: local DB phone pe, sync background me.
- Latest owner decision: Light mode primary/default. Settings me Light / Dark / System; choice persist ho, System live OS preference follow kare. Complete dark mode bhi; har screen par consistent typography, spacing, components, readable charts aur polished interactions.
- Ek haath se use: core actions thumb ke paas, clear navigation, useful bottom sheets aur bade touch targets. Reports/search/forms me bhi same quality bar.

Context ka rule:
- Foreground app detect karna mana hai. Uske liye Usage Access ya Accessibility chahiye; ye permissions hum nahi lenge.
- Context sirf do tareeke se aayega: share sheet (payment screenshot share karna) aur user-made templates (bubble me Swiggy/Blinkit ka shortcut; tap se merchant + category bhar jaye).

Design process (mandatory):
- Pehla saat-screen draft superseded hai. Final full-app prototype owner-approved on 2026-09-16; ise locked native design baseline mano, upar ke new Vault/capture/CA-export decisions ke saath.
- Pehle full information architecture, navigation aur screen/flow inventory banana; har main-scope feature ko design me cover karna.
- Latest owner correction: ab poora prototype ek saath banana aur ek hi review me dikhana hai. Purana four-screen/incremental review rule superseded hai. Unlock se Audit trail tak saari requested screens aur Reports ke overview/spending/income views include honge; beech me screen-by-screen approval nahi poochna.
- Native build se pehle full clickable HTML prototype private HTTPS preview par owner ko dikhana hai, screen by screen. Screens ke beech links aur main actions genuinely clickable honge, sirf static cards nahi.
- Owner design sign-off mil chuka hai. Agla task approved native spike/build hai; is docs/commit task me Kotlin nahi likhni.
- Bina approval ke koi screen banana allowed nahi.
- HTML me verify na hone wale fingerprint, OS bubble, QS tile, widgets, share sheet, haptics aur 120Hz behavior ke liye design approval ke baad chhota Compose interaction-spike APK; phir full native implementation.

Scope discipline:
- Main scope ke saare modules v1 me mandatory hain; reports, CA export, reconcile, liabilities, audit trail ya motivation ko future Phase 5/6 keh kar postpone nahi karna.
- Implementation milestones modules ke hisaab se honge, lekin finished app ka sign-off tabhi jab poora scope complete ho.
- Scope ko kam karke timeline fit nahi karni; naya risk/feature aaye to estimate update karna.

Full-app prototype screen/flow inventory - minimum coverage:
1. Setup/unlock: master-password setup, fingerprint unlock, lock, Recovery Kit setup/verification, new-phone restore aur read-only device state.
2. Home: daily/monthly overview, net worth, recent activity, pending/reminders, motivation aur streak.
3. Capture: Quick Add amount/category/account/source/note/date, templates list/create/edit, share review, Pending list/detail/confirm/dismiss; bubble, QS tile aur widgets ke interaction storyboards.
4. Ledger/search: full entry list, search/results, combined filter sheet, date-range picker, entry detail/edit, account correction, audit history, Trash/restore.
5. Reports: monthly income/spend/cash flow, trend charts, heatmap, source/category/account breakdown aur projections; report filters aur drill-down.
6. Subscriptions/recurring: list/calendar/upcoming, create/edit schedule, bill/EMI/rent details, reminder, pause/resume, skip aur generated Pending confirmation.
7. Accounts: overview, bank/cash/crypto details, account setup/edit, transfer, conversion, reconciliation/difference review aur confirmation.
8. Liabilities: credit card/loan/udhaar list, create/edit, debt detail, repayment aur net-worth impact.
9. Targets/goals: list, create/edit, allocation, progress/ETA, pause/resume aur completion.
10. CA export: year/date-range label, complete CSV download/share, success/error states; tax calculation UI nahi.
11. Settings/security: devices/enrollment/revocation, sync/outbox/retry, storage, lock/biometric/appearance, encrypted backup status/export/restore aur recovery.
12. Cross-screen states: populated, empty, validation failure, offline, sync failure/conflict, success/undo aur destructive-action confirmation. Actual device/browser limits ko fake security behavior se nahi chhupana.

Prototype delivery:
- Clickable HTML prototype `prototype/` me; private HTTPS preview `preview.noxrelay.in` par alag Worker `vwealth-preview` se serve hoga. Full requested app flow ek saath review hoga; purana seven-screen capture draft served nahi hai.
- Har screen me dark/light aur relevant empty/error/background-loading/confirmation states reviewable rahenge. States control failures/refresh ko simulate karega. Normal navigation instant; spinner, skeleton ya artificial waits nahi.
- Owner ke navigation feedback ke baad bottom nav me paanch tabs: Home, Ledger, Accounts, Reports, More. Nav scroll par kabhi hide/move nahi hoga. Raised rounded bar, subtle transparency/blur, Material 3 Expressive-inspired icon/indicator transition aur halka bounce; iOS liquid glass nahi. Touch targets ki position animation me bhi stable rahegi. Latest correction: floating Add sirf Home/Ledger par; forms, entity details aur apne primary action wale pages par nahi. Iski alag patti/separator nahi. Home/Ledger me explicit 160px end spacer + normal padding, taaki final content button se poora upar aaye. Baaki scroll screens me bhi end clearance. Food/Kotak charts, liabilities/goals/recurring actions ke upar competing Add nahi. Home par Accounts/Subscriptions priority cards aur workspace shortcuts bhi rahenge; More se saare baaki sections accessible honge.
- Light mode crisp white/green hoga, beige nahi. Chart/app text-selection ki blue/black highlight patti nahi; user-select none, no tap-highlight/callout. Form editing usable rahega. Category icons proper SVG icon-library se honge.
- Quick Add templates owner-editable honge; Swiggy, Zomato, Blinkit, Amazon, Petrol, Kirana sirf demo seeds hain, fixed catalog nahi. Expense aur Income ke alag shortcuts; income me expense shortcuts nahi. Templates/categories/accounts/income sources/recurring/goals/liabilities me create, edit, delete aur reorder ka raasta. Used accounts/categories/sources archive honge taaki balances aur history na udein.
- Quick Add amount live Indian comma formatting ke saath screen ki width me fit hoga; zaroorat par font automatically chhota. Rupee symbol aur number centred ek unit honge, alag kinare par nahi. Expense deep terracotta, Income green: farak sirf toggle aur amount area me; baaki capture screen neutral. Extra EXPENSE badge, money-in/out explanation aur Expense/Income amount heading nahi. Save action hamesha green, sirf Save label; mode ya template badalne par old amount clear hoga, galat mode/currency me carry nahi hoga.
- Template tap se merchant/category/account prefill; selected details compact, keypad upar. Suggested amounts actual prior entries se. New entry save single tap, no extra confirmation; synchronous duplicate guard aur exact-paise validation mandatory. Keypad bade touch targets, accidental rapid repeated tap suppression, deliberate repeated digits supported, hold-backspace clear; invalid/zero amount par Save disabled.
- Optional setting: template select karne ke baad auto-save, default OFF. Prototype me valid amount par typing rukne ke 2 seconds baad save, visible countdown + Pause; nayi typing timer reset karti hai, background me auto-save pause. Har save ke baad 3-second Undo; save se pehle Undo/demo-data footer line nahi. Ye timing behavior phone review ka hissa hai; default manual save rahega.
- Design usool: kam shabd, kam labels, kam samjhana. Jo rang ya position se saaf hai uske liye repeat text nahi. Important validation/security warnings aur necessary crypto-rate context phir bhi visible rahenge.
- Expense/Income switch par koi mode-explanation toast nahi. Koi toast/message Quick Add Save ko cover nahi karega; capture ke messages optional fields ke flow me inline honge. Post-save 3-second Undo rahega; turant next entry kholne par woh bhi inline, Save se alag.
- Crypto accounts se Income aur Expense dono: quantity, entry-date ka INR rate aur frozen INR value visible/stored. Baad ka live price purani entry ka INR amount nahi badlega. Past-date capture par explicit historical rate; current quote ko past rate batana mana hai.
- Charts me press/drag scrub: nearest point par thin vertical guide + small dot, upar compact date/value tooltip. Finger release/cancel par turant gayab; sticky readout nahi. Point change par light haptic: HTML preview me best-effort browser vibration, actual Samsung haptic feel native spike/phone test me verify hoga. Large net-worth headline whole rupees; underlying paise calculation unchanged.
- UI typography modern Geist medium/semibold; financial numbers tabular figures, large amounts/net-worth tight spacing. 320/360/390/412/430px dono themes me screen checks. Latest crypto-capture correction: rate aur frozen INR value compact side-by-side row, clear gap/divider; bada tall card nahi. Quick Add keypad aur Save viewport me fixed visible rahenge, bank/cash/crypto sab me; short-height devices par sirf upar ke optional fields scroll honge, keypad tak scroll nahi.
- Net worth card par Assets, Liabilities aur Net clearly alag dikhna chahiye; debt subtract kiye bina assets total ko net worth nahi kehna.
- Bank/cash/crypto current balances anytime editable: account detail par prominent Edit current balance action aur simple Reconcile form. Zero valid, blank invalid; adjustment/audit trail preserve. Crypto quantity exact decimals me rahegi.
- Crypto detail me coin quantity, INR rate per coin, INR holding value, source aur original last-updated timestamp. Fresh/stale/demo/unavailable states saaf; failed refresh me purana timestamp badal kar price fresh nahi dikhana. Prototype may fetch public CoinGecko quotes while holdings stay synthetic; real personal vault data kabhi provider ko nahi bhejna.
- Public quote provider unavailable ho to prototype Coinbase spot-price fallback use kar sakta hai. Coinbase provider-update timestamp nahi deta: UI me Rate fetched aur retrieval time dikhana, saath me provider quote time unavailable likhna; fetched time ko source update time mat kehna.
- Private review link possession se access milega; 256-bit key ka full link public docs/Git me kabhi nahi rakhna. HTML, JS, CSS aur fonts sab server-side access gate ke peeche rahenge.
- Sirf synthetic demo entries/balances. Real vault API/R2 se connection nahi; reload par demo data reset hota hai.
- Light primary/default, Dark/System available; bottom navigation/amount pad, smooth motion aur reduced-motion support.
- HTML fingerprint tap, device/sync aur lock-timing preferences simulated hain. Asli biometric, OS capture surfaces, haptics aur 120Hz feel approved Compose interaction-spike APK par phone test se verify honge.
- Full current prototype design approved; seven-screen draft was not the approved baseline. Native functionality/security/hardware acceptance abhi alag baaki hai.

Web ka kya:
- Capture, reports, analytics, CA export, accounts, reconcile, targets, recurring, search aur settings - sab Android app me honge. Permanent web companion nahi rahega.
- Current web sirf full app v1 ready hone tak VPS par chalega; reports/settings ke liye alag app v2 ka wait ab plan nahi hai.
- Poora v1 owner phone par test/approve karega. Backup/recovery/restore checks aur old-data CSV archive verify hone ke baad owner ke explicit removal approval se web band hoga aur VPS se sirf V-Wealth hataya jayega.
- Blob-based web client nahi banega. Noxyaari aur unrelated VPS services apni jagah rahengi.
- Fresh-start decision barkarar: historical 489 entries app me migrate nahi hongi. Purane plaintext DB/backups ka disposition aur verified deletion/re-encryption alag approved step rahega; web shutdown se unki automatic deletion authorize nahi hoti.

Health monitoring:
Sirf phone se monitoring nahi chalegi - phone mara ya app crash hua to alert kaun bhejega. Do hisse:
- Server se: privacy-safe external uptime monitor + non-financial signed backup heartbeat
- Phone se: sync aur outbox ke alerts
Nox me yahi galti ho chuki thi - 16 din tak pipeline mari padi thi aur koi alert nahi aaya. Wo dobara nahi honi chahiye.

Delivery:
- APK sideload hoga, Play Store nahi. Owner khud install karega.
- Codex emulator screenshots se har native build/layout khud check karega. Owner phone-only hardware/OEM checks aur release acceptance karega; Codex owner ke physical phone ko directly inspect karne ka claim nahi karega.

### APK supply chain
Pehla APK banane se pehle tay karna:
- Offline signing key
- Signed update manifest
- Certificate pinning
- Rollback prevention
- Local decrypted DB ki protection: Keystore-wrapped key, Android backup se exclude, logs/crash/screenshot me leak na ho

Timeline - full app, realistic planning estimate:
Purana one-week/day-by-day delivery plan cancel hai. Worker endpoint deployed hona Android encryption, financial modules ya release readiness complete hone ke barabar nahi hai.

Ek focused implementation stream aur regular owner feedback ke liye remaining full-app work ka initial engineering estimate 14-22 hafte (lagbhag 3.5-5.5 mahine) hai. Ye fixed deadline/guarantee nahi; design ab approved hai aur native tax removed; native spike/crypto/CA export scope ke baad remaining estimate rebaseline karna hai, purana total remaining deadline nahi hai.
1. Full screen inventory, design, clickable prototype aur owner iterations: completed/approved; purana 2-3 week design allowance ab remaining work nahi.
2. Approved Compose interaction spike, native foundation, encrypted local DB, biometric/key handling, blob sync/conflict/recovery aur signing/update safeguards: 3-4 hafte.
3. Capture, templates, share/Pending, bubble, QS tile, widgets aur ledger/search/filters: 2-3 hafte.
4. Accounts/crypto/transfers/conversion/reconcile, liabilities, audit trail/account correction aur reports/analytics: 3-5 hafte.
5. Subscriptions/recurring/reminders, targets/goals, CA CSV export, full settings/backup aur motivation/streak/net-worth widget: 2-4 hafte.
6. Cross-feature financial/security regression, real two-phone/offline tests, backup/Recovery Kit/new-phone restore drills, accessibility/motion/performance polish aur owner acceptance: 2-3 hafte.

Dependencies: approved prototype se pehle Kotlin nahi; financial golden vectors modules ke saath likhne hain; CSV correctness aur recovery verification ke bina release nahi. Approval delays, unresolved security decisions, off-site backup choice aur additional scope calendar time badha sakte hain. Dates chhoti dikhane ke liye inhe skip nahi karna.
Web retirement sirf final full-app acceptance ke baad; timeline ke kisi din apne aap shutdown nahi hoga.

### Storage - kabhi full na ho
- Data chhota hai: abhi lagbhag 156 KB, das saal me shayad 6 MB.
- Target images compress karke rakhna; koi image 200 KB se badi nahi.
- Har accepted blob revision alag immutable R2 object hoga. Lifecycle rule 90 din baad purane revision objects apne aap delete karega.
- App settings me blob, retained versions aur images ka storage usage dikhega.
- Owner-defined storage budget/quota ke 80% par alert aayega. Budget ka exact number implementation se pehle tay hoga.
- Storage ya billing limit chup-chaap cross nahi hogi.

## Ye galtiyan dobara mat karna
1. Blob pe versioning nahi chahiye - GALAT. Revision + CAS ke bina stale upload poori vault mita dega.
2. Data sirf server pe hai - GALAT. Dono jagah hai.
3. Remove device se purana data mit jata hai - GALAT. Sirf aage ka access rukta hai.
4. Encryption ke saath 7 din me poora app - GALAT. Full native app, financial calculation correctness, CA export, recovery/security tests aur owner acceptance ko realistic multi-month plan chahiye. Fresh start hai; blob web client ya historical web-data migration banana scope nahi hai.
5. Foreground app detect karke context lena - MANA HAI. Usage Access/Accessibility nahi lenge.
6. Face unlock - Android me crypto-grade nahi. Fingerprint hi.
7. Noxyaari ko Worker pe le jaana - NAHI HO SAKTA. Wo Telegram se lagatar juda rehta hai, Worker pe lambe chalne wale kaam nahi hote.
8. Cloudflare account pe sirf noxrelay.in chhuna hai. yaaritricks.com aur yaaritricks.in ko haath nahi lagana. Ek purana Worker "gift-app-bot-user-staging" bhi chal raha hai - usse bhi door rehna. Token ka scope bhi sirf noxrelay.in zone tak rakhna.
9. Capture-only v1 bana ke reports/tax/settings web par chhodna - AB GALAT. Latest owner decision full app v1 hai; pehla saat-screen basic prototype approved design nahi hai.

## Khule sawaal jo abhi tay nahi hue
- Blob file format version, AEAD algorithm, nonce rules, Argon2 parameters, KDF upgrade path
- Recovery phrase standard entropy/checksum se banega (BIP39 jaisa) - apna scheme invent nahi karna
- Device enrollment, device signing keys aur revocation ka protocol - master password authentication nahi hai
- Fresh-start opening balances kis snapshot/date par liye jayenge aur old web ka read-only/archive transition kaise hoga - decide karna hai; parallel ledgers ko automatic synced data nahi batana
- Private HTML preview me sirf synthetic data rahega; koi browser-decrypted real vault client nahi banega
- Purane plaintext DB aur backups ka verified deletion ya re-encryption - iske bina D6 khula rahega
- Golden vectors drift pakdenge, rokenge nahi. Rust reference aur Kotlin financial rules maintain karna ongoing kaam hai; E8 jaise known-bad reference output ko sahi expected answer nahi maana jayega.
- Storage usage alert ke liye owner-defined logical budget/quota ka exact number

Phase 4 - Full app acceptance aur web retirement
- Main scope aur poori approved screen/flow inventory native app me complete.
- CA export correctness, golden financial tests, two-phone sync, offline/retry/conflict, recovery/restore aur security checks pass.
- Owner real phones par test kare aur full app sign-off de.
- Old web CSV archive verify; old DB/backups disposition separately approve.
- Explicit owner approval ke baad web band aur VPS se V-Wealth removal. Noxyaari/unrelated services untouched.

Purane Phase 5 aur 6 ab separate future phases nahi hain. Liabilities, audit trail, account correction, motivation, streak aur net-worth widget main v1 scope me upar included hain. Recurring/reconcile/nudges bhi v1 me hain.

## Audit se bacha hua kaam
- E8: old web tax remains known incorrect (deduction/marginal-relief/crypto-gains issues). Vault has no tax engine; E8 rewrite no longer a native release requirement. Old web untouched until separately authorized.
- D6: TOTP aur AI keys plaintext DB/backup me. E2EE ke saath hi theek hoga.
- Dead code: web/ folder, 1.4 GB build artifacts, unused React components
- G1: Rust unit tests nahi the (ab kuch hain), frontend typecheck/lint script nahi
- Frontend typecheck/lint aur strict CI - E2EE se pehle, kyunki client-side calculation aane ke baad drift pakadna mushkil hoga
- Golden financial test vectors - same fixture pe Rust reference aur Android ka output byte-for-byte match hona chahiye
- DB indexes - abhi 489 rows pe farak nahi, badhne par chahiye

## Hard rules
- Bina owner ki explicit permission ke koi UI screen redesign nahi
- Git commit sirf sign-off milne pe
- Production DB aur backups ko bina kahe haath nahi lagana
- Har task ke baad STATUS.md update
