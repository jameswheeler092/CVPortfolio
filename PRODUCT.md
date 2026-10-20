# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack
Static HTML, one shared stylesheet (`assets/site.css`), vanilla JS inline per page. No build step or frameworks. Hosted on GitHub Pages from the repo root, so every path is relative.

## Users
Recruiters and hiring managers at AI product companies, on desktop and phone. They read the site alongside a CV and decide in about 30 seconds whether to talk to James.

## Product Purpose
James Wheeler's personal portfolio: three pages (home, `atlas/`, `guardian/`) that show him as a product manager who builds with AI. Success is a recruiter reading the home page quickly, opening a case study, and emailing him.

## Positioning
A product manager who shipped two real products, Guardian Compliance (950 live sites, 35% of the market) and RBW Atlas (an AI-native internal operating system built with coding agents), and who shows where a person decides in each.

## Operating Context
James is Product Manager (Compliance & AI) at RBW Systems, London. The site supports his CV. Facts on the pages come from his work at RBW; anything not yet known is a dashed "add: …" draft marker rather than an invented claim.

## Capabilities and Constraints
- Home page with typed greeting, About, Projects, How I work, Experience and Contact.
- Two case studies with diagrams, a before/after morph and a threshold simulator (demos are placeholders).
- Links (LinkedIn, email, CV) are set in one `window.SITE` block; the CV link is empty, so CV buttons show a disabled state.
- Draft markers and their toggle stay until the content is filled in.
- Open: `og.png`, the `YOUR-DOMAIN` placeholders and the final Pages URL.

## Brand Commitments
Plain wordmark "James Wheeler". Voice: first person for what James did or decided ("I"), "we" for the company. Locked visual direction from the 20 Oct 2026 brief: matte "ink on paper", Besley and Source Sans 3, amber only as a fill meaning a person decides, no avatar, the scroll-driven thread as the one signature graphic. Recorded here as a binding constraint, not expanded.

## Evidence on Hand
Real numbers and claims already on the pages: 950 live sites as of 19 October 2026, 35% of the sites in the market, 1,200 locations on the telemetry feed, 80% benchmark and about 70% live for the support agent, a £2.5m 10-year contract. Absent: a published CV, adoption numbers for Atlas, a third project, and real click-through demos. Future work must not fabricate any of these.

## Product Principles
- Read in 30 seconds, then reward a second look.
- Every claim comes from the page or becomes a visible "add:" marker.
- Show where a person decides; amber marks it and nothing else.
- Authored, not templated: one idea (the thread), used well.
- Fast and static: no frameworks, no dependencies beyond Google Fonts.

## Accessibility & Inclusion
Visible focus ring, skip link, `prefers-reduced-motion` support (no typing, thread fully drawn), body text at 4.5:1 or better, content fully readable with JavaScript off.
