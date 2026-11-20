# Outlanding

Outlanding builds landing pages that beat your competitors'. It tears down their real landing pages, takes what works, and builds yours to a higher bar, in **any niche**, with one formula:

> **our structure × competitors' best practices, built to the reference standard**

It ships two skills with the same method, one per language:

| Skill | Language |
|---|---|
| `landing-page-builder` | English |
| `landing-page-builder-ru` | Русский |

## What it does

- **Structure.** Every page covers five jobs: first screen, pains (only if competitors use a pains block), benefits, product, and offer & CTA. There are two CTA types, into the product or book a call. It never builds lead or quiz forms.
- **Competitors.** It finds 3–5 real competitor landing pages, screenshots them section by section and tears them down.
  - What the whole market does becomes the baseline.
  - What the leaders do well gets adopted, in your own words.
  - What nobody does becomes your positioning.
- **Reference standard.** Four hand-picked landing pages set the quality bar for block patterns, copy and visual craft.
- **Design.** Every page gets its own design, built from the niche and its competitors. There is no shared template.

There are two modes:

1. **Build from scratch.** Brief → competitor capture → teardown → page plan (you approve it) → build → QA on desktop and mobile.
2. **Improve the design** of an existing landing. Capture the current page → audit → plan a new look that keeps your facts, copy and logo → you approve it → edit → compare before and after.

Before the page goes live, it runs a QA gate in the browser and an SEO and GEO checklist covering one address, robots.txt, the sitemap, llms.txt, Open Graph, JSON-LD and speed.

## Use

Ask Claude as you normally would:

> Build a landing page for my app. Here are the screenshots and pricing.

> Redesign this landing: https://example.com

## What the plugin runs, fetches and sends

The plugin is instructions and reference files only. It contains **no scripts, no executables and
no dependencies**.

- **Browsing.** To study competitors and to check your page, Claude uses the browser tools already
  in your session (a built-in browser, Claude in Chrome, or a browser MCP). It opens only the
  competitor pages it finds through its own web search, the URLs you give it, and your own page.
  It takes screenshots and reads page structure. It never accepts cookie banners and never enters
  credentials.
- **Nothing is sent anywhere.** The plugin makes no API calls of its own and has no telemetry.
  Screenshots and notes stay in your conversation or in local files you choose (`research/` by
  default).
- **No credentials.** It reads no API keys or tokens and stores no personal data.
- **Optional tools you run yourself.** The go-live checklist (`references/seo-geo.md`) links to
  PageSpeed Insights, Lighthouse and the Tailwind CLI documentation. Nothing is installed by the
  plugin.

The standalone version of the skill, with optional Node scripts for capture and QA, lives at
https://github.com/natalietd/landing-page-builder.

## Files

```
.claude-plugin/plugin.json
skills/
├── landing-page-builder/        # English
└── landing-page-builder-ru/     # Русский
    ├── SKILL.md                 # workflow, structure, base rules
    ├── references/              # brief, competitor research, framework, reference standard,
    │                            # redesign (mode B), visual system, build, browser checks + QA gate,
    │                            # SEO + GEO, examples
    └── templates/teardown.md    # teardown file to fill
```

## License

MIT. See [LICENSE](LICENSE).
