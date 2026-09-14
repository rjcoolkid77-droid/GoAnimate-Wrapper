# Base44 Dev Environment

## Project Overview
GoAnimate Wrapper — a plain Node.js HTTP server (no framework) that wraps GoAnimate's retired Legacy Video Maker API. Serves HTML pages and API endpoints for character creation, movie making, TTS, and asset management.

## Running the App
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
The app listens on port 3000 (set via `PORT` env var; defaults to 80 in `config.json`).

## Key Facts
- **No database** — uses the filesystem (`_SAVED/`, `_CACHÉ/` folders) for persistence.
- **No external credentials** — references public GitHub URLs for SWF/store/client assets (see `env.json`).
- **Dependencies** are pure JS (brotli, formidable, js-md5, mp3-duration, node-zip, xmldoc); no native compilation needed.
- **Entry point**: `main.js` → `server.js` (plain `http.createServer`).
- **Dev server**: `node --watch main.js` (Node 22 built-in file watcher for live reload).
- **Routing**: `server.js` iterates an array of handler functions from subdirectories (`character/`, `movie/`, `asset/`, `static/`, `theme/`, `tts/`). Each handler returns truthy if it handled the request.
- **Static file config**: `static/info.json` maps URL patterns to files and inline content. `static/load.js` serves them.
- **Homepage**: `/` redirects to `/html/list.html`.

## Verifying the App
- `curl -s -o /dev/null -w '%{http_code}' http://localhost:3000/html/list.html` should return 200.
- `curl -s -I http://localhost:3000/` should return 302 redirect to `/html/list.html`.
