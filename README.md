<div align="center">

<img src="Phased Approach_v2.svg" width="120" alt="Phased Approach logo" />

# RunViewerMulti

**A GPS telemetry comparison tool for motorsport data analysis**

*A [Phased Approach](https://phasedapproach.com) product*

---

![No build required](https://img.shields.io/badge/no%20build-required-2ea043?style=flat-square)
![Zero dependencies](https://img.shields.io/badge/dependencies-zero-2ea043?style=flat-square)
![Browser based](https://img.shields.io/badge/runs%20in-the%20browser-58a6ff?style=flat-square)
![Single file](https://img.shields.io/badge/frontend-single%20HTML%20file-f0883e?style=flat-square)

</div>

---

## What it does

RunViewerMulti lets you load two or more motorsport data logger CSV files and replay them simultaneously on an interactive GPS map — comparing speed, gear, throttle, brake, RPM, sector times, G-forces, and line choice side by side.

Everything runs locally in the browser. No server, no install, no data upload.

## Quick start

```
1. Clone or download this repo
2. Open index.html in any modern browser
3. Drop your CSV files onto the screen
```

That's it.

---

## Features

### Multi-run GPS overlay
Load as many runs as you want. Each is assigned a distinct color and rendered simultaneously as a polyline on the map. A moving marker tracks each run's real-time position during playback.

### Synchronized playback
A shared timeline drives all runs in lockstep with adjustable speed — ½×, 1×, 2×, 5×, and 10×. Scrub to any point with the timeline slider or click directly on the map.

### Location-based map seek
Click anywhere on the GPS trace to snap **all runs to the same physical location** simultaneously — not the same timestamp. Each run independently finds its closest GPS point to the clicked coordinate. Useful for comparing brake points, apexes, and throttle application zones.

### Sector time analysis
Sectors are auto-detected from speed data. The sector table shows:
- Lap time per sector per run
- Delta vs. the baseline run (color-coded green/red)
- Distance driven per sector — shortest line shown in green, longer lines show only the difference in red

### Baseline selection
Any run can be pinned as the baseline for delta calculations. Defaults to the fastest overall lap time.

### Trim to course
Set GPS-based start and finish waypoints to exclude out-laps, in-laps, and paddock sections. Each run independently snaps to the nearest GPS point for each boundary. Sector times recalculate automatically after trimming.

### Live metrics panel
Per-run real-time display of speed (mph), gear, RPM, throttle bar, brake bar, and cumulative delta vs. baseline.

### G-force display
Live lateral/longitudinal G-force dot with a fading per-run trail on a circular reference plot.

### Map layers
Toggle between CartoDB Dark Matter (street) and Esri World Imagery (satellite).

### Drag-and-drop everywhere
Drop CSV files onto the initial full-screen overlay **or** the persistent drop zone in the header bar — add runs at any point during a session.

---

## CSV format

The viewer expects a CSV with the following columns. Column names are case-insensitive.

| Column | Required | Description |
|--------|----------|-------------|
| `Time (ms)` or `t` | ✅ Required | Timestamp in milliseconds |
| `Latitude` or `lat` | ✅ Required | GPS latitude, decimal degrees |
| `Longitude` or `lon` | ✅ Required | GPS longitude, decimal degrees |
| `Speed (mph)` or `speed` | Optional | Vehicle speed in mph |
| `RPM` or `rpm` | Optional | Engine RPM |
| `Gear` or `gear` | Optional | Current gear (0 = neutral) |
| `Throttle (%)` or `throttle` | Optional | Throttle position 0–100 |
| `Brake (%)` or `brake` | Optional | Brake pressure 0–100 |
| `Lat G` or `latg` | Optional | Lateral G-force |
| `Lon G` or `long` | Optional | Longitudinal G-force |
| `Lap` or `lap` | Optional | Lap number — enables per-lap selection |

When a `Lap` column is present, the viewer splits the session into individual laps. Each run pill in the header includes a lap selector so runs can display different laps independently.

---

## File structure

```
run_viewer_multi/
├── index.html          # Full application — single self-contained file
├── help.html           # In-app documentation (opens in new tab)
└── Phased Approach_v2.svg  # Brand logo
```

No `node_modules`. No build step. No config files. The entire application is `index.html`.

---

## Tech stack

| Library | Version | Use |
|---------|---------|-----|
| [Leaflet.js](https://leafletjs.com) | 1.9.4 | Interactive GPS map, canvas renderer |
| CartoDB Dark Matter | — | Default map tile layer |
| Esri World Imagery | — | Satellite map tile layer |

Everything else — CSV parsing, playback engine, sector detection, G-force display, UI — is vanilla JavaScript with no framework. Loaded from CDN; no local dependencies.

---

## How key features work

### GPS seek (map click)

When you click the map, the viewer runs an O(n) proximity search over each run's GPS data independently. Each run finds its own closest point to the clicked coordinate and stores a `timeOffset` — the difference between the shared timeline position and that run's lap time at the matched GPS point. During playback, each run advances from its own GPS-aligned position rather than a shared timestamp.

Clicking Stop or dragging the slider clears all offsets, re-synchronizing runs by time.

### Trim

Start and finish waypoints store GPS coordinates (lat/lon), not timestamps. When trim is applied, each run slices its own lap data from its closest point to the start waypoint through its closest point to the finish waypoint. Sector times are fully recalculated on the trimmed data.

### Sector detection

Sectors are detected by finding local speed minima that cross a defined threshold. Sector boundaries are marked on the map with numbered yellow circles positioned on the baseline run's trace. Distance per sector is calculated using an equirectangular GPS approximation (accurate for the short distances typical of a motorsport sector).

---

## Documentation

Full in-app documentation is available at [`help.html`](help.html), accessible via the **Help** button in the top-right of the viewer header. Topics covered:

- CSV format reference
- Loading and managing runs
- Playback controls
- Map navigation and layer toggle
- Sector time table and delta reading
- Baseline selection
- Trim controls
- G-force display
- Map click seek — how GPS offsets work
- Workflow tips for corner analysis and driver comparison

---

## Browser support

Any modern browser with ES6+ and Canvas support. Tested in Chrome, Edge, and Firefox.

> Safari users: ensure "Allow cross-origin requests" is not blocked for local file access, or serve the files from a local HTTP server.

---

## License

© Phased Approach. All rights reserved.

---

<div align="center">

Built by **[Phased Approach](https://phasedapproach.com)** — Celeritas per physicam et scientiam.

</div>
