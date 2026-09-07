# isops.ai — marketing site

Public marketing site for **isops.ai**, served at <https://isops.ai>.
Hand-built static HTML — no framework, no build step, no runtime dependencies.
Every visual is inline SVG/CSS, so the whole site ships in ~260 KB with zero
image or JS-library requests.

## Pages

| File                  | Purpose                                              |
| --------------------- | ---------------------------------------------------- |
| `index.html`          | Landing page — hero, product mock, frameworks, FAQ   |
| `docs.html`           | Product documentation — full capability reference    |
| `about.html`          | Why isops exists, how the team works                 |
| `contact.html`        | Contact form (posts to `app.isops.ai`, mailto fallback) |
| `security.html`       | Trust principles, AI boundaries, disclosure policy   |
| `privacy.html`        | Privacy answers for buyer review                     |
| `subprocessors.html`  | Service categories used to deliver isops.ai          |
| `404.html`            | Not-found page                                       |

### Supporting assets

| File                        | Purpose                                                   |
| --------------------------- | --------------------------------------------------------- |
| `og-image.png`              | 1200×630 social share card (referenced by every page)     |
| `og-image.html`             | **Source** for `og-image.png` — edit here, then re-export |
| `robots.txt` / `sitemap.xml`| Crawl directives + sitemap                                |
| `.well-known/security.txt`  | RFC 9116 vulnerability-disclosure pointer                 |
| `CNAME`                     | Custom domain (`isops.ai`) for Pages hosting              |

## Local development

```bash
python3 -m http.server 8090
# → http://localhost:8090
```

(The same config lives in `.claude/launch.json` for the preview tooling.)

## Regenerating the OG image

`og-image.png` is rendered from `og-image.html`. After editing the source,
re-export with headless Chrome:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --hide-scrollbars --force-device-scale-factor=1 \
  --window-size=1200,630 \
  --screenshot="$PWD/og-image.png" \
  --virtual-time-budget=2000 "file://$PWD/og-image.html"
```

Keep it at 1200×630 (the 1.91:1 ratio LinkedIn / X / Slack / iMessage expect).

## Deploy

Static hosting (GitHub Pages / equivalent) serves the repo root. The `CNAME`
file binds the custom domain. Pushing to the default branch publishes.

## ⚠️ Security boundary

Anything committed here is **served publicly**. The local isops app writes
runtime data — SQLite DB, scan reports, audit logs, tokens tied to real AWS
accounts — into `.isops/` and `reports/`. These are gitignored and **must never
be committed**. Treat `.gitignore` as a "do not ship to production" list, not a
convenience.
