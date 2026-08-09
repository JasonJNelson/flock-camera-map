# Flock & ALPR Camera Map

Interactive nationwide map of **Flock Safety** and other automated license plate reader (ALPR) cameras across the United States.

Data comes from the open OpenStreetMap database via public Overpass API mirrors (the same source used by projects like DeFlock).

**Live demo:** https://JasonJNelson.github.io/flock-camera-map/ (after first successful deploy)

## Features

- Leaflet map with marker clustering
- Live viewport-based loading of ALPR cameras
- Filter: All ALPR or Flock Safety only
- City / address / ZIP search (Nominatim)
- Click any camera for details (manufacturer, operator, direction, OSM link)
- Educational panel explaining what Flock cameras do
- Multiple Overpass mirrors with automatic fallback for reliability

## CI/CD

This repository uses **GitHub Actions**:

| Workflow | File | Trigger | Purpose |
|----------|------|---------|---------|
| **CI** | `.github/workflows/ci.yml` | Push & Pull Request to `main` | Validates HTML, checks required files and critical JS functions |
| **Deploy** | `.github/workflows/deploy.yml` | Push to `main` + manual | Builds and deploys the static site to GitHub Pages |

### Enable GitHub Pages (one-time)

1. Go to the repository → **Settings** → **Pages**
2. Under **Build and deployment** → **Source**, select **GitHub Actions**
3. The next push to `main` (or a manual workflow run) will publish the site

Live URL will be:  
`https://JasonJNelson.github.io/flock-camera-map/`

You can also trigger a deploy manually from the **Actions** tab → **Deploy to GitHub Pages** → **Run workflow**.

## Quick Start (local)

```bash
git clone https://github.com/JasonJNelson/flock-camera-map.git
cd flock-camera-map
python3 -m http.server 8000
# Then open http://localhost:8000
```

> **Important:** Opening the raw `index.html` as a `file://` page can cause "Failed to fetch" errors because of browser security restrictions. Always serve it over HTTP (local server or GitHub Pages).

## How to use the map

1. Zoom in to zoom level **9 or higher** (or search for a city).
2. Cameras load automatically for the visible area.
3. Use the dropdown to show only Flock Safety cameras or all ALPRs.
4. Click markers for details.
5. Use **Refresh View** if needed.

## Data source & limitations

- Source: OpenStreetMap nodes tagged `surveillance:type=ALPR`
- Crowdsourced — coverage is incomplete, especially in rural areas
- Cameras can be moved, removed, or mis-tagged
- Flock Safety claims 120,000+ cameras; independent maps currently document roughly 95k–127k ALPRs (majority Flock)

Contribute missing or incorrect cameras via the [DeFlock app](https://deflock.me) or any OpenStreetMap editor.

## Tech stack

- Pure HTML / CSS / JavaScript (no build step)
- [Leaflet](https://leafletjs.com/) + MarkerCluster
- Overpass API (multiple public mirrors)
- Nominatim for geocoding
- Carto dark basemap
- GitHub Actions for CI + Pages deployment

## Disclaimer

This is an independent educational / transparency tool.  
It is **not** affiliated with Flock Safety, any law-enforcement agency, or OpenStreetMap.

## License

Code: MIT  
Map data: © OpenStreetMap contributors (ODbL)
