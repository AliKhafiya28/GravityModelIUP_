# Gravity Model Analysis — Kecamatan Cerme, Kabupaten Gresik

A static, self-contained website presenting a gravity-model analysis of residential
location attractiveness in Kecamatan Cerme — a companion study to the MCDA
(Multi-Criteria Decision Analysis) for residential area suitability.

## Contents

```
site/
├── index.html                  # Main project site (overview, methodology, findings, embeds)
├── results.html                # Interactive Leaflet results map (layers, legend, tooltips)
├── Gravity_Trip_Simulation.html# Animated agent trip-flow simulation
├── Cerme_Gravity_Model_Report.docx
├── data/
│   ├── boundary.geojson / .js     # Kecamatan boundary
│   ├── roads.geojson / .js        # Road network (820 segments)
│   ├── desa_pop.geojson / .js     # 25 desa, BPS population
│   ├── zones.geojson / .js        # 288 analysis zones, model outputs
│   ├── main_routes.geojson / .js  # 50 aggregated main-route segments (flow)
│   └── od_flows.geojson / .js     # 33 zone-to-zone OD flow lines
└── README.md                   # This file
```

The `.geojson` files are plain GeoJSON (WGS84 / EPSG:4326), useful for downloads or
re-use in QGIS/other tools. The `.js` files contain the same data wrapped as
`window.DATA_*` globals and are what `results.html` actually loads via `<script>`
tags — this avoids `fetch()`/CORS issues when the site is opened directly from the
filesystem (`file://`) as well as when hosted.

## Running locally

No build step is required — everything is static HTML/CSS/JS using CDN-hosted
Leaflet (`unpkg.com/leaflet@1.9.4`). Two ways to view it:

1. **Directly**: double-click `index.html` to open it in a browser. (The embedded
   `results.html` / `Gravity_Trip_Simulation.html` iframes and the `data/*.js`
   scripts work fine over `file://`.)
2. **Local server** (recommended, avoids any browser file-access restrictions):
   ```bash
   cd site
   python3 -m http.server 8000
   # then open http://localhost:8000
   ```

## Deploying

### GitHub Pages

1. Create a new GitHub repository and push the contents of this `site/` folder to
   the repo root (or to a `docs/` folder, or a `gh-pages` branch — your choice).
2. In the repo, go to **Settings → Pages**.
3. Under **Source**, select the branch/folder you pushed to (e.g. `main` / `/` or
   `main` / `docs`).
4. Save. GitHub will publish the site at
   `https://<username>.github.io/<repo-name>/` within a minute or two.
5. Visit the URL — `index.html` is served automatically as the home page.

```bash
# Example from inside this site/ folder
git init
git add .
git commit -m "Gravity model results site"
git branch -M main
git remote add origin https://github.com/<username>/<repo-name>.git
git push -u origin main
```

### Netlify

**Option A — Drag and drop (fastest):**
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the entire `site/` folder onto the page.
3. Netlify uploads and deploys it instantly, giving you a live URL
   (e.g. `https://random-name-123.netlify.app`).

**Option B — Git-based:**
1. Push `site/` to a GitHub/GitLab/Bitbucket repo (as above).
2. In Netlify, click **Add new site → Import an existing project** and connect the
   repo.
3. Leave the build command empty and set the publish directory to the folder
   containing `index.html` (e.g. `site` or `/` if `site/` is the repo root).
4. Deploy — Netlify will host it and give you a live URL, with HTTPS and a custom
   domain option.

### Other static hosts

Because the site has **no build step and no server-side code**, it can be deployed
to any static host that serves files as-is: Vercel, Cloudflare Pages, S3 +
CloudFront, Surge, Firebase Hosting, etc. Just upload/point the host at the
`site/` folder.

## Notes / Limitations

- `Gravity_Trip_Simulation.html` is ~4.4 MB (it embeds the full agent-path
  dataset). This is fine for static hosting but may load slowly on very slow
  connections.
- All map layers use CARTO basemap tiles (`light_all`) and Leaflet from the
  `unpkg.com` CDN — an internet connection is required to see basemap tiles and
  load Leaflet, even when the site itself is opened locally.
- Coordinates were converted from the source CRS (WGS_1984_UTM_Zone_49S /
  EPSG:32749) to WGS84 (EPSG:4326) for web mapping.
