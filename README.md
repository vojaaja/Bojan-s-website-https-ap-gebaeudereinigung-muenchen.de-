# AP Gebäudereinigung — Website

Bilingual (DE/EN) marketing website for AP Gebäudereinigung, a commercial
cleaning company based in Munich, Germany.

**Live site:** https://ap-gebaeudereinigung-muenchen.de/

## Tech stack

- **[Eleventy (11ty)](https://www.11ty.dev/)** — static site generator
- **Nunjucks** — templating language for pages and shared partials
- **Vanilla CSS** — no framework, single shared stylesheet
- **Vanilla JS** — small amount, for the mobile nav toggle and the AJAX
  contact form submission
- **[Formspree](https://formspree.io/)** — handles contact form submissions
  (no backend of our own)
- Hosted on **[Netlify](https://www.netlify.com/)**, domain via **Strato**

## Project structure

```
src/
  _includes/
    base.njk              # master HTML layout (head, hreflang, JSON-LD)
    partials/
      header.njk           # nav bar, language switcher (shared, i18n-aware)
      footer.njk            # footer (shared, i18n-aware)
  _data/
    site.json              # company info: name, address, phone, email, etc.
    i18n.json               # UI strings (nav labels, footer labels) per language
  assets/
    styles.css              # single shared stylesheet for the whole site
    logo.png
    main-desktop.jpg / main-mobile.jpg   # hero photos (responsive via <picture>)
    badge-desktop.svg / badge-mobile.svg # "AI-generated image" badge overlay
  index.njk, about.njk, contact.njk,     # German pages (primary, served at root)
  impressum.njk, privacy.njk
  en/
    index.njk, about.njk, contact.njk,   # English pages (served under /en/)
    impressum.njk, privacy.njk
  sitemap.njk               # generates /sitemap.xml
  robots.njk                 # generates /robots.txt
.eleventy.js                 # Eleventy config (input/output dirs, passthrough copy)
```

## Local development

Requires [Node.js](https://nodejs.org/).

```bash
npm install
npx @11ty/eleventy --serve
```

This builds the site into `_site/` and serves it locally with live reload.

To build once without serving (e.g. before deploying):

```bash
npx @11ty/eleventy
```

## Deploying

The contents of `_site/` are what gets uploaded to Netlify (drag-and-drop
the folder, or connect this repo to Netlify for automatic deploys on push).

## Bilingual structure

German is the primary language and lives at the site root
(`/index.html`, `/about.html`, etc.). English is a full mirror under
`/en/` (`/en/index.html`, etc.). Every page pair is connected via:

- `hreflang` alternate tags in `<head>` (see `base.njk`) so Google knows
  the two versions are translations of each other, not duplicate content
- The `translationUrl` front-matter field on every page, used to build
  the working EN/DE language-switcher links in the header

Adding a new page means creating both a German file at the root and an
English file under `en/`, each with matching `permalink`, `lang`,
`selfPath`, and `translationUrl` front matter.

## Editing content

- **Text content** (headings, paragraphs, service descriptions) lives
  directly in each page's `.njk` file.
- **Shared UI strings** (nav links, footer labels, "Language" label,
  etc.) live in `src/_data/i18n.json`, keyed by `en`/`de`.
- **Company info** (address, phone, email) lives in `src/_data/site.json`
  and is used by the footer — update it there once rather than per page.

## Contact form

The form on `/contact.html` (and `/en/contact.html`) submits via
`fetch()` to a Formspree endpoint and shows a success/error message
inline instead of redirecting away from the site. The Formspree
endpoint ID is hardcoded in `contact.njk` — update it there if the
form is ever recreated on Formspree.

## Legal pages

`impressum.njk` and `privacy.njk` (and their English equivalents)
contain the legally required German Impressum and a GDPR-compliant
privacy policy, including disclosures for the two third-party services
this site actually uses: Formspree (form processing) and Google Fonts.
Update these if the services used by the site change.
