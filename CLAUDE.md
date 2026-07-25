# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing consultancy website ("Momentum"), built as one static file: [index.html](index.html). No frameworks, no build tools, no package manager, no test suite — plain HTML/CSS/JS only. Pastel-themed (lavender/mint/peach on an off-white base, deep-indigo accent), styled after the layout patterns of freedomtech.tech (marquee bands, card grids, testimonial marquee) without reusing its palette, photography, or copy.

## Running it

There is no dev server or build step. Open `index.html` directly in a browser to preview changes:

```powershell
Start-Process "index.html"
```

To preview with a local server (needed for things like testing with Playwright, since some browser behaviors differ under `file://`):

```powershell
npx http-server -p 8080 -c-1
```

## Architecture

Everything lives in [index.html](index.html) in three inline blocks, in this order:

1. **`<style>` block** — CSS custom properties on `:root` drive the whole visual system: `--paper`/`--ink`/`--ink-muted` (base + text), `--lavender`/`--mint`/`--peach` (pastel fills), `--accent`/`--accent-deep` (decorative vs. the one bold full-bleed/CTA fill — see the contrast note below), spacing via `--section-padding`, radii, shadows. Breakpoints at `560px`/`700px`/`900px`/`1024px` for the form, approach grid, two-column sections, and nav respectively. Two font stacks: `--font-heading` ("Geist", falling back to "Sora", both loaded via `<link>` in `<head>`) and `--font-body` (system sans-serif stack).
2. **Body markup**, in document order: sticky `<nav>`, `#home` (hero), a decorative abstract graphic band, `#capabilities` (auto-scrolling services marquee), a problem/pain-point section, `#approach` (4-step numbered card grid), a feature list + graphic section, `#growth-score` (the Growth Score quiz — see below), a stats band, `#testimonials` (auto-scrolling testimonial marquee), `#faq` (accordion), `#contact` (enquiry form), `<footer>` (CTA + repeated nav). Nav/footer links are plain `href="#id"` anchors; each section has `scroll-margin-top` set so the sticky nav doesn't overlap the scroll target.
3. **`<script>` block** at the end of `<body>` — four independent pieces of behavior:
   - Smooth-scroll handler: intercepts clicks on any `a[href^="#"]` and calls `scrollIntoView({behavior: 'smooth'})`.
   - FAQ accordion: toggles `aria-expanded` and animates `max-height` per `.faq-question` button.
   - Growth Score quiz: a 5-question stepper (`.quiz-step[data-step]`) scored client-side (see below), ending in a results panel whose "Get Your Full Growth Plan" button scrolls to `#contact` and writes a summary into `#message` — but only if that field is empty or still holds a previous auto-write (tracked via `data-autofilled`), so it never clobbers something the visitor typed.
   - Enquiry form handler on `#enquiry-form`: client-side validation (required fields + email regex) against the `fields` map, then `fetch()`-posts JSON to the `FORMSUBMIT_ENDPOINT` constant with an `Accept: application/json` header, toggling status text/state (`sending` / `success` / `error`) in `#form-status`. Unchanged by the redesign.

## Key things to know before editing

- **Contrast rule for the pastel palette:** pastel fills (`--lavender`/`--mint`/`--peach`) must always pair with `--ink` text — white text on a pastel fill measures ~1.2:1 (fails WCAG badly). Only `--accent-deep` (not the paler `--accent`) is dark enough for white text, verified at ~4.8:1. Check contrast with a real calculation (not by eye) before introducing a new solid-fill component.
- **Growth Score quiz** (`#growth-score`) is the site's lead magnet: 5 questions, each option worth 1–4 points, `score = Math.round((raw - 5) / 15 * 100)`, banded into Early Stage/Building/Scaling/Optimized. It's entirely client-side (no backend/analytics/persistence) — intentional, since the site has no infrastructure for gated/email-delivered lead magnets.
- **JSON-LD** (`ProfessionalService` + `FAQPage`) lives in `<head>`. The `FAQPage` answer text must stay in sync verbatim with the visible `.faq-answer` markup — they're checked to match exactly.
- `FORMSUBMIT_ENDPOINT` in the `<script>` block points at [FormSubmit](https://formsubmit.co/)'s AJAX endpoint (`https://formsubmit.co/ajax/{email}`) — no account or form ID needed, just the destination email baked into the URL. The first submission to a new destination email triggers a one-time confirmation email from FormSubmit that must be clicked before real submissions start delivering.
- `robots.txt` and `sitemap.xml` are separate static files at the project root (not inside `index.html`) — update `sitemap.xml`'s `<lastmod>` when making meaningful content changes.
