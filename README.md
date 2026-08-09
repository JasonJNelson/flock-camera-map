# Flock & ALPR Camera Map

Interactive nationwide map of **Flock Safety** and other automated license plate reader (ALPR) cameras across the United States.

Data comes from the open OpenStreetMap database via public Overpass API mirrors (the same source used by projects like DeFlock).

**Live demo:** Open `index.html` or enable GitHub Pages.

## Features

- Leaflet map with marker clustering
- Live viewport-based loading of ALPR cameras
- Filter: All ALPR or Flock Safety only
- City / address / ZIP search (Nominatim)
- Click any camera for details (manufacturer, operator, direction, OSM link)
- Educational panel explaining what Flock cameras do
- Multiple Overpass mirrors with automatic fallback for reliability

## Quick Start

### Option 1 — GitHub Pages (recommended)
1. Go to the repository **Settings → Pages**
2. Set Source to **Deploy from a branch**
3. Select branch `main` and folder `/ (root)`
4. Save — your map will be available at  
   `https://JasonJNelson.github.io/flock-camera-map/`

### Option 2 — Local
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

## Disclaimer

This is an independent educational / transparency tool.  
It is **not** affiliated with Flock Safety, any law-enforcement agency, or OpenStreetMap.

## License

Code: MIT  
Map data: © OpenStreetMap contributors (ODbL)