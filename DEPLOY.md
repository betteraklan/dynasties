# Aklan Dynasty Map: Deployment Instructions

Contributed by Rance Ashriel R. Sabino

## 1. Overview

The Aklan Dynasty Map is a static website built with HTML, CSS and JavaScript. It requires no server-side code, database or build process.

## 2. Package Contents

| Path | Description |
|---|---|
| `index.html` | Main page containing the layout, styles and application code |
| `data/aklan.js` | Election data and municipal boundaries (generated file) |
| `data/families.js` | Documented family relationships and their sources |
| `assets/logo.png` | Site logo |
| `assets/favicon.png` | Browser tab icon |
| `photos/` | Reserved for optional portraits on family cards |
| `scripts/build_data.py` | Script for regenerating `data/aklan.js` after future elections |
| `README.md` | Technical documentation, data sources and methodology |

## 3. Installation

1. Upload the entire `aklan-dynasty-map` folder to the web server.
2. Preserve the folder structure. The `data/`, `assets/` and `photos/` folders must remain in the same directory as `index.html`.
3. Open the page in a browser at its published address, for example `https://<website>/aklan-dynasty-map/`.

The site may also be hosted on any static hosting service, such as GitHub Pages, Netlify or Cloudflare Pages.

## 4. Integration with an Existing Website

The map may be published in either of the following ways:

- **As a standalone page:** link to the folder's `index.html` from the website's navigation.
- **As an embedded page:** insert it using an iframe, for example:

  ```html
  <iframe src="/aklan-dynasty-map/" style="width:100%;height:1600px;border:0" title="Aklan Dynasty Map"></iframe>
  ```

To use the website's own header and footer instead of the page's, remove the `<nav class="top">` and `<footer>` sections from `index.html`. The contributor credit should remain visible on the page.

## 5. External Services

The page loads the following resources at runtime. If the website enforces a Content Security Policy, these hosts must be permitted.

| Host | Purpose |
|---|---|
| `cdnjs.cloudflare.com` | D3.js 7.9.0 and Leaflet 1.9.4 libraries (scripts) |
| `fonts.googleapis.com`, `fonts.gstatic.com` | Inter and Roboto Mono fonts |
| `server.arcgisonline.com` | Map background tiles (Esri World Light Gray Canvas; no API key required) |

The map attribution for Esri and OpenStreetMap must remain visible. The tile provider may be changed in the `L.tileLayer` line of `index.html`.

## 6. Maintenance

- **Changes in office** (resignation, removal or death): add a `status` entry for the official in `data/families.js`.
- **Family updates:** after editing `data/families.js`, update its `"checked"` date. The page displays this date as "Families last checked".
- **Future elections:** after the May 2028 elections, regenerate `data/aklan.js` using `scripts/build_data.py`, as described in `README.md`.
