# Arya Alpha Capital — Website

Marketing website for **Arya Alpha Capital**, a research-led investment
distribution business (Mutual Funds · PMS · Long-Only AIFs) founded by
Paridhi Agrawal (CA, CFA Level II).

Static site — no build step, no framework. Content and brand palette
(navy `#12213A` / cream `#F4EFE4` / gold `#C9A24B`) are sourced from
`ARYA_ALPHA_2Pager.pdf` / `assets/pamphlet.html` in the parent
[`arya-alpha`](..) project.

## Structure

- `index.html` — the entire single-page site (hero, philosophy, process,
  investment universe, research framework, who-we-serve, founder bio,
  contact/CTA, footer disclaimer).
- `assets/` — logo files (SVG + PNG, light/dark/icon variants).
- `Dockerfile` — serves the static files via `nginx:alpine` on port 80.

## Run locally

No dependencies — any static file server works:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Or via the same Docker image used in production:

```bash
docker build -t arya-alpha-site .
docker run --rm -p 8080:80 arya-alpha-site
# then open http://localhost:8080
```

Or with Docker Compose:

```bash
docker compose up --build
# then open http://localhost:8080
```

## Deploy (Dokploy)

1. Push this repo to GitHub (see below).
2. In Dokploy: **Projects → Create Service → Application**, source =
   this GitHub repo, build type = **Dockerfile**, container port = `80`.
3. Add the domain under the app's **Domains** tab and enable Let's
   Encrypt SSL. Point the domain's DNS A record at the Dokploy server.
4. Click **Deploy**.

## Contact

- Email: paridhi.connect@gmail.com
- Phone: +91 97021 50909
