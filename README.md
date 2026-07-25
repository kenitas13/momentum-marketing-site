# Momentum — Marketing Consultancy Website

A one-page marketing consultancy website built with plain HTML, CSS, and JavaScript — no frameworks, no build step.

**Live site:** https://kenitas13.github.io/momentum-marketing-site/

## Structure

Everything lives in a single file, [index.html](index.html):

- **Hero** — full-height intro with headline, subheadline, and a CTA that smooth-scrolls to the enquiry form.
- **Testimonials** — a responsive 3-card grid of client quotes.
- **Enquiry form** — Name / Email / Company (optional) / Message, submitted via `fetch()` to [Formspree](https://formspree.io/).

## Running locally

No build tools or dependencies required — just open the file directly:

```powershell
Start-Process "index.html"
```

## Configuration

The enquiry form posts to a Formspree endpoint defined in the `<script>` block near the bottom of `index.html`:

```js
var FORMSPREE_ENDPOINT = 'https://formspree.io/f/{YOUR_FORM_ID}';
```

Replace `{YOUR_FORM_ID}` with your actual Formspree form ID for submissions to go through. Until then, submitting the form will correctly show an inline error state.

## Deployment

The site is deployed via GitHub Pages from the `main` branch. Pushing to `main` updates the live site automatically.
