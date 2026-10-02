> 🔒 **Distribution:** `moha-page-pilot.zip` — password-protected official archive. Extract with your PIN, then `chrome://extensions` → Developer mode → Load unpacked.

---

# Moha Page Pilot — by Moha Sumon

All-in-one Facebook page automation suite (Chrome extension, Manifest V3).

**Support / Contact:** WhatsApp [+971526373563](https://wa.me/971526373563)

---

## What each module does

### 1. Bulk Reel Uploader
- Add video files + one caption template (+ optional schedule time), tick target pages, hit **START BULK UPLOAD**.
- Uploads serially: each video × each page opens `facebook.com/reels/create`, attaches the file, fills the caption, and publishes.
- **Pacing:** random 3–8 min delay between uploads (configurable in Settings).
- **Safety:** the driver verifies the composer's posting identity matches the target page **before and right before** publishing. If it can't confirm, it aborts that item with a clear log instead of posting to the wrong page.
- Progress bar, per-item status, STOP button, run log persisted in `chrome.storage.local`.
- Hashtags in captions are auto-trimmed to max 3.

### 2. Auto Comment Reply Bot
- Needs your own AI API key (OpenAI-compatible endpoint, set in Settings).
- One click scans recent posts of selected pages, lists unreplied comments, and posts a warm **short** reply matching the comment's tone/language (English default, Bengali for Bengali comments).
- Skips spam/begging/scam comments (heuristic pre-filter + the AI is instructed to return `SKIP`).
- Max 1 reply per comment, configurable max replies per run (default 20), 20–45s human-like gap between replies, STOP button, full log.
- Verifies the comment voice is the **page** (not the personal profile) before every reply.

### 3. All-in-One Video Downloader
- On facebook.com, instagram.com, tiktok.com, youtube.com a small **Download** button appears bottom-right when a video is on the page.
- Saves the highest-quality MP4 the site exposes. TikTok: tries the no-watermark source first, falls back to watermarked with a note.
- Every outcome is shown in an on-page toast — failures are never silent.

### 4. Caption + Hashtag Generator
- Enter a topic, pick English/Bengali, hit GENERATE (uses your AI key).
- Returns: SEO title, 1–2 line description, hashtags with a **hard cap of 3** (enforced again client-side). Copy button per field.

### 5. Page Health Checker
- For selected pages, reads the Professional Dashboard monetization surface with your own login and renders a red/yellow/green table (monetization + policy + notes).
- Anything unreadable shows **"unknown — check manually"** — statuses are never invented.

---

## The full-screen dashboard (v1.1.1)

**v1.4.6 — Layout reshuffle.** (1) The bottom animation panel is removed. (2) The animated logo ident (rotating light-beam, shine sweep, waving MOHA SUMON letters, typing tagline, LIVE badge, news ticker) now lives at the top of the right-side terminal — built once as static HTML so animations never restart when logs stream. (3) The middle NIGHTHAWK code panel is now full-height (top to bottom, no gaps) with the feed stretching to fill.

**v1.4.5 — Animated broadcast ident.** Under the NIGHTHAWK panel (the empty area you circled): a news-channel-style animated logo ident — your logo with a rotating light-beam border, slow Ken Burns zoom, periodic shine sweep, waving MOHA SUMON letters, a typing tagline loop, a blinking red LIVE badge, and a scrolling news ticker at the bottom. Pure visual, no function.

**v1.4.4 — NIGHTHAWK ops panel.** The empty middle gap on the Overview page is now filled with a decorative hacker-ops panel: live-scrolling code lines (handshakes, relays, packet routes), animated stat tiles (relays/throughput/uptime/threats) and a rotating hex dump. It is a pure SIM FEED visual (badged as such) — it does no work; all real work and readable details stay in the right-side terminal as before.

**v1.4.3 — Logo + binary stealth mode in the terminal.** The ASCII banner (which wrapped badly on narrow screens) is replaced by your real logo image + glowing MOHA PAGE PILOT title at the top of the terminal, plus a logo in the terminal title bar. New **STEALTH** toggle: when on, the console shows Matrix-style binary noise (010101…) so nobody reading the screen can understand anything — while all real logs keep flowing to local storage in the background. Toggle it off anytime to debug with readable logs.

**v1.4.2 — Kali-style hacker terminal.** The right-side live console is now a green-on-black terminal: ASCII MOHA PAGE PILOT banner, scanline/CRT glow styling, `root@mohapilot:~#` prompt with blinking cursor, and a live intel ticker (every 12s, session-only) streaming real status — session heartbeat, armed scheduler jobs, local vault size, policy-shield watch count.

**v1.4.1 — Page-name fix.** Facebook's page-row `aria-label` ("Profile picture for &lt;Name&gt;") was being saved as the page name. New `MPP.cleanPageName()` strips the "Profile picture for / Cover photo for" prefix at detection time (background extractor), on every display (My Pages list, merge/scheduler/cross-post dropdowns), plus a one-time migration that cleans already-saved names in storage.

**v1.4.0 — Page Merge module (15 total).** **Page Merge** (tag `merge`, storage `mpp_merge`): Source + Target page selectors from My Pages → pre-flight checklist (login ID, src≠dst, word-overlap name-similarity score ported from the standalone Moha Page Merge v1.0.8, published/same-admin/BM warnings) with a confirmation checkbox and a low-similarity second-click warning → START MERGE opens `facebook.com/pages/merge/` in a hidden tab (background, message `MPP_BG_MERGE_FILL`; web.facebook.com bounce is forced back to www) and injects the patient auto-fill `mergeFillInDomPatient`, which finds the two React dropdowns and selects the pages by normalized name/ID match. The tab is left OPEN on any actionable outcome and revealed to the user — the final **Continue + Request Merge** click is always the user's (never automated). STOP button, step-by-step log, merge history (last 100, deletable). Honest limits shown in a red banner: "Sorry, something went wrong" at re-auth is a **server-side refusal — no tool can bypass it** (remedy: Page Settings → Merge Pages manually, another admin ID, or Facebook support); one pair at a time; bulk merging = account risk.

**v1.3.0 — 5 new modules (14 total).** (1) **Policy Monitor** (tag `policy`, storage `mpp_policy`): "SCAN NOW" over checked pages (~5s pacing) — hidden tab professional_dashboard + patient extractor `extractPolicyFromDomPatient` for violation / restriction / policy-issue / limited-distribution / not-eligible / Page Quality / account-warning / content-removed / demonetization markers; status Clean / Flagged / Unknown per page. NEW flags vs the previous scan trigger a `chrome.notifications` alert + red row highlight; optional once-a-day auto-scan via `chrome.alarms` (`MPP_BG_POLICY_AUTOSCAN` toggle, persisted in settings). (2) **Earnings Tracker** (tag `earn`, storage `mpp_earnings`): per-page, per-month editable earnings ($) + payout status dropdown (Pending/Paid/On hold); auto total row + progress bar toward the goal from Settings (new `earnGoal` setting, default $10,000). UI states honestly: "Facebook doesn't expose per-page earnings to extensions — enter from your payout dashboard." (3) **Cross-Posting** (tag `xpost`, storage `mpp_xpost`): job = page + file-name note + caption (max 3 hashtags enforced) → per-platform buttons open the real upload page in a new tab (YouTube `/upload`, TikTok `/upload`, Instagram home — no stable direct web-upload URL) with the caption+hashtags copied to clipboard; per-platform Pending/Done tracking with "Mark done"; UI text is explicit: "you finish the last click." (4) **Bulk Page Settings** (tag `psettings`): bio text + profile-pic + cover files applied to checked pages (~8s pacing, STOP button) via hidden-tab injected workers `applyBioInDom` / `applyPhotosInDom` (file attached via DataTransfer from a data URL); every step logs OK/FAIL with the reason in a per-page result log — best-effort, no blind retries. (5) **Page Creator** (tag `pcreate`, storage `mpp_created_pages`): base name + auto-numbering ("Name 1", "Name 2"…), category select, count (max 20) → hidden-tab `facebook.com/pages/create/` flow via `createPageInDomPatient`, paced 60–120s, STOP button, per-attempt log; MANDATORY red warning that Facebook rate-limits creation and "unlimited" is impossible; the run stops automatically on any checkpoint/rate-limit text (regex-tested) and says so plainly. All five follow the hard rule: hidden-tab + rendered-DOM only, dashboard → background via `MPP_BG_*` directly.

**v1.2.0 — 4 new modules.** (1) **Monetization** (tag `money`, storage `mpp_monetization`): table of My Pages with Followers / Monetization status / Policy flags / Last checked; "CHECK SELECTED" scans each checked page's professional_dashboard in a hidden tab (~4s pacing) with the patient extractor `extractMonetizationFromDomPatient`, which reads each program row's OWN line (In-stream ads, Ads on Reels, Stars, Bonuses, Subscriptions) for Eligible / Not eligible / Restricted / Policy issue / Under review, then classifies the page as Eligible / Partially eligible / Not eligible / Policy issue / Unknown. Followers come from the latest Analytics snapshot. (2) **Scheduler** (tag `sched`, storage `mpp_scheduled`): page dropdown + video file + caption + hashtags (max 3 enforced) + datetime → `MPP_BG_SCHEDULE`; background creates a `chrome.alarms` alarm (new `alarms` + `notifications` permissions); on fire it shows a notification, stores the job as `mpp_pending_job`, and opens the dashboard, which prefills the Bulk Upload caption/hashtags — the video file must be re-picked (browser security, noted in the UI). "Test alarm in 1 min" included. (3) **Comment Inbox** (tag `inbox`, storage `mpp_inbox` / `mpp_inbox_replied`): "SCAN COMMENTS" reuses `content/drivers/comments.js` per checked page (hidden tabs, 8s pacing) and renders an inbox with inline quick-reply boxes; replies reuse `content/drivers/reply.js` (page voice verified) and mark the comment replied. (4) **Analytics** (tag `stats`, storage `mpp_analytics`): hidden-tab professional_dashboard scan per page (~4s pacing) with `extractStatsFromDomPatient` for followers / 28-day reach / engagement — raw strings or "—", never invented; keeps last 30 snapshots per page. All four follow the hard rule: no `fetch()` to facebook.com — hidden-tab + rendered-DOM only; dashboard talks to the background directly via `MPP_BG_*` messages.

**v1.1.4 fix:** fetch-scraping AND the 3s hidden-tab fallback both still returned 0 BMs on the live machine, so detection moved fully to rendered-DOM strategies. (1) **Patient extractor:** `extractBMsFromDomPatient` (injected via `chrome.scripting.executeScript`, closure-free, timeout passed via `args`) polls the rendered DOM every 1s up to 30s for business markers — links with `?business_id=`, `data-testid` business candidates, anchors on business.facebook.com with a 6+-digit id — and resolves the moment ≥1 business appears. (2) **New fallback order:** fetch scrape → `select_business/` hidden-tab DOM scan (Facebook's light business picker page) → `settings/` hidden-tab DOM scan; the tab is always closed in a `finally` block. (3) **New "Detect from open tab" button:** guaranteed path — open `business.facebook.com` in any tab, click it, and the extractor runs against that already-rendered page (message `MPP_BG_GET_BMS_OPEN_TAB`). Every stage is logged to the live console (tag `pages`); diagnostics + manual fallback unchanged.

**v1.1.3 fix:** dashboard/popup showed a hardcoded "v1.1.0" — now renders the real version from the manifest at runtime.

**v1.1.2 fix:** BM auto-detect still returned zero on the live machine, so two things were added. (1) **Diagnostics:** every auto-detect run records per-surface url / HTTP status / final URL / HTML length / per-regex hit counts to `mpp_bm_diag` in storage — the dashboard's **"Copy diagnostics"** button copies it for support; the failure note is now a one-line summary. (2) **Hidden-tab DOM fallback:** when the fetch scrape finds 0 BMs, the background opens `business.facebook.com/settings/` in a hidden tab, scans the business switcher DOM (links with `?business_id=`, `data-testid` business candidates) with `chrome.scripting.executeScript`, then **always closes the tab** in a `finally` block. Each fallback step is logged to the live console. Manual-add fallback unchanged.

**v1.1.1 fix:** BM auto-detect moved into the background service worker — content scripts on www.facebook.com cannot fetch business.facebook.com (cross-origin, no CORS headers), which is why "No Business Managers found" appeared. 4 scrape surfaces + expanded id/name patterns; manual-add fallback unchanged.

Click the extension icon → **OPEN FULL DASHBOARD**. The popup is now just a launcher + mini status; everything moved into the dashboard tab:

- **Left nav rail** — Overview, Bulk Upload, Auto Replies, Downloader, Captions, Health Check — each module gets its own full-width section.
- **Overview** — login status; **Business Managers auto-load** right after login detect (best-effort scrape of business.facebook.com — BM names are paired by order, verify the BM ID before bulk runs; manual BM-ID add box included). Click a BM to expand **all its pages**; each page row has **Add** / **Remove** to build your persisted **"My Pages" working set** ("Add all" per BM). All 5 modules operate on the **checked** pages in My Pages.
- **Live console (right side, terminal style)** — every action streams here live: login detects, BM/page loads, upload progress per page, replies posted, downloads, errors. Persisted (last 500 entries) and restored when the dashboard opens. All logging flows through the one shared `MPP.log()` / `MPP.unifiedLog()` helper in `lib/utils.js`, including fire-and-forget `MPP_LOG_ALL` posts from the content-script drivers.
- Keep the **dashboard tab open** during upload/reply/health runs — closing it stops the run.

## How to load (unpacked) in Chrome

1. Unzip `moha-page-pilot-v1.zip` (or use the source folder directly).
2. Open `chrome://extensions`, enable **Developer mode** (top-right).
3. Click **Load unpacked** → select the `moha-page-pilot` folder.
4. Pin the extension (puzzle icon → pin "Moha Page Pilot").

## Setup steps

1. Click the extension icon → gear icon (Settings).
2. (Optional) Enter your **BM ID** — this only *scopes/filters* the page list. It does not grant any access.
3. Enter your **AI endpoint + API key + model** (needed for modules 2 and 4) → **Test connection**. The key is stored only in `chrome.storage.local` and sent only to your endpoint.
4. Set pacing (default 3–8 min) and max replies per run (default 20) → **Save**.
5. Open **facebook.com** in a tab and log in. Then click the extension icon → **OPEN FULL DASHBOARD**.
6. In the dashboard Overview: login is detected automatically, then the **Business Manager list loads automatically**. Expand a BM → Add pages to **My Pages** (or add page IDs manually). Check the pages each module should use.
7. Use the 5 modules from the left rail. Watch everything in the **live console** on the right. Keep the dashboard tab open during bulk runs — closing it stops the run.

---

## Honest limitations — read before running bulk uploads

1. **Session-based automation carries real ban/restriction risk.** Driving facebook.com UI with a script violates Meta's automation policies. Random pacing lowers the risk but does **not** remove it. Start with 1–2 pages and small batches; never run aggressive automation on your main earning assets without accepting the risk.
2. **The extension cannot conjure permissions.** It acts as *your logged-in account* and can only touch pages/BM assets that account already administers. "BM ID access" = scoping/filtering the page list, **not** privilege escalation.
3. **Facebook's markup changes constantly.** Page auto-discovery, comment scanning, dashboard reads and the reel composer flow are all best-effort DOM automation — they can break or return partial data. Unreadable data is reported as "unknown — check manually", never guessed. The manual page-add box always works.
4. **Downloader quality depends on the platform.** DRM/encrypted/private videos cannot be fetched directly — most YouTube videos will show an honest "not available" message. TikTok no-watermark sources are frequently blocked; the watermarked fallback is used with a note.
5. **Scheduling is best-effort.** If the composer exposes a schedule control it is used; otherwise the reel publishes immediately and the log says so.
6. **The dashboard tab must stay open** during upload/reply/health runs (the dashboard holds the video blobs and pacing timers; the MV3 service worker may be suspended between delays).
7. **The legitimate long-term path** for bulk posting is a Meta App with `pages_manage_posts` (and related) permissions via OAuth + Meta app review — that's the v2 direction, not UI automation.

## Privacy

- No analytics. No external calls except: (a) your own configured AI endpoint, (b) facebook.com / instagram.com / tiktok.com / youtube.com pages you visit.
- Secrets (AI key) live only in `chrome.storage.local`.

## Files

```
manifest.json          MV3 manifest (v1.4.0)
popup.html/js          Slim launcher + mini status (opens the dashboard)
popup.css              Shared popup styles
dashboard.html/css/js  Full-screen dashboard: 15 module sections, BM → pages
                       manager, My Pages working set, live work console
options.html/js        Settings page
background.js          Service worker (login detect via c_user cookie, downloads, relay)
lib/utils.js           Shared helpers (hashtag cap, pacing, logging) —
                       MPP.log() feeds the unified live console (MPP.unifiedLog,
                       cap 500, persisted)
lib/ai.js              OpenAI-compatible API helper
content/fb_detect.js   Login detect + page/BM list scraping + BM auto-list
                       (MPP_GET_BMS), best-effort, with console hooks
content/downloader.js  Download button overlay for FB/IG/TikTok/YouTube
content/drivers/       On-demand automation drivers:
  upload.js              reel composer driver (identity-verified)
  comments.js            comment scanner
  reply.js               reply poster (voice-verified)
  health.js              dashboard health reader
icons/                 MS-branded icons (16/48/128)
```
