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
│   ├── index.html      # Main landing page (redesigned)
│   ├── pitch.html      # Investor pitch deck page
│   ├── pitch-deck.pdf  # Downloadable pitch deck PDF
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
- Modern responsive design with mobile support
- Sticky navigation with glassmorphism scroll effect
- Scroll-reveal animations and smooth transitions
- Floating phone mockup with info cards
- Social proof statistics bar
- How It Works timeline section
- Testimonials section with star ratings
- Multi-language support (auto browser detection)
- SEO optimized with enhanced meta tags, Open Graph, JSON-LD with ratings
- Contact form via Formspree with dual-column layout
- FAQ chatbot widget
- Privacy policy and terms pages

## Development Notes
- CSS uses custom properties (variables) for theming
- Primary color: #2d5a5a
- Accent color: #f97316
- Font: Inter (Google Fonts)
- No build step required - edit HTML/CSS directly
- Changes auto-deploy via GitHub Pages on push to main
- Intersection Observer API for scroll animations
- CSS keyframes for floating and pulse effects

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
- Dual-column layout with contact info sidebar

## Recent Changes
- Complete landing page redesign with modern UI/UX
- Added sticky navbar with glassmorphism scroll effect
- Enhanced hero section with gradient background and floating cards
- Added social proof statistics bar
- Added How It Works timeline section
- Added testimonials section with star ratings
- Added CTA section with gradient background
- Improved footer with multi-column layout and social icons
- Added scroll-reveal animations using Intersection Observer
- Enhanced SEO with rich JSON-LD structured data and ratings
- Added mobile responsive hamburger menu
- Improved contact form with dual-column layout
