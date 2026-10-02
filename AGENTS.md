# MyHobbyPlan Website - Project Context

> Read this before editing. The Android app is the source of truth for what the
> product actually does; this file records how the site presents it.

## Overview
- **Project**: MyHobbyPlan - weather-aware hobby planning and scheduling app
- **Type**: Static landing page website (HTML/CSS/JS), no build step
- **Hosted**: GitHub Pages
- **Owner**: Eleviq Technologies (https://kigechaedward.github.io/eleviq-website/#/)
- **Landing Page URL**: https://kigechaedward.github.io/Myhobbyplan-website/
- **Sibling app project**: `C:\Users\kigec\AndroidStudioProjects\MyHobbyPlan`
  — **not a git repo**, so there is no history to diff against. Verify claims by
  reading `app/src/` directly.
  - `applicationId` / Play Store id: **`binarybytez.net.myhobbyplan`**
  - `namespace` + Kotlin source root: `com.example.myhobbyplan` (mismatch is
    intentional; every Play link must use the applicationId)
  - `versionName` **4.6**, `versionCode` 34, minSdk 26, target/compileSdk 36
  - Single `:app` module — the Wear OS tile and Android Auto screen live inside
    it, there is no `:wear` or `:automotive` module
  - Secrets come from `local.properties` (`OPENWEATHER_API_KEY`, `MAPS_API_KEY`);
    never copy them into this repo

## Purpose
- Landing page for the MyHobbyPlan Android app: explains the problem, shows the
  product, states which features ship today vs. are on the roadmap, and links to
  download. Legal pages cover privacy and terms.

## Tech Stack
- Pure HTML, CSS, JavaScript. No frameworks, no bundler, no npm.
- Google Fonts (Sora display, Inter body)
- Three.js **r160 UMD build** loaded from `https://unpkg.com/three@0.160.0/build/three.min.js`
  (deprecated file — it still resolves, but prints a console warning; do not
  "upgrade" it casually, the whole site relies on the global `THREE` object)
- Formspree for the contact form
- GitHub Pages via `.github/workflows/deploy-pages.yml` (publishes `docs/`)

## File Structure
```
├── docs/                          # GitHub Pages root
│   ├── index.html                 # Main landing page (single file: CSS + JS inline)
│   ├── pitch.html                 # Investor pitch deck page
│   ├── pitch-deck.pdf             # Downloadable pitch deck PDF
│   ├── privacy.html               # Privacy policy
│   ├── terms.html                 # Terms and conditions
│   ├── app-ads.txt                # Ad configuration (also at repo root)
│   ├── logo.png / favicon.png     # Brand assets
│   ├── screenshot.png             # App screenshot — used by pitch.html ONLY
│   ├── weatheraware_planning.png  # Feature screenshot
│   ├── smart_scheduling.png       # Feature screenshot
│   ├── hobby_management.png       # Feature screenshot
│   ├── personalized_preferences.png
│   ├── smart_notifications.png
│   └── family_friendly.png        # NOTE: stale asset, no longer referenced
├── .github/workflows/
│   └── deploy-pages.yml
└── app-ads.txt
```

## Design System — MUST match the app
The site's palette is a direct port of the app's Cyberpunk theme. Source of truth:
`app/src/main/java/com/example/myhobbyplan/ui/theme/Color.kt`. When the app palette
changes, update the `:root` block in `docs/index.html` to match.

Token names were deliberately renamed from the old navy scheme. The current
mapping, and the app colour each one mirrors:

| Token        | Value     | App colour                          |
|--------------|-----------|-------------------------------------|
| `--bg`       | `#070A12` | `DarkBackground` (ultra-dark void)  |
| `--bg-2/3`   | `#0B1018` / `#0F1624` | section alternation        |
| `--surface`  | `#111827` | `DarkSurface`                       |
| `--surface-2`| `#1E293B` | `DarkSurfaceElevated`               |
| `--text`     | `#FFFFFF` | `DarkTextPrimary`                   |
| `--text-2`   | `#94A3B8` | `DarkTextSecondary`                 |
| `--text-3`   | `#64748B` | neutral slate                       |
| `--lime`     | `#CCFF00` | `AccentBlue` — primary CTA          |
| `--lime-soft`| `#E6FF8F` | tint for headings / hover           |
| `--cyan`     | `#00E5FF` | `AccentMint` — secondary + metrics  |
| `--jade`     | `#00E676` | `GoodGreen` — positive / "Live"     |
| `--amber`    | `#FFD600` | `CautionAmber` — in-development     |
| `--coral`    | `#FF3D71` | `DangerRed` — alerts                |
| `--violet`   | `#7C3AED` | `GradientEnd` — brand tail          |

Notes:
- Text on lime/cyan gradients is `#070A12` (the app's `OnAccent`), never white.
- The app calls lime `AccentBlue` and cyan `AccentPurple`. That naming is
  misleading; on the website use `--lime` / `--cyan`.
- Hardcoded colours still exist outside `:root` in the Three.js hero scene as
  `0xRRGGBB` literals. Keep those in sync when re-skinning.

## Product Accuracy — non-negotiable
The app has **no LLM, cloud model or ML SDK**. `app/build.gradle.kts` contains no
Gemini/OpenAI/ML Kit/TFLite/ONNX dependency, and the only outbound host is
`api.openweathermap.org`. Every "smart" feature is local rule-based scoring in
`domain/engine/` and `domain/usecase/EvaluateHobbyWeatherUseCase.kt`, driven by
OpenWeather data plus the device barometer. The app says so itself in
`NotificationInsightEngine.kt`: *"AI in this app means local heuristic scoring
rather than a hosted model."* **Never market the app as using AI/LLMs.** Use
"smart forecasting", "weather-aware" or "on-device scoring".

Verified against app v4.6 — safe to claim:
- Per-hobby weather scoring over temp / wind / gust / rain / sun / UV, with a
  **six**-label scale: Optimal → Good → Moderate → Poor → Not Recommended, plus
  a transient "Pending" state when the nearest hourly slot is >2 h away
  (`EvaluateHobbyWeatherUseCase.kt`)
- OpenWeather **One Call 3.0** with a 2.5 forecast/current fallback, plus
  city search and reverse geocoding; per-hobby coordinates are supported
- Nine smart-notification insight kinds (`GOOD_TO_GO`, `BETTER_WINDOW`,
  `GEAR_NUDGE`, `WEATHER_WARNING`, `STORM_WARNING`, `CONDITIONS_CLEARING`,
  `REMINDER`, `PLAN_AHEAD`, `REENGAGE`) across four independent toggles (smart,
  alerts, reminders, planning), de-duplicated once per day with a 3-day log
- Barometric alerts from a 3-hour pressure delta on a 48-hour local history:
  storm warning at a 1.0 hPa drop, conditions-clearing at a 1.5 hPa rise
- Better-window alerts only fire when the alternative window genuinely wins
  (target ≥ 4.5 and ≥ 2.0 better) — the user always reschedules
- Firebase Auth + Firestore cloud sync with a realtime snapshot listener, and a
  first-run push/pull migration on sign-in
- Shared missions: 6-character invite codes, ORGANIZER / CO_PILOT / MEMBER roles,
  a live comms thread, and hourly mission-update notifications
- Wear OS **"NEXT HOBBY"** tile (legacy `TileService`, tile only — there is no
  watch app UI) and an Android Auto screen registered under the
  `androidx.car.app.category.WEATHER` category
- Three Glance home-screen widgets (Today Hobby, Tomorrow Hobby, Pro
  Intelligence) — real and shipped, but not yet on the site
- Weather-protocol sharing as a rendered PNG card plus a `myhobbyplan://protocol`
  deep-link import
- 20 languages (`values` + 19 locales, full `strings.xml` parity) with RTL
  support and adaptive phone/tablet layouts
- Pro: hobby cap 5 → unlimited, forecast 3 → 14 days, smart suggestions
  5/month → unlimited, daily briefing card, weekly briefing, plan-ahead
  horizon 3 → 7 days

Not implemented — do not claim:
- Streaks, milestones or habit tracking (zero occurrences in `app/src`)
- Family or child profiles / parental monitoring
- GPS route tracking for runs (no route entity, no `ACTIVITY_RECOGNITION`;
  "journey weather" is just home / midpoint / destination lookups)
- A conversational in-app assistant — `PreparationAssistant` is a static rule
  list, and the only chat is the human-to-human mission thread
- Automatic rescheduling; the app *suggests* a better window, the user acts
- A live community-learning / "AI intelligence" backend. The community
  dashboard is **mock data**: `CommunityViewModel` hard-codes four fake users
- Local Settings > Backup export/restore. The only "backup" is OS-level
  Android Auto Backup with untouched template rule files
- A conversational assistant screen: `HobbyExplorerScreen` still contains a
  literal "AI analysis for hobby ID … will appear here" placeholder
- Anything server-side: `domain/appfunctions/` is an empty stub even though
  `BIND_APP_FUNCTION_SERVICE` is declared in the manifest

Traps to keep in mind:
- The **in-app UI still says "AI"** in several strings (widget label "Pro AI
  Intelligence", "WEEKLY AI BRIEFING", "AI RECOMMENDATIONS", "Monthly AI Limit
  Reached"). Those are legacy labels, not features. This is exactly why the site
  must not repeat them.
- Localisation is only *structurally* complete — `SettingsScreen`,
  `PlannerScreen`, `DiscoverScreen`, `ProUpgradeScreen`, `SocialScreen` and
  `SharedMissionScreen` still contain hard-coded English literals. Don't promise
  a fully localised UI.
- "Ad-free" is a weak claim: there is no ads SDK in the app at all
  (`playServicesAds` is declared in `libs.versions.toml` but never wired up), so
  the free tier simply shows no ads.

## Known Drift — open issues on the live site
Verified against the app, not yet fixed in `docs/`. Fix before publishing.
- **Pro benefits are overstated.** The Pro table lists "Unlimited smart
  notifications" and "Smart forecasting", but the notification system and
  `PreparationAssistant` are **not** gated on `isPro` at all — every toggle works
  for free users. Only the six gates listed above are real.
- **Free forecast is 3 days, not 7.** (`WeatherScreen.kt` even reuses a string
  named `seven_day_forecast` for the 3-day cap.)
- **Location wording contradicts itself.** The app *does* declare
  `ACCESS_COARSE_LOCATION` + `ACCESS_FINE_LOCATION` and offers one-tap GPS
  detect on onboarding and in Settings, while `privacy.html` states "We do not
  track your real-time GPS location. You manually select a city". Correct copy
  is: optional one-tap GPS to set your city, or search any city by name —
  used only to resolve a weather coordinate, never tracked in the background.
- **The FAQ chatbot still contradicts the rest of the site.** Its `location`
  entry says the app "uses your device location", its `pro` entry says Pro "is in
  development" (billing is already wired: SKU `myhobbyplan_pro`, monthly and
  yearly), and the "Key features" entry needs the real feature list.
- The `streak` / `weekly summary` chatbot answers were reworded rather than
  deleted, so a user asking about streaks still gets a non-answer. Acceptable,
  but do not reintroduce streak language.

## Site Structure (order matters for the nav)
`#problem` → `#solution` → `#how-it-works` → `#weather` → `#features` → `#ai`
(now "Smart forecasting") → `#prep` → `#pro` → `#experience` → `#ecosystem` →
`#why` → `#vision` → `#company` → `#download` → `#contact`

## Key Features
- Responsive layout, sticky glassmorphism nav, mobile hamburger menu
- Three.js 3D hero scene (planet, stars, chip, calendar, bike, rain) + Intersection
  Observer scroll reveals. There is **no CSS fallback image** any more — if
  WebGL, `prefers-reduced-motion` or the CDN fails, the hero is just gradients.
- Forecast-condition cards, timeline, orbital ecosystem diagram
- AI-free FAQ chatbot (keyword matching against a hardcoded array in the inline JS)
- Contact form via Formspree, FAQ widget, privacy and terms pages
- SEO: meta tags, Open Graph, JSON-LD (`SoftwareApplication` + `Organization`)
- Site language picker (translates the site chrome only — it does not mirror the
  app's 20 in-app languages)

## Development Notes
- `docs/index.html` is ~3000 lines with `<style>` and `<script>` inline. There is
  no separate CSS or JS file.
- UTF-8, no BOM, LF line endings. Preserve all three when rewriting the file —
  PowerShell edits must use `[System.IO.File]::WriteAllText` with
  `New-Object System.Text.UTF8Encoding($false)`.
- `.chat-demo .bubble:nth-child(n)` counts `.ai-pulse` and `.chat-demo-head` as
  children 1 and 2, so bubble delays start at `nth-child(3)`.
- The hero canvas and its old fallback were siblings, hidden via
  `.hero-canvas.ready ~ .hero-fallback`. That rule set is gone — don't reintroduce
  a sibling-based hero fallback, it flashes over the headline while Three.js loads.
- `@keyframes float-y` is shared by several elements; don't delete it with the
  hero mockup.
- Verify palette edits mechanically before finishing: no unused `--token`
  definitions, no `var(--x)` without a matching `--x:`, no duplicate custom
  property keys, and no leftover pre-skin hex values.
- Check content claims against the app before adding them (see above).

## Common Tasks
- Edit landing page: `docs/index.html`
- Edit pitch deck: `docs/pitch.html`
- Edit privacy policy: `docs/privacy.html`
- Edit terms: `docs/terms.html`
- Update logo/screenshots: replace files in `docs/`
- Add a page: create in `docs/`, then link it in `docs/index.html`
- Re-skin: change `:root` in `docs/index.html` **and** the `0x` literals in the
  Three.js block

## Recent Changes
- **Removed the hero screenshot overlay.** The centered phone mockup wrapping
  `screenshot.png` (`.hero-fallback`) was painted over the hero intro at
  `z-index: 1` and stayed visible until Three.js finished loading from unpkg —
  it read as a screenshot popping up on page load. Markup, its CSS
  (`.hero-fallback`, `.phone-mockup`, `.phone-frame`, `.phone-notch`,
  `.phone-screen`) and the `.hero-canvas.ready ~ .hero-fallback` hide rule were
  all deleted. `screenshot.png` is now referenced only by `pitch.html`.
- **Full app re-verification (v4.6)**: the Product Accuracy section above was
  rewritten from a fresh read of `app/src/`. Score scale is six labels, the app
  does request GPS permissions, Pro's real gates are now enumerated, and the
  mock community dashboard / empty `domain/appfunctions/` stub are called out.
- **Product accuracy pass (app v4.6 alignment)**:
  - Removed all AI/LLM claims. The "AI-Powered Future" section is now "Smart
    Forecasting"; nav links read "Smart". Meta keywords, OG description,
    JSON-LD `featureList` and the FAQ chatbot answers were updated to match.
  - Deleted fabricated features: streaks/milestones, family & child profiles,
    GPS route tracking, golden-hour alerts, local Settings > Backup export.
  - Removed the fictional AI chat conversation; it now shows real v4.6 smart
    notification samples (Good to go / Better window / Storm warning).
  - Promoted shipped features: cloud sync, Mission Control, Wear OS tile,
    Android Auto, 20 languages, barometric alerts.
  - Gear nudges relabelled from a "future preparation assistant" to a live feature.
- **Cyberpunk re-skin**: palette ported from the app's `Color.kt`
  (Neon Lime `#CCFF00` / Electric Cyan `#00E5FF`) across CSS tokens and the
  Three.js scene. Tokens renamed from `--sky`/`--green` to `--lime`/`--cyan`/`--jade`.
- Fixed the `.vcard` hover shadow (the `vcard` variant was missing `--shadow-card`).
- Removed a duplicate `--jade` custom property and several dead tokens.
- Fixed `privacy.html`: stripped stray `**` markdown, and documented optional
  account/cloud-sync data, shared-mission data and barometer sensor use.