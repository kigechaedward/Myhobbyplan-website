# MyHobbyPlan Website - Project Context

> Read this before editing. The Android app is the source of truth for what the
> product actually does; this file records how the site presents it.

## Overview
- **Project**: MyHobbyPlan - weather-aware hobby planning and scheduling app
- **Type**: Static landing page website (HTML/CSS/JS), no build step
- **Hosted**: GitHub Pages
- **Owner**: Eleviq Technologies (https://kigechaedward.github.io/eleviq-website/#/)
- **Landing Page URL**: https://kigechaedward.github.io/Myhobbyplan-website/
- **Sibling app repo**: `C:\Users\kigec\AndroidStudioProjects\MyHobbyPlan`
  (package `com.example.myhobbyplan`, version **4.6** / code 34)

## Purpose
- Landing page for the MyHobbyPlan Android app: explains the problem, shows the
  product, states which features ship today vs. are on the roadmap, and links to
  download. Legal pages cover privacy and terms.

## Tech Stack
- Pure HTML, CSS, JavaScript. No frameworks, no bundler, no npm.
- Google Fonts (Sora display, Inter body)
- Three.js (r128-style UMD build) for the 3D hero scene
- Formspree for the contact form
- GitHub Pages via `.github/workflows/deploy-pages.yml`

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
│   ├── screenshot.png             # Hero app screenshot
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
The app has **no LLM or cloud AI model**. Every "smart" feature is local rule-based
scoring (`SmartSuggestionEngine`, `DailyBriefingEngine`,
`NotificationInsightEngine`, `PreparationAssistant`) driven by OpenWeather data
plus the device barometer. **Never market the app as using AI/LLMs.** Use "smart
forecasting", "weather-aware" or "on-device scoring".

Verified against app v4.6 — safe to claim:
- Weather-aware per-hobby scoring; Optimal → Poor suitability
- Nine smart-notification insight kinds across four toggles (smart, alerts,
  reminders, planning), de-duplicated once per day
- Better-window alerts, gear nudges, barometric storm warnings, plan-ahead prompts
- Cloud sync across signed-in devices; shared missions with invite codes and comms
- Wear OS "Next Hobby" tile, Android Auto destination weather
- 20 languages, RTL support, adaptive phone/tablet layouts
- Pro: unlimited hobbies, 14-day forecast

Not implemented — do not claim:
- Streaks, milestones or habit tracking
- Family or child profiles / parental monitoring
- GPS route tracking for runs
- A conversational in-app assistant (the "assistant" visual is a stylised card)
- Automatic rescheduling; the app *suggests* a better window, the user reschedules

## Site Structure (order matters for the nav)
`#problem` → `#solution` → `#how-it-works` → `#weather` → `#features` → `#ai`
(now "Smart forecasting") → `#prep` → `#pro` → `#experience` → `#ecosystem` →
`#why` → `#vision` → `#company` → `#download` → `#contact`

## Key Features
- Responsive layout, sticky glassmorphism nav, mobile hamburger menu
- Three.js 3D hero scene (planet, stars, chip, calendar, bike, rain) + Intersection
  Observer scroll reveals
- Forecast-condition cards, timeline, orbital ecosystem diagram
- AI-free FAQ chatbot (keyword matching against a hardcoded array in the inline JS)
- Contact form via Formspree, FAQ widget, privacy and terms pages
- SEO: meta tags, Open Graph, JSON-LD (`SoftwareApplication` + `Organization`)
- Site language picker (translates the site chrome only — it does not mirror the
  app's 20 in-app languages)

## Development Notes
- `docs/index.html` is ~3050 lines with `<style>` and `<script>` inline. There is
  no separate CSS or JS file.
- UTF-8, no BOM, LF line endings. Preserve all three when rewriting the file —
  PowerShell edits must use `[System.IO.File]::WriteAllText` with
  `New-Object System.Text.UTF8Encoding($false)`.
- `.chat-demo .bubble:nth-child(n)` counts `.ai-pulse` and `.chat-demo-head` as
  children 1 and 2, so bubble delays start at `nth-child(3)`.
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