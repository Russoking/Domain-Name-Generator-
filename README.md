# Two Word Domain Name Generator

A single-purpose, static utility site. Instantly combines a seed word with curated
word banks to suggest brandable two-word domain names, with copy and live
registrar-check actions. No backend, no build step, no dependencies beyond
Google Fonts (loaded client-side via CDN).

## File structure

```
/
├── index.html        English version (root, x-default)
├── es/
│   └── index.html    Spanish version
├── robots.txt
└── sitemap.xml
```

## Before you deploy

Every file with `YOUR-DOMAIN.com` needs your real domain substituted in:

- `index.html` — `<link rel="canonical">`
- `es/index.html` — `<link rel="canonical">`
- `robots.txt` — `Sitemap:` line
- `sitemap.xml` — all four `<loc>` / `<xhtml:link>` URLs

Quick way to do this from the repo root once you know your domain:

```bash
find . -type f \( -name "*.html" -o -name "*.xml" -o -name "*.txt" \) \
  -exec sed -i '' 's/YOUR-DOMAIN.com/example.com/g' {} +
```

(Drop the empty `''` after `-i` if you're on Linux instead of macOS.)

## Deploying via GitHub + Cloudflare Pages

1. **Push to GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit: two word domain name generator"
   git branch -M main
   git remote add origin https://github.com/Russoking/Domain-Name-Generator-.git
   git push -u origin main
   ```

2. **Connect Cloudflare Pages**
   - Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git
   - Select this repository
   - Build settings: this is a static site, so leave **Build command** empty
     and set **Build output directory** to `/` (the repo root)
   - Deploy

3. **Custom domain**
   - In the Pages project → Custom domains → add your domain
   - Update DNS if the domain isn't already on Cloudflare (Pages will prompt you)

4. **After the domain is live**
   - Submit `sitemap.xml` in Google Search Console (Search Console → Sitemaps)
   - Verify both the English and Spanish URLs are indexed and that Search
     Console's International Targeting report picks up the hreflang pairs
     correctly

## Adding another language later

Duplicate the `es/` pattern: create `/de/index.html` (or whichever language),
add its own localized word banks, FAQ, and JSON-LD, then add a third
`hreflang="de"` line to *all three* HTML files (including the new one
pointing to itself) and a matching entry in `sitemap.xml`.
