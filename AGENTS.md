# MyHobbyPlan Website - Project Context

## Overview
- **Project**: MyHobbyPlan - Smart hobby planning and activity scheduling app
- **Type**: Static landing page website (HTML/CSS/JS)
- **Hosted**: GitHub Pages
- **Owner**: Eleviq Technologies (https://kigechaedward.github.io/eleviq-website/#/)
- **Landing Page URL**: https://kigechaedward.github.io/Myhobbyplan-website/

## Purpose
- This website is a **landing page** for the MyHobbyPlan mobile app
- It showcases app features, provides download links, and includes legal pages (privacy, terms)
- The app itself is developed by Eleviq Technologies

## Tech Stack
- Pure HTML, CSS, JavaScript (no build tools/frameworks)
- Google Fonts (Inter)
- Formspree for contact form
- GitHub Actions for deployment

## File Structure
```
├── docs/
│   ├── index.html      # Main landing page (703 lines)
│   ├── pitch.html      # Investor pitch deck page
│   ├── privacy.html    # Privacy policy page
│   ├── terms.html      # Terms and conditions page
│   ├── logo.png        # App logo
│   ├── screenshot.png  # App screenshot
│   ├── app-ads.txt     # Ad configuration
│   └── *.png           # Feature images
├── .github/workflows/
│   └── deploy-pages.yml  # GitHub Pages deployment
└── app-ads.txt         # Root ad configuration
```

## Key Features
- Responsive design with mobile support
- Multi-language support (auto browser detection)
- SEO optimized with meta tags, Open Graph, JSON-LD
- Contact form via Formspree
- Privacy policy and terms pages

## Development Notes
- CSS uses custom properties (variables) for theming
- Primary color: #2d5a5a
- Accent color: #f97316
- Font: Inter (Google Fonts)
- No build step required - edit HTML/CSS directly
- Changes auto-deploy via GitHub Pages on push to main

## Common Tasks
- Edit landing page: `docs/index.html`
- Edit pitch deck: `docs/pitch.html`
- Edit privacy policy: `docs/privacy.html`
- Edit terms: `docs/terms.html`
- Update logo/screenshots: Replace files in `docs/`
- Add new pages: Create in `docs/` directory

## Contact Form
- Formspree endpoint configured for contact form submissions
- Located in `docs/index.html`

## Recent Changes
- Standardized contact information across site
- Updated contact form with active Formspree endpoint
- Updated Privacy Policy for Google Play Developer standards
- Implemented site-wide automatic browser language detection
- Added metadata, social tags, and JSON-LD structured data
- Added feature screenshots, hover animations, and multi-language support
- Added investor pitch deck page (pitch.html)
