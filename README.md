# Performance Checker

How fast is a page, and what should you fix first? Enter a URL, pick mobile or desktop, and get a Lighthouse report in plain language.

Live: https://granqvistsanna-web.github.io/perf-checker/ (not deployed yet) · Part of the [Toolbox](https://granqvistsanna-web.github.io/).

## What it shows

| Section | What's in it |
| --- | --- |
| Scores | Performance, Accessibility, Best practices and SEO as rings, 0–49 poor, 50–89 needs work, 90–100 good |
| Core Web Vitals, lab | LCP, TBT (lab stand-in for INP), CLS, FCP and Speed Index from this run, placed on the good / needs work / poor scale |
| Real users | Chrome UX Report data for the last 28 days: LCP, INP, CLS, FCP, TTFB and the share of visits in each band. Falls back to the whole site when the page has too little traffic |
| Fix these first | The top six performance findings, sorted by estimated time saved, each with a plain-language fix |

`?url=example.com&strategy=desktop` re-runs the check, so the address bar is the share link.

## How it's built

- `index.html` is the whole app. Static, on GitHub Pages, styled with [Pumice](https://granqvistsanna-web.github.io/pumice/). No backend.
- It calls Google's [PageSpeed Insights API v5](https://developers.google.com/speed/docs/insights/v5/get-started) straight from the browser, which runs real Lighthouse on Google's servers (10–30 s per check).
- **API key:** calls without a key share one anonymous Google quota with everyone on the internet, and in practice it's often used up (HTTP 429). When that happens the page asks for the visitor's own free key, which is kept in `localStorage` only and sent to Google with each check. Another option is to create one key restricted to the `granqvistsanna-web.github.io` referrer and set it as a default in `index.html`.

## Develop

```bash
python3 -m http.server 5179
```

Then open http://localhost:5179. To test the rendering without spending quota, save a PageSpeed response (or a local `npx lighthouse <url> --output=json` report wrapped as `{ "lighthouseResult": … }`) in `fixtures/` (git-ignored) and point `fetch` at it from the console.
