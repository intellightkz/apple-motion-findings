# Apple Motion Findings

**Start here: [START.md](START.md)** — what to replace (name, ID, photo) and how to deploy.

Static site with the results of the Apple-style motion research for marketing videos: principles, reference films and a recipe for one launch video.

## Run locally

```bash
cp .env.example .env
```

```bash
docker build -t motion-findings . && docker run --rm -p 8080:8080 --env-file .env motion-findings
```

Open http://localhost:8080.

## Deploy on Railway

1. Railway → **New Project** → **Deploy from GitHub repo** → pick this repo.
2. Railway builds the `Dockerfile` (see `railway.json`). Caddy listens on `$PORT`, which Railway sets.
3. Service → **Settings** → **Networking** → **Generate Domain**.
4. Service → **Variables**: `OWNER_NAME`, `OWNER_ID`. The photo is the file `site/photo.jpg` (see START.md).

Every push to `main` redeploys automatically.

## Structure

- `site/` — `index.html`, `photo.jpg`, demo video and poster
- `Caddyfile` — static file server on `$PORT`, fills `OWNER_NAME` / `OWNER_ID` env vars into the HTML
- `Dockerfile` — `caddy:2-alpine` image
- `railway.json` — build and healthcheck config
