# Momentum — Marketing Consultancy Website

A one-page marketing consultancy website built with plain HTML, CSS, and JavaScript — no frameworks, no build step.

**Live site:** https://kenitas13.github.io/momentum-marketing-site/

![Screenshot of the Momentum landing page](assets/screenshot.png)

## Structure

Everything lives in a single file, [index.html](index.html), styled with a pastel palette (lavender/mint/peach on an off-white base, deep-indigo accent) after the layout patterns of freedomtech.tech:

- **Hero** — headline, subheadline, primary CTA to the enquiry form, secondary CTA to the Growth Score quiz.
- **Services marquee** — auto-scrolling band of what Momentum does.
- **Approach** — a numbered 4-step "how we work" card grid.
- **Growth Score quiz** — a 5-question interactive self-assessment (the site's lead magnet) that gives an instant score and hands off a summary into the enquiry form.
- **Testimonials** — an auto-scrolling marquee of client quotes.
- **FAQ** — an accessible accordion, mirrored in `FAQPage` structured data.
- **Enquiry form** — Name / Email / Company (optional) / Message, submitted via `fetch()` to [FormSubmit](https://formsubmit.co/).

Also includes `robots.txt`, `sitemap.xml`, and Open Graph/Twitter/JSON-LD metadata for SEO.

## Running locally

No build tools or dependencies required — just open the file directly:

```powershell
Start-Process "index.html"
```

## Configuration

The enquiry form posts to a [FormSubmit](https://formsubmit.co/) endpoint defined in the `<script>` block near the bottom of `index.html`:

```js
var FORMSUBMIT_ENDPOINT = 'https://formsubmit.co/ajax/kwswee@gmail.com';
```

No account or form ID needed — FormSubmit routes submissions straight to the email address in the URL. The first submission to a new address triggers a one-time confirmation email that must be clicked before real submissions start delivering.

## Deployment

The site is deployed via GitHub Pages from the `main` branch. Pushing to `main` updates the live site automatically.
