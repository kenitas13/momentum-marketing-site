# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing consultancy website ("Momentum"), built as one static file: [index.html](index.html). No frameworks, no build tools, no package manager, no test suite — plain HTML/CSS/JS only.

## Running it

There is no dev server or build step. Open `index.html` directly in a browser to preview changes:

```powershell
Start-Process "index.html"
```

## Architecture

Everything lives in [index.html](index.html) in three inline blocks, in this order:

1. **`<style>` block** — CSS custom properties defined on `:root` (colors: `--navy-*` / `--blue-accent`, spacing via `--section-padding`, radii, shadows) drive the whole visual system. Mobile-first rules with `@media (min-width: 700px)` / `560px` breakpoints for the testimonial grid and form layout. Two font stacks: `--font-heading` (Google Font "Sora", loaded via `<link>` in `<head>`) and `--font-body` (system sans-serif stack).
2. **Body markup** — four sections in document order: sticky `<nav>`, `#home` (hero), `#testimonials` (3-card grid), `#contact` (enquiry form), `<footer>`. Nav links and the hero CTA are plain `href="#id"` anchors targeting these section IDs; each section has `scroll-margin-top` set so the sticky nav doesn't overlap the scroll target.
3. **`<script>` block** at the end of `<body>` — two independent pieces of behavior:
   - Smooth-scroll handler: intercepts clicks on any `a[href^="#"]` and calls `scrollIntoView({behavior: 'smooth'})`.
   - Enquiry form handler on `#enquiry-form`: client-side validation (required fields + email regex) against the `fields` map, then `fetch()`-posts JSON to the `FORMSUBMIT_ENDPOINT` constant with an `Accept: application/json` header, toggling status text/state (`sending` / `success` / `error`) in `#form-status`.

## Key thing to know before editing the form

`FORMSUBMIT_ENDPOINT` in the `<script>` block points at [FormSubmit](https://formsubmit.co/)'s AJAX endpoint (`https://formsubmit.co/ajax/{email}`) — no account or form ID needed, just the destination email baked into the URL. The first submission to a new destination email triggers a one-time confirmation email from FormSubmit that must be clicked before real submissions start delivering.
