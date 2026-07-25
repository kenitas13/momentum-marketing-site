# Product / Marketing Context — Momentum

## What this is

"Momentum" is a marketing consultancy's own single-page marketing site (`index.html`), live at https://kenitas13.github.io/momentum-marketing-site/. It's the agency marketing itself, not a client project — so SEO/lead-gen work here should optimize for prospects searching for a marketing consultancy/agency, and any lead magnet should demonstrate the agency's own expertise (it's the product).

## Business

- **What we do**: strategy, creative, and performance marketing systems for ambitious brands — positioned against generic "pitch deck" agencies. Tagline: "Marketing That Moves the Needle."
- **ICP**: growth-stage brands/companies (B2B and DTC) who want marketing tied to revenue outcomes, not vanity metrics — buyer titles seen in testimonials: VP of Growth, Founder, CMO.
- **Proof points on site**: 4.2x average ROAS lift, 60+ brands scaled, 9 years in market.
- **Social proof**: 3 testimonials (Sarah Chen — VP of Growth, Northwind Labs; Marcus Reyes — Founder, Fieldstone Goods; Aisha Thompson — CMO, Lumen & Co.), emphasizing speed, no-fluff execution, and treating client budget like their own.
- **Tone/voice**: direct, no-fluff, confident, allergic to "pitch decks and vanity metrics" language — avoid generic marketing-agency clichés when writing new copy for this site.

## Current site state (as of last audit — post redesign)

- One page, 13 sections top to bottom: hero (`#home`), abstract graphic band, services marquee (`#capabilities`), problem section, approach 4-step grid (`#approach`), feature list, Growth Score quiz (`#growth-score`), stats band, testimonials marquee (`#testimonials`), FAQ accordion (`#faq`), enquiry form (`#contact`), footer.
- `<title>`: "Momentum — Marketing That Moves the Needle"
- `<meta name="description">`: "Momentum helps growth-stage B2B and DTC brands grow faster with strategy, creative, and performance marketing — 60+ brands scaled, 9 years in market."
- `robots.txt` + `sitemap.xml` now present at the project root. Canonical link, theme-color, full Open Graph + Twitter Card tags (backed by `assets/og-image.png`) in place.
- `ProfessionalService` + `FAQPage` JSON-LD present in `<head>` — no address/phone (not confirmed real, intentionally omitted rather than invented), no `AggregateRating` (the 3 testimonials aren't backed by a verifiable rating platform).
- **Still no blog/content section** — most content-depth/authority SEO work stays out of scope for this landing page.
- **Lead magnet now exists**: the Growth Score quiz (`#growth-score`) — a 5-question client-side-scored self-assessment that hands its summary into the enquiry form. Chosen because the site has no backend/ESP for a gated, email-delivered lead magnet; this is the correct adaptation for that constraint, not a placeholder for a "real" one later.
- Lead delivery: form still posts to FormSubmit (`https://formsubmit.co/ajax/kwswee@gmail.com`), no CRM/email tool integration yet.
- Hosting: GitHub Pages, deployed via GitHub Actions on push to `main`.
- No analytics or Search Console wired up yet (not confirmed either way — ask before assuming access).

## Design system (for consistency in any new pages/assets)

- **Palette (pastel, deliberately changed from the prior dark-navy identity at the user's explicit request)**: off-white base `--paper #FBF8FF`, ink text `--ink #2A2438` / `--ink-muted #6B6478`, pastel fills `--lavender #E4DDF7` / `--mint #D8F3E6` / `--peach #FBE4D8`, decorative accent `--accent #8B7FE8`, the one bold solid-fill/CTA accent `--accent-deep #6E5FE0`.
- **Contrast rule**: pastel fills only ever pair with `--ink` text (white-on-pastel fails WCAG badly, ~1.2:1). Only `--accent-deep` supports white text (~4.8:1). Verify contrast with a real calculation before adding new solid-fill components, not by eye.
- Fonts: headings in "Geist" (Google Font, falls back to "Sora"), body in system sans-serif stack.
- Radii/shadows: 8–20px radii, soft ink-tinted shadows (not black).
- Layout language borrowed from freedomtech.tech (verified live, not copied): full-bleed auto-scrolling marquee bands, alternating solid/pastel card grids, a bold full-bleed stats band, testimonial marquee, FAQ accordion, giant footer CTA. Freedomtech's own palette, photography, and copy were explicitly NOT reused.
- Overall aesthetic: soft pastel fields kept confident via bold, tight-tracked, large-scale type — not twee/decorative. No stock photography or fabricated "office photos" — abstract CSS/SVG gradient graphics instead.

## Open questions to ask before deep work (don't assume)

- Search Console / analytics access?
- Target keywords/topics for SEO (still none defined)?
- Whether the Growth Score quiz should eventually get real infrastructure (email delivery, analytics, persistence) if the site ever gets a backend/ESP — currently intentionally client-side-only.
