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

Before the page goes live, it runs an automatic QA gate (`scripts/qa.mjs`) and an SEO and GEO checklist covering one address, robots.txt, the sitemap, llms.txt, Open Graph, JSON-LD and speed.

## Use

Ask Claude as you normally would:

> Build a landing page for my app. Here are the screenshots and pricing.

> Redesign this landing: https://example.com

## What the plugin runs, fetches and sends

The skills are mostly instructions and reference files. Two optional Node.js scripts sit in each skill's `scripts/` folder:

| Script | What it does | Network |
|---|---|---|
| `capture.mjs` | Opens the URLs **you give it** (competitor pages, or your own page) in a local headless browser. It saves screenshots and a JSON summary of each page's structure into a local folder (`research/` by default). | Loads only the URLs passed to it, plus whatever those pages load themselves. |
| `qa.mjs` | Opens **your** page (a URL or a local `file://` path) in a local headless browser and prints a report: broken or dev links, forms, headings, repeated copy, image problems, footer and mobile header. | Loads only the URL passed to it. |

- **Nothing is sent anywhere.** The scripts make no API calls and include no analytics or telemetry. They don't upload screenshots or reports; all output stays on your machine.
- **Dependency.** The scripts need [Playwright](https://playwright.dev), pinned in `scripts/package.json`. You install it once:

  ```bash
  npm install --prefix "${CLAUDE_SKILL_DIR}/scripts"
  ```

  If no browser is installed, the scripts fall back to the system Chrome or Edge, or you can download Playwright's Chromium:

  ```bash
  npx --prefix "${CLAUDE_SKILL_DIR}/scripts" playwright install chromium
  ```

- **No credentials.** The plugin reads no API keys or tokens and stores no personal data.
- **Optional tools in the go-live checklist.** `references/seo-geo.md` suggests you run Lighthouse (`npx lighthouse@13.6.0`) and the Tailwind CLI (`npx tailwindcss@3.4.16`) on your own page. Claude runs them only as part of that step, with your approval.

Web searches for competitors go through Claude's own search tools, not through these scripts.

## Files

```
.claude-plugin/plugin.json
skills/
├── landing-page-builder/        # English
└── landing-page-builder-ru/     # Русский
    ├── SKILL.md                 # workflow, structure, base rules
    ├── references/              # brief, competitor research, framework, reference standard,
    │                            # redesign (mode B), visual system, build + QA, SEO + GEO, examples
    ├── templates/teardown.md    # teardown file to fill
    └── scripts/                 # capture.mjs, qa.mjs, package.json
```

## License

MIT. See [LICENSE](LICENSE).
