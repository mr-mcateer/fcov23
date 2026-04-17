# FCoV-23 International Research Consortium — Website

Static-site rebuild of [fcov23.org](https://fcov23.org). Replaces the
compromised WordPress install (Balada Injector malware, April 2026) with
a zero-attack-surface static site.

**Preview:** https://promptaisolutions.com/fcov23/

---

## What's in this repo

```
site/               Everything that gets served to visitors
├── index.html      Home (hero + mission + position statements + funding)
├── research.html   6 research programs + spike protein + resource cores
├── resources.html  Genomic sequencing + position statements + literature + privacy
├── about.html      Sponsors + 22 experts + 4 external advisors
├── contact.html    Contact form (mailto-based, GDPR checkbox)
├── blog.html       Placeholder pointing to Cornell Fight FIP blog
├── 404.html        Branded not-found page
├── robots.txt      Allow all, points to sitemap
├── sitemap.xml     6 canonical URLs, hosted at fcov23.org
└── assets/
    ├── css/        tokens.css (design tokens) + main.css + components.css + fonts.css
    ├── js/         main.js (theme toggle, mobile nav, scroll reveal)
    ├── images/     All original photos + vectorized SVG logo
    ├── fonts/      Self-hosted Inter + Playfair Display (woff2, subsetted)
    └── pdfs/       Position Statement PDF

.github/workflows/deploy.yml    GitHub Actions → GitHub Pages
_orchestrator/                  Build tools (scraper, downloader) — not deployed
_raw/                           Original scraped WP HTML — gitignored (contains malware)
content/                        Extracted content JSON — gitignored
```

## Deployment options for the owner

### Option A — GitHub Pages with fcov23.org (recommended, free)

1. At DNS provider (GoDaddy), create these records for `fcov23.org`:
   ```
   A     @   185.199.108.153
   A     @   185.199.109.153
   A     @   185.199.110.153
   A     @   185.199.111.153
   AAAA  @   2606:50c0:8000::153
   AAAA  @   2606:50c0:8001::153
   AAAA  @   2606:50c0:8002::153
   AAAA  @   2606:50c0:8003::153
   CNAME www  mr-mcateer.github.io
   ```
2. In this repo, create a file `site/CNAME` containing exactly:
   ```
   fcov23.org
   ```
3. Push to `main`. GitHub Pages will:
   - Verify DNS
   - Provision Let's Encrypt TLS
   - Serve site at `https://fcov23.org` within ~10 min

### Option B — Host elsewhere (Netlify, Cloudflare Pages, S3+CloudFront, etc.)

Serve the contents of `site/` as a static site root. No build step required —
the files are the build. Any host that serves static files works; the site
has no server-side dependencies.

### Option C — Upload to GoDaddy shared hosting (not recommended)

Only if she wants to keep the existing GoDaddy plan. Upload the contents of
`site/` (not the folder itself — its contents) to the `public_html/` directory
via FTP/File Manager. Remove the WordPress files entirely (do NOT leave WP
alongside — the malware persists in the database and plugin files). This
option keeps a larger attack surface than static hosting elsewhere.

## Before flipping DNS away from the old WP site

1. **Take WordPress offline** or put it in maintenance mode
2. **Rotate all credentials**: WP admin, GoDaddy cPanel, FTP, database
3. **Scan** the WP install with Wordfence or Sucuri before keeping any backups
4. **Export any data** the WP site had (database, uploads) before decommissioning

## Rebuild trail

Every expert credential, institution line, publication title, funding figure,
and paragraph on every page was restored verbatim from the scraped original
HTML. Full parity check: 46/46 critical strings match, zero typos found.

Every image and PDF was downloaded from the original WP `/wp-content/uploads/`
URLs (binary files — PNG/JPEG/PDF headers verified, malware was only ever
in the HTML). SVG logo was generated locally via `potrace + ImageMagick`
color-separation of the 1080×1080 PNG — the paths are traced from pixel
data, nothing fetched from elsewhere.

## Credits

Original design: **Šárka Kuráková** ([skmarketing.cz](https://www.skmarketing.cz/))
Static rebuild: preserves the original visual identity, content, and workflow.
