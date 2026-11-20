# Browser checks: capture and the QA gate

This plugin ships no scripts. Capture and QA run through the browser tools Claude has in the
session: a built-in browser, Claude in Chrome, or a Playwright/Puppeteer MCP. With no browser tool,
use web fetch for text and ask the owner for screenshots.

The standalone version of this skill, with ready-made Node scripts for the same two jobs, is
at https://github.com/natalietd/landing-page-builder.

## 1. Capture a page (competitors, references, our own page)

For each URL:

1. **Open it at 1440×900.** Close cookie and promo overlays without accepting anything (privacy
   rules: decline or close). Never weaken TLS or security settings to get a page to load.
2. **Turn off smooth scrolling before you scroll:** run
   `document.documentElement.style.scrollBehavior = 'auto'` in the page. Otherwise
   `scroll-behavior: smooth` makes screenshots land mid-scroll and come out shifted.
3. **Scroll the whole page once** in steps of about 80% of the viewport, so lazy images and
   scroll-reveal blocks load. Then go back to the top.
4. **Screenshot it section by section:** scroll each `section` (or each top-level block) into view,
   wait about a second, take a screenshot. Hide sticky headers first if they cover the block. Save as
   `research/<site>/sections/NN.png` when the tool can save files. Otherwise keep them in the
   conversation and describe each one in the notes.
5. **Write down the structure:**
   - the outline of H1–H3 headings;
   - each CTA's text and where it goes (`href`);
   - prices and plans;
   - proof signals (ratings, logos, numbers);
   - the fonts (computed `font-family` of h1 and body), the main colors (body background, text,
     primary CTA) and the media format (screens, video, illustrations).

   Reading the accessibility tree or page text covers most of it.
6. **Repeat at 390×844** for the mobile view: the first screen and anything that breaks.

Look at the screenshots, not only the text: the structure tells you the order, the images tell you
the quality. Some sites fail to load (timeouts, geo-blocking, certificates the browser doesn't
trust). Then use web fetch for the text, or swap in the next competitor, and record the failure in
the teardown.

## 2. The QA gate for our page

Run this before any check by eye. Open our page at 1440×900, then at 390×844. Use the browser tool
(read the page, run small read-only expressions in it, take screenshots) to answer each line.
**Every FAIL gets fixed before the owner sees the page.** Read every WARN and fix or justify it.

### FAIL

| Check | How to test it |
|---|---|
| **Dev or placeholder destinations** | No `href` or form `action` contains `localhost`, `127.0.0.1`, `0.0.0.0`, `.local`, `TODO`, `REPLACE_ME` or `example.com`. |
| **Links with no destination** | No visible `<a>` without an `href`, or with `href="#"`. |
| **Custom forms** | No `<form>`, unless it is a product input that hands off to `PRODUCT_URL` and carries `data-product-input`. |
| **Exactly one H1** | `document.querySelectorAll('h1').length === 1`. |
| **Label headings** | No visible H2 of 1–2 words ("Examples", "Templates", "Pricing"). Move the label to the eyebrow and write a headline that sells. |
| **Repeated copy** | No phrase of 6+ words appears in 3+ visible text blocks outside the FAQ. List the visible text and look for repeats. |
| **Dark slab on a light page** | On a light page, no full-width block (≥90% of the viewport wide, ≥200px tall) has a near-black background. |
| **Heavily shrunk images** | No image ≥1000px wide shown at ≥450px with `naturalWidth / renderedWidth > 3`. |
| **Images cut by their container** | No image ≥120px wide is 5–85% visible inside a parent with `overflow: hidden`, `clip`, `auto` or `scroll`. Carousels included. |
| **Footer** | A `<footer>` exists and has legal links (privacy, terms; for a paid service in Russia also публичная оферта, политика обработки ПДн and the seller's details). |
| **Mobile overflow** | At 390px, `document.documentElement.scrollWidth <= innerWidth`. |
| **Mobile header** | At 390px, the header is ≤72px tall and on one row. |
| **Console errors** | None. |

### WARN

| Check | How to test it |
|---|---|
| **CTA that only scrolls to pricing** | A product-sounding button ("Start", "Create") whose `href` is `#pricing` or `#offer`. The label must say where it goes, or link to `PRODUCT_URL`. |
| **Small desktop UI** | A wide image (aspect ≥1.3) of an app window shown under 700px. Zoom to 100% and look: if the UI text is under ~11px, crop to the panel that matters or show it bigger. Output galleries can opt out with `data-gallery`. |
| **Reused image** | The same image (≥120px) appears in two blocks. |
| **Long headlines** | Any H1/H2 over 3 lines at 1440px (height ÷ line-height). |
| **Long eyebrows** | Uppercase small labels over ~50 characters. |
| **Overlapping images** | Two images ≥160px overlap by 15–95%. That's fine for a deliberate composition, a bug when one covers the other's content. |
| **Missing `alt`** | Any `<img>` without an `alt` attribute. |
| **Default FAQ markers** | A `<summary>` still shows the browser's ▶ marker (`display: list-item`). |
| **Fallback fonts** | Visible text (inputs and buttons included) renders in Arial, Times or a system font while the page declares web fonts. Set `font: inherit`. |
| **Footer contact** | No email, Telegram or phone in the footer. |
| **Small tap targets** | Links and buttons under 36px tall at 390px. |
| **SEO gaps** | No meta description, canonical or `og:image`; the Tailwind Play CDN on a live URL; a platform or version URL (`*.vercel.app`, `*.railway.app`, `/v3/`). See `seo-geo.md`. |

Record the result in the handoff as "QA gate: N FAIL (fixed), M WARN (fixed or justified)".
