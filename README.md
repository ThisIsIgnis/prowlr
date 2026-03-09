# Prowlr

**A mobile-first search UI for Prowlarr.** Browse all your configured indexers, filter by category, and send torrents to qBittorrent in one tap — no app install required, runs in any browser.

> Designed for phones and Android TV. Works on desktop too.

![Prowlr preview](screenshot.png)

---

## Why

Prowlarr's built-in search is desktop-only and buried in the settings UI. Prowlr is a lightweight companion that puts search front and center, optimized for touch and small screens.

---

## Features

- Search across all Prowlarr indexers simultaneously
- Filter by category with customizable filter buttons
- Sort results by date, size, seeders, or title
- One-tap send to qBittorrent via Prowlarr's native grab endpoint
- Confirmation dialog before sending
- Per-result info link to the torrent page
- First-run setup screen — no config file editing
- Customizable colors (accent, background, surface, text)
- Single HTML file — no build step, no dependencies

---

## Requirements

- [Prowlarr](https://github.com/Prowlarr/Prowlarr) with at least one indexer configured
- qBittorrent set as a download client in Prowlarr
- Docker (for the recommended deployment) or any web server

---

## Deployment

### Docker run

```bash
docker run -d \
  --name prowlr \
  -p 8989:80 \
  -v /path/to/prowlr.html:/usr/share/nginx/html/index.html:ro \
  --restart unless-stopped \
  nginx:alpine
```

Open `http://localhost:8989`.

---

### Docker Compose

```bash
git clone https://github.com/ThisIsIgnis/prowlr.git
cd prowlr
docker compose up -d
```

Open `http://localhost:8989`.

---

### No Docker

Open `prowlr.html` directly in a browser. Note: some browsers block cross-origin requests from `file://` URLs. Serving via a local web server avoids this.

```bash
python3 -m http.server 8989
```

---

## Setup

On first load, a setup screen appears automatically:

1. Enter your **Prowlarr URL** — e.g. `http://localhost:9696`
2. Enter your **Prowlarr API key** — found in Prowlarr under Settings → General → API Key
3. Click **Save**

Settings are stored in `localStorage` and persist across sessions. Tap **⚙** at any time to change them.

---

## Settings

### Categories

Add, remove, or rename the filter buttons shown on the search screen. Each entry needs a label and a Prowlarr category ID.

Common category IDs:

| Category | ID |
|---|---|
| Movies | 2000 |
| TV | 5000 |
| Music | 1000 |
| PC / Software | 4000 |
| Books | 8000 |
| XXX | 6000 |
| Other | 7000 |

Full list: [Newznab/Torznab category standard](https://github.com/Prowlarr/Prowlarr/wiki/Supported-Indexers)

### Colors

Customize accent, background, surface, and text colors. Changes preview live before saving.

---

## How sending works

Prowlr uses Prowlarr's `/api/v1/search` grab endpoint. When you tap **Send to qBit**, Prowlr sends the result's `guid` and `indexerId` to Prowlarr, which handles forwarding the torrent to qBittorrent. The browser never talks to qBittorrent directly — no credentials needed.

---

## Updating

Replace `prowlr.html` with the latest version. Settings stored in `localStorage` are unaffected.

---

## License

MIT
