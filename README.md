# Apple Motion Findings

Static site with the results of the Apple-style motion research for marketing videos: principles, reference films and a recipe for one launch video.

## Run locally

```bash
docker build -t motion-findings . && docker run --rm -p 8080:8080 motion-findings
```

Open http://localhost:8080.

## Deploy on Railway

1. Railway → **New Project** → **Deploy from GitHub repo** → pick this repo.
2. Railway builds the `Dockerfile` (see `railway.json`). Caddy listens on `$PORT`, which Railway sets.
3. Service → **Settings** → **Networking** → **Generate Domain**.

Every push to `main` redeploys automatically.

## Structure

- `site/` — `index.html`, demo video and poster
- `Caddyfile` — static file server on `$PORT`
- `Dockerfile` — `caddy:2-alpine` image
- `railway.json` — build and healthcheck config
