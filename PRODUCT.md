# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

User-chosen on the decision page (2026-07-29): **single-file static HTML/CSS/JS** — one `index.html` holding all styles and interaction, zero dependencies, opens by double-click, easy to send to travel companions or host anywhere. No build step, no npm install.

## Users

A Chinese-speaking traveller planning a Hokkaido, Japan trip, sharing the plan with 1–3 travel companions (friends or family). They read it on a phone during the trip (on the subway between stations, in a taxi, in a ryokan lobby) and on a laptop while planning at home.

*Inferred from the brief, not confirmed:* party size, season, and exact length were not stated. The build assumes a 7-day/6-night winter-to-spring-friendly loop (Sapporo → Otaru → Furano/Biei → Daisetsuzan → Hakodate) that the owner can edit in place.

## Product Purpose

A 路书 (roadbook): one page that turns a Hokkaido trip into something a companion can read, follow, and get excited about. Day-by-day route, what to see, what to eat, where to stay, what to book, what to carry, and how to get from A to B. Success = a companion closes the page knowing exactly what the trip looks like and what they must do next (book, pack, decide).

## Positioning

Not a blog post and not a booking funnel: a *working* itinerary that is also a keepsake. Every fact on the page is editable text in one file, and the page carries the trip's own texture (transit legs, distances, station names, budget lines) rather than stock travel-brochure gloss.

## Operating Context

- Read mostly on a phone, often outdoors in cold light; must stay legible in bright snow-glare and dark-mode-averse winter gloves (large tap targets).
- Shared by sending the file or a link to companions; no account, no server, no network dependency beyond fonts.
- The owner edits the content by hand in the HTML, so the content must live in obvious, well-marked blocks.

## Capabilities and Constraints

- Single self-contained `index.html`; no framework, no bundler, no external JS libraries.
- Web fonts via Google Fonts CDN are acceptable; the page must degrade to a system-font fallback offline.
- No photography assets exist. Any imagery must be authored (CSS/SVG/emoji-grade illustration), not fetched, and must not pretend to be real travel photography.
- No real prices are confirmed. Budget figures are illustrative planning numbers and must read as editable estimates, not quoted prices.
- Language: Simplified Chinese, with Japanese place names shown alongside for on-the-ground use.

## Brand Commitments

- **可爱风 (cute style) is a binding brief constraint** from the user's request. It governs the whole surface and outranks any default restraint for a Read-mode page.

## Evidence on Hand

- None. No photos, no booking confirmations, no route files, no prior version of this page.
- Absences the work must not fabricate: real prices, real hotel names presented as booked, real availability, real restaurant recommendations claimed as visited, testimonials.

## Product Principles

1. The itinerary is the product — every design choice serves "can a companion act on this tomorrow?"
2. Cute is craft, not decoration: warmth comes from made things (drawn icons, paper texture, hand-set type), not from sprinkled emoji.
3. Facts stay editable and honest; atmosphere is authored and clearly authored.
4. Phone-first, snow-proof: legible, tappable, and readable in one thumb-scroll per day.
5. One file, zero dependencies — the page must survive being emailed as an attachment.

## Accessibility & Inclusion

WCAG 2.2 AA-minded: contrast-safe palette against the cute pastel world, full keyboard reach for the day navigation, visible focus, `prefers-reduced-motion` respected, and text that stays readable at 200% zoom.
