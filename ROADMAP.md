# V-Wealth Roadmap

Ye file sach hai. Koi bhi faisla, correction ya galti yahan likhi jayegi. Chat ya session ki baatein yaad nahi rakhi jayengi. Agar koi cheez is file ke khilaf lage to pehle owner se poocho, apne aap mat badlo.

Ye owner ka approved plan hai. Har session ki shuruaat me AGENTS.md ke saath ye bhi padhna.

## Maqsad
Ek personal finance app jo (a) entry banana 3 second ka kaam bana de, (b) bhooli hui entry khud pakad le, (c) server hack ho jaye to bhi data na khule.

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
- Bhaari calculation Worker par nahi hogi; calculation phone/browser par hogi.

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

Iska natija: naye system ka saara calculation phone app pe hoga. Cloudflare Worker + R2 sirf encrypted blob store karenge. Purana web VPS par app v2 complete hone tak chalega; blob-based web client nahi banega.
Data abhi 156 KB / 489 rows hai, isliye ye realistic hai.

Recovery Kit: 24-word phrase, kagaz pe, locker me. Digital kahin nahi.
Master password bhoola aur kit kho gayi = data hamesha ke liye gaya. Koi recover nahi kar sakta.

## Capture - sab owner ke haath se, 3 second me
1. Floating bubble - screen ke kinare, hamesha. Tap karo, amount daalo, done.
   Context-aware: Swiggy me bubble tap kiya to merchant/category/account pehle se bhare honge, sirf amount daalna hoga.
   Context sirf tap se aata hai, kisi taak-jhaank se nahi.
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
- Atomic write: temp file likho phir rename
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

Phase 3 - Android app v1
Bubble + widget + QS tile + share sheet + Pending inbox + BiometricPrompt + BIOMETRIC_STRONG + Keystore/StrongBox, fingerprint primary.
Sideload, Play Store nahi. Samsung S25U + doosra Samsung.

Phase 3 - Android app v1 (details)

Quality bar:
- World-class native app. Kotlin + Jetpack Compose, Material 3. Koi WebView wrapper ya hybrid nahi.
- 120Hz animations (Samsung S25U), haptic feedback, instant open (zero loading screen), gesture navigation, One UI ke saath fit.
- Offline-first: local DB phone pe, sync background me.

Context ka rule:
- Foreground app detect karna mana hai. Uske liye Usage Access ya Accessibility chahiye; ye permissions hum nahi lenge.
- Context sirf do tareeke se aayega: share sheet (payment screenshot share karna) aur user-made templates (bubble me Swiggy/Blinkit ka shortcut; tap se merchant + category bhar jaye).

Design process (mandatory):
- Build se pehle owner ko clickable prototype dikhana hai, screen by screen.
- Owner approve karega tabhi Kotlin me build shuru hoga.
- Bina approval ke koi screen banana allowed nahi.

Scope discipline:
- v1 me sirf capture: bubble, widget, QS tile, share sheet, pending inbox, quick add, biometric unlock.
- Reports, settings aur reconcile v1 me NAHI. Wo app v2 me aayenge; tab tak current web pe rahenge.
- Tax v1/v2 scope se alag hai. E8 ki wajah se uska faisla baad me hoga.
- Sab ek saath banane ki koshish mat karna.

Web ka kya:
- Current web VPS par app v1 aur app v2 ke build ke dauran chalta rahega.
- App v2 me reports, settings aur reconcile poore hone zaroori hain. Isse pehle VPS se V-Wealth nahi hatega, warna ye features kho jayenge.
- App v2 owner test aur approve karega; uske baad CSV export verify karke owner approval se VPS ka V-Wealth hata diya jayega.
- Blob-based web client nahi banega.
- Tax alag rahega; E8 ke baad owner uska faisla karega.

Health monitoring:
Sirf phone se monitoring nahi chalegi - phone mara ya app crash hua to alert kaun bhejega. Do hisse:
- Server se: privacy-safe external uptime monitor + non-financial signed backup heartbeat
- Phone se: sync aur outbox ke alerts
Nox me yahi galti ho chuki thi - 16 din tak pipeline mari padi thi aur koi alert nahi aaya. Wo dobara nahi honi chahiye.

Delivery:
- APK sideload hoga, Play Store nahi. Owner khud install karega.
- Har build ke baad owner phone pe test karega. Codex screen nahi dekh sakta.

### APK supply chain
Pehla APK banane se pehle tay karna:
- Offline signing key
- Signed update manifest
- Certificate pinning
- Rollback prevention
- Local decrypted DB ki protection: Keystore-wrapped key, Android backup se exclude, logs/crash/screenshot me leak na ho

Timeline - ek hafta:
- Din 1 - Worker + R2 pe encrypted blob endpoint (revision + CAS ke saath)
- Din 2 - Prototype, owner ka approval
- Din 3-5 - App: local DB, sync, capture (bubble, templates, share sheet, pending inbox), biometric
- Din 6 - Doosre phone pe sync test, naya phone restore test, Recovery Kit
- Din 7 - Polish: animations, haptics, bugs
- Web VPS par app v1 aur app v2 poore hone tak chalta rahega. VPS removal app v2 completion, owner testing, CSV verification aur explicit owner approval ke baad hi hoga.

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
4. Encryption ke saath 7 din me poora app - GALAT. Web migration alag kaam hai, uske liye alag samay chahiye.
5. Foreground app detect karke context lena - MANA HAI. Usage Access/Accessibility nahi lenge.
6. Face unlock - Android me crypto-grade nahi. Fingerprint hi.
7. Noxyaari ko Worker pe le jaana - NAHI HO SAKTA. Wo Telegram se lagatar juda rehta hai, Worker pe lambe chalne wale kaam nahi hote.

## Khule sawaal jo abhi tay nahi hue
- Blob file format version, AEAD algorithm, nonce rules, Argon2 parameters, KDF upgrade path
- Recovery phrase standard entropy/checksum se banega (BIP39 jaisa) - apna scheme invent nahi karna
- Device enrollment, device signing keys aur revocation ka protocol - master password authentication nahi hai
- Web migration/cutover plan - plaintext web aur encrypted blob parallel chalaye to do sources of truth ban jayenge
- Browser me decrypted vault ka XSS/supply-chain risk - E2EE malicious frontend JS se nahi bachata
- Purane plaintext DB aur backups ka verified deletion ya re-encryption - iske bina D6 khula rahega
- Golden vectors drift pakdenge, rokenge nahi. JS aur Kotlin me same financial rules maintain karna ongoing kaam hai.
- Storage usage alert ke liye owner-defined logical budget/quota ka exact number

Phase 3.5 - Android app v2 - VPS removal se pehle mandatory
- Reports
- Settings
- Reconcile
- Owner phone testing aur approval
- CSV archive verification
- Tax is scope me nahi; E8 ka faisla alag hoga
- App v2 poora hone se pehle VPS se V-Wealth nahi hatana

Phase 4 - Bhoolna band
Recurring, balance reconcile, nudge.

Phase 5 - Asli feature
- Liabilities (credit card, loan, udhaar) - abhi net worth jhootha hai, karz ghata hi nahi
- Audit trail - entry edit karte hi purani value mit jati hai
- Entry ko galat account se sahi account me move karna

Phase 6 - Maza
Motivation, streak, net worth widget.

## Audit se bacha hua kaam
- E8: tax calculation galat hai - standard deduction har income pe lagti hai, marginal relief nahi hai, crypto gains count nahi hote. Poora rewrite chahiye.
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
