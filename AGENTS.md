# Base44 Setup Notes

## Project
GoAnimate Wrapper — a Node.js HTTP server that wraps GoAnimate's Legacy Video Maker (Flash/SWF based). Serves static files, SWF pages, and API endpoints for character creation, movie editing, TTS, and asset management.

## Running
- `docker compose -f docker-compose.base44.yml up -d` starts the app on port 3000.
- The app uses `node main.js` (which requires `server.js`). It listens on the port from `PORT` env or `SERVER_PORT` in `config.json` (default 80). Compose sets `PORT=3000`.
- Dependencies install automatically on container startup via `npm install`.
- No external credentials or secrets are needed. Asset URLs point to GitHub Pages (`config.json`).

## Verification
- `curl -sL http://localhost:3000/` should return the HTML list page (redirects from `/` to `/html/list.html`).
- Healthcheck probes `http://localhost:3000/` for a non-5xx response.

## Notes
- This is a Flash (SWF) application; the SWF content loads from external GitHub Pages URLs, not from this server.
- Data folders (`_SAVED`, `_CACHÉ`, `_THEMES`, `_PREMADE`, `_EXAMPLES`) are part of the repo and bind-mounted into the container.
