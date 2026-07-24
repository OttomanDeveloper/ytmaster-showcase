# YT Master — YouTube Growth Community Platform

> Portfolio brief. Written to be machine-readable: everything a portfolio
> page needs (links, copy, facts) is in this file.

## One-liner

A two-sided YouTube growth marketplace: creators spend coins to run
watch-time and subscriber campaigns; community members earn those coins
by genuinely watching and subscribing — with real YouTube API
verification — all managed from a dedicated admin panel.

## Live demo

| Piece | URL | Access |
|---|---|---|
| Landing page | https://ottomandeveloper.github.io/ytmaster-showcase/ | public |
| User app (Flutter web) | https://ottomandeveloper.github.io/ytmaster-showcase/app/ | any Google account |
| Admin panel (Flutter web) | https://ottomandeveloper.github.io/ytmaster-showcase/admin/ | demo login below |

**Admin demo account (view-only, safe to publish):**
`demo@ytmaster.app` / `Demo@1234` — also shown on the login screen with
tap-to-sign-in. It can browse every screen; all writes are blocked both
client-side and by Firestore security rules.

Native targets: Android is the primary platform (Google Mobile Ads,
full feature set); the web builds above are the same codebase compiled
for browsers.

## What it does

**User app**
- **Watch & Earn** — embedded YouTube player pays coins only after the
  video has *measurably* played for the campaign's exact duration
  (event-stream playback-position tracking; pausing, seeking, stalling,
  backgrounding, or 2x speed earn nothing extra).
- **Subscribe & Earn** — claims are verified against the user's own
  YouTube account via the YouTube Data API (`subscriptions.list`)
  before any reward is credited. No honor system.
- **Create campaigns** — spend coins on views or subscriber campaigns;
  budgets settle atomically as other users consume them.
- **Referral system** — unique 6-char codes, transactional redemption
  crediting both sides, admin-configurable reward and kill-switch.
- **Coin store** — packages purchasable via local payment rails
  (Easypaisa / JazzCash / bank transfer) with an admin approval flow.
- **Channel marketplace**, watch-time/views/subscriber service catalog,
  order history, FAQ, light/dark theme.

**Admin panel**
- Live-KPI dashboard (Firestore `count()` aggregates), order
  review/approval, user management with referral stats, coin-package /
  campaign-pack / service-catalog editors, channel market moderation,
  payout-account management, and global app settings (ads IDs, referral
  program, links).
- Gated by Firebase Auth with a custom `admin` claim; includes the
  public view-only demo mode.

## Tech stack

- **Flutter 3** (one codebase → Android, iOS, web), Dart 3
- **Firebase**: Auth (Google sign-in + custom admin claims), Cloud
  Firestore, deployed security rules
- **YouTube Data API v3** (free quota) for subscription verification and
  channel resolution — deliberately architected to need **no paid
  backend** (no Cloud Functions / Blaze plan)
- `provider` state management, `go_router` URL-based navigation,
  `google_sign_in` incremental OAuth scopes, `youtube_player_iframe`
- Design system: `ThemeExtension` token architecture (single source of
  color truth), persisted light/dark/system theme, responsive
  breakpoint system (phone → tablet → desktop web)

## Engineering highlights (true stories, good interview material)

1. **Anti-cheat watch tracking** — reward timer counts *actual playback
   position deltas* capped by wall-clock time, delivered via the
   player's event stream (the polling API silently fails on Flutter
   web). Closed real exploits: timer stacking via rapid video-skipping,
   background farming, stalled-player credit.
2. **Free-tier subscription verification** — user's own OAuth token
   proves their subscription (`mine=true&forChannelId=…`), with channel
   IDs resolved from any URL shape (video / @handle / /channel/ /
   legacy) and cached back onto campaign docs to halve quota use.
3. **Firestore security hardening** — full audit of every DB call site
   in both codebases produced deployed rules: payout accounts, prices,
   and settings are admin-claim-only; the audit was smoke-tested with
   46 automated REST assertions across four identities (anonymous,
   user, demo, admin) — 46/46 passing.
4. **Transactional settlement** — campaign consumption, reward
   crediting, and referral redemption are single Firestore
   transactions; no lost updates between concurrent watchers.
5. **Tokenized theming migration** — legacy hardcoded-color codebase
   migrated to a ThemeExtension token system with a compatibility
   facade, so ~40 legacy files kept compiling untouched during the
   migration.

## Repository & source

Source lives in two **private** repositories
(`OttomanDeveloper/subforsubYT` — app, `OttomanDeveloper/subforSub_AdminPanel`
— admin). This public repo contains only compiled, minified web builds
(source maps stripped). Source available on request.

## Suggested portfolio copy (short)

> **YT Master** — a two-sided YouTube growth marketplace built with
> Flutter & Firebase. Users earn coins by watching and subscribing
> (verified through the YouTube Data API — no honor system) and spend
> them on their own growth campaigns. Includes a claim-gated admin
> panel with a public view-only demo, deployed Firestore security
> rules, and a zero-paid-backend architecture. [Live demo →](https://ottomandeveloper.github.io/ytmaster-showcase/)

## Facts for the portfolio agent

- role: solo developer (design, architecture, implementation, security, deployment)
- platform_targets: Android (primary), iOS, Web
- year: 2026 (major modernization & security overhaul)
- categories: mobile app, web app, SaaS admin panel, Firebase
- demo_note: admin demo is view-only by design; user app writes to a live demo database
- screenshots: take from the live URLs above (landing, app home, Watch & Earn, admin dashboard, admin login with demo card)
