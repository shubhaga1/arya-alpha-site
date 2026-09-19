# Changelog

All notable changes to the Arya Alpha Capital site, most recent first. Each entry shows what changed and why, with before/after snippets.

## 2026-09-19 — Added CTA to "Invest With Clarity" section

**Why:** Consistency — this was the only content section left without a CTA link.

Before:
```html
<div class="hero-products" style="color: var(--navy);">MUTUAL FUNDS &nbsp;|&nbsp; PMS &nbsp;|&nbsp; LONG-ONLY AIFs</div>
  </div>
</section>
```
After:
```html
<div class="hero-products" style="color: var(--navy);">MUTUAL FUNDS &nbsp;|&nbsp; PMS &nbsp;|&nbsp; LONG-ONLY AIFs</div>
    <a href="#contact" class="section-cta">Start a Portfolio Conversation →</a>
  </div>
</section>
```

## 2026-09-19 — Added CTA links to all remaining content sections

**Why:** User asked to review every link and ensure every section has a call to action. Only the Hero and Contact sections had explicit CTAs; the other 8 sections (Opportunity, Philosophy, Process, Universe, Research Framework, Who We Serve, Difference, Founder) had none.

Added a new `.section-cta` style:
```css
.section-cta {
  display: inline-block;
  margin-top: 32px;
  color: var(--gold-deep);
  font-weight: 600;
  font-size: 14.5px;
  text-decoration: none;
  border-bottom: 1px solid var(--gold-deep);
  padding-bottom: 2px;
}
.section-cta:hover { color: var(--navy); border-color: var(--navy); }
```

And appended `<a href="#contact" class="section-cta">Start a Portfolio Conversation →</a>` to the end of: `#landscape`, `#philosophy`, `#process`, `#universe`, `#framework`, `#who-we-serve`, `#difference`, `#founder`.

## 2026-09-19 — Fixed domain mismatch in SEO/social meta tags

**Why:** Canonical URL, Open Graph tags, Twitter card tags, and JSON-LD schema all referenced `aryaalphacapital.com`, but the site is actually deployed at `aryaalpha.com` and `arya.prafullsaxena.cloud`. Wrong domain in these tags would show incorrect URLs in Google search results and social link previews.

Before:
```html
<link rel="canonical" href="https://aryaalphacapital.com/">
<meta property="og:url" content="https://aryaalphacapital.com/">
<meta property="og:image" content="https://aryaalphacapital.com/assets/og-image.png">
<meta name="twitter:image" content="https://aryaalphacapital.com/assets/og-image.png">
"url": "https://aryaalphacapital.com/",
"logo": "https://aryaalphacapital.com/assets/logo-icon.png",
"image": "https://aryaalphacapital.com/assets/og-image.png",
```
After: every occurrence of `aryaalphacapital.com` → `aryaalpha.com`.

## 2026-09-19 — Removed founder credential boxes, replaced bio text

**Why:** Paridhi requested via WhatsApp: remove the sub-heading line and the 4 credential boxes under her name, replace the bio paragraphs with her new text, no sub-heading.

Before:
```html
<h2 class="section-title" style="margin: 0;">CA Paridhi Agrawal</h2>
<div class="role">Founder · CFA Level II · NISM Certified</div>
<div class="founder-credentials">
  <span>CA</span>
  <span>CFA</span>
  <span>NISM</span>
  <span>10+ yrs</span>
</div>
...
<p>Brings over a decade of experience across financial analysis, professional services, business finance and investment research.</p>
<p>Her approach combines financial discipline, investment analysis, business understanding and long-term portfolio thinking. Arya Alpha is built as a focused platform where the quality of guidance matters more than the volume of transactions.</p>
```
After:
```html
<h2 class="section-title" style="margin: 0;">CA Paridhi Agrawal</h2>
...
<p>Paridhi Agrawal, Founder of Arya Alpha Capital, is a Chartered Accountant and CFA Level II professional who brings an institutional pedigree to wealth distribution. She possesses 10+ years of experience across equity research and financial analysis as an alumna of Anand Rathi, KPMG, and Deloitte.</p>
<p>Alongside launching Arya Alpha Capital, she continues as a Partner at BOE Consulting, driving finance, investment research, and corporate fund investment decisions. Holding specialized NISM certifications in Mutual Fund (V-A) and PMS Distribution (XXIII-A), she leverages this active market background to bring immense rigor to the selection and distribution of PMS, AIF, and Mutual Fund products.</p>
```
Also removed the now-unused `.role` and `.founder-credentials` CSS rules (including the mobile media-query override for `.founder-header .role`).

## 2026-09-19 — Added `docker-compose.yml` for local/self-hosted runs

**Why:** Needed a way to build and run the site container locally (and potentially on a self-hosted Docker host) without typing out `docker build`/`docker run` manually every time.

Added `site/docker-compose.yml`:
```yaml
services:
  site:
    build: .
    ports:
      - "8080:80"
    restart: unless-stopped
```
Also added a "Docker Compose" section to `README.md`'s run-locally instructions.

## 2026-09-19 — Changed Compose host port from 8080 to 4001

**Why:** User requested a different port than the default 8080.

Before:
```yaml
    ports:
      - "8080:80"
```
After:
```yaml
    ports:
      - "4001:80"
```
README's compose run instructions updated to reference `http://localhost:4001` instead of `8080`.

## 2026-09-19 — Diagnosed and fixed Dokploy 404 on arya.prafullsaxena.cloud

**Why:** After deploying, `https://arya.prafullsaxena.cloud` returned Traefik's default "404 page not found" even though the container ran fine locally and via direct IP (`195.35.23.52:4001`).

Root cause: adding `docker-compose.yml` switched the Dokploy app's Build Type to "Docker Compose," which requires the Domains tab's **Container Port** to be the port the app listens on *inside* the container (`80`, nginx's port) — not the host-published port (`4001`) from the compose file. The Container Port field was showing `4001`; Service Name was unset.

Fix applied in Dokploy's Domains UI (no code change):
- Container Port: `4001` → `80`
- Service Name: (unset) → `site`
- Host: confirmed as `arya.prafullsaxena.cloud` (had briefly reverted to Dokploy's auto-generated `*.sslip.io` preview domain when the service name was changed — reset back manually)

## 2026-09-19 — Added `aryaalpha.com` as a second domain in Dokploy

**Why:** User wants the app reachable on an additional custom domain, alongside the existing `arya.prafullsaxena.cloud`.

Config added in Dokploy Domains tab (no code change): Host = `aryaalpha.com`, Service Name = `site`, Container Port = `80`, HTTPS = on. Requires a DNS A record for `aryaalpha.com` → `195.35.23.52`.

## 2026-09-14 — (baseline) Existing Dockerfile-based static site

**Why:** Pre-existing setup, included here as the baseline this changelog starts tracking from.

`site/Dockerfile` (unchanged, already in place before this session):
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```
