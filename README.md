# 🚚 LogisticsRoute Pro — Saudi Arabia Heavy Truck & Fuel Calculator

> Fleet road routing & fuel system for KSA heavy-truck operations — **zero-build, single-file web app**.

A pure **client-side** application for planning **multi-stop truck routes**, simulating the **fuel & mission math** (distance, liters, tank sufficiency, driving time, mandatory rest breaks, road delays, ETA) and pushing results into **KML (Google Earth)**, a **Google Maps directions link**, or a **WhatsApp summary** — everything inside one HTML file.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-38B2AC?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Leaflet](https://img.shields.io/badge/Leaflet%201.9-199900?style=for-the-badge&logo=leaflet&logoColor=white)
![OpenStreetMap](https://img.shields.io/badge/OpenStreetMap-7EBC6F?style=for-the-badge&logo=openstreetmap&logoColor=white)
![OSRM Router](https://img.shields.io/badge/OSRM%20Router-1e293b?style=for-the-badge)
![Font Awesome](https://img.shields.io/badge/Font%20Awesome-528DD7?style=for-the-badge&logo=fontawesome&logoColor=white)
![100% Client-Side](https://img.shields.io/badge/100%25%20Client--Side-0ea5e9?style=for-the-badge)
![No Build Required](https://img.shields.io/badge/No%20Build%20Required-16a34a?style=for-the-badge)
![Status: Active](https://img.shields.io/badge/Status-Active-22c55e?style=for-the-badge)

---

## ✨ Features

| 🎯 Feature | What it does |
|---|---|
| 🗺️ **Multi-stop route planning** | Start point, **drag-and-drop reorderable** destination stops, optional return point |
| 🖱️ **Click-to-set on map** | Click anywhere and set it as **Start** / **Add as Stop** / **Return** from one popup |
| 📍 **Live coordinate preview** | Markers + permanent tooltips update as you type `lat, lon` and auto-fit the view |
| 🔍 **Jump-to-coordinates widget** | Paste `lat, lon` and fly the map to that spot instantly |
| 🧭 **5 base-map styles** | Standard OSM, English Streets (Esri), Voyager (CARTO), Light Map (Carto), Hybrid Satellite (Esri) |
| ⚖️ **Truck load presets** | Empty / Partial / Full / Overloaded — each auto-applies a realistic km/L efficiency |
| ⛽ **Fuel tank module** | Tank fuel, **auxiliary fuel** (PTO / idling / reefer), km/L, price (SAR/L) — **surplus** or **refuel-deficit** status |
| 🛡️ **Saudi TGA Compliance Mode** | Enforces **4.5 h** max driving, **45-minute** mandatory breaks, **11 h** daily rest (> 9 h driving) |
| ⏱️ **Mission timeline & ETA** | Departure date/time → arrival, split into driving / breaks / daily rest / delays |
| 🛣️ **OSRM road routing** | Real road polyline & distance; **Haversine straight-line fallback** with dashed line if offline |
| 📦 **Exports & integrations** | **KML** (Google Earth pins + path), **Google Maps link** (with waypoints), **WhatsApp** summary |
| 💾 **Profile save / load** | Full parameter set exported / imported as **JSON** profile files |
| ⚙️ **Settings panel** | Live preview + show/hide KML, GMaps, WhatsApp toolbar buttons — saved in `localStorage` |

---

## 🚀 Quick Start

No build step — just open the file:

1. Open **`travel-timing-calculator.html`** in any modern browser (double-click works — it runs from `file://`).
2. Enter a start coordinate in `lat, lon` format, e.g. Riyadh: `24.7136, 46.6753`.
   - Easier: toggle **Click to Set: ON**, click the map — or use the **Jump to Coordinates** widget.
3. Add destination stops with **+ Add Stop** (drag 🖐️ to re-order) and set an optional return point (leave empty for one-way).
4. Choose truck **load weight**, set **available fuel**, **extra fuel**, **km/L**, **price/L** (SAR), **avg. speed**, **delays** and **departure** date/time.
5. Keep **Saudi TGA Compliance** ON (default) for KSA driver rules — or switch OFF for custom rest intervals.
6. Press **🧮 Calculate Truck Route & Fuel** and read the summary cards + modal.

> ⚠️ Internet is required — tile servers, OSRM router and CDNs load at runtime. If the router is unavailable, the app falls back to straight-line (approximate) distances with a dashed route preview.

---

## 🧮 Under the Hood (calculation engine)

```text
Road distance (km)   = OSRM driving route         (fallback: Haversine straight line)
Driving fuel (L)     = distance ÷ efficiency (km/L)
Extra fuel (L)       = auxiliary / PTO / idling / reefer fuel
Fuel required (L)    = driving fuel + extra
Fuel cost (SAR)      = fuel required × price (SAR/L)
Driving time (h)     = distance ÷ average speed (km/h)
Rest time (h)        = TGA rules  OR  custom interval × rest hours
Total mission (h)    = driving + rests + daily rest + road delays
ETA                   = departure date/time + total mission time
```

### ⚖️ Truck load → efficiency presets

| Load status | Weight | Built-in efficiency |
|---|---|---|
| 🟢 Empty Truck | 0 t | 3.2 km/L |
| 🟡 Partial Load | 15 t | 2.5 km/L |
| 🟠 Full Load | 25 t | 2.0 km/L |
| 🔴 Overloaded | 28 t | 1.5 km/L |

### 🛡️ Saudi TGA Compliance rules (default ON)

| Rule | Preset |
|---|---|
| Max continuous driving per stint | **4.5 h** |
| Mandatory break after each stint | **45 min** |
| Daily rest when driving > 9 h | **11 h** |

Breaks count = `max(0, ⌈driving hours ÷ 4.5⌉ − 1)`. With TGA OFF the app uses your custom **Rest interval (km)** and **Rest hours per interval**.

---

## 🧩 Inputs & Parameters

| Parameter | Unit | Default |
|---|---|---|
| Start point / destination stops / return point | `lat, lon` | — |
| Truck load weight | — | Full Load (25 t) |
| Available fuel in tank | L | 0 |
| Extra fuel (auxiliary / PTO / idling / reefer) | L | 0 |
| Fuel efficiency | km/L | 2.0 |
| Fuel price | SAR/L | 1.79 |
| Average truck speed | km/h | 75 |
| Road delay | hrs | 0 |
| Rest interval | km | 500 |
| Rest hours per interval | hrs | 1.0 |
| Saudi TGA compliance mode | on/off | ON |
| Departure date & time | local datetime | now |

## 📊 Mission Summary

| Output | Description |
|---|---|
| 📏 **Total Distance** | Route length (km) |
| ⛽ **Fuel Required** | Liters + cost in **SAR** |
| ⏱️ **Travel Duration** | Driving + rests + delays (h/m) |
| 📅 **Expected Arrival** | Local timestamp computed from departure |
| ✅ / ⚠️ **Fuel Status** | Sufficient (surplus L) or refuel needed (deficit L) |
| 🧾 **Mission Summary** | Load label, stop count, road vs approximate route |

---

## 📁 Project Structure

```text
LogisticsRoute Pro/
├── travel-timing-calculator.html   # The entire app — UI, map, engine & exports
└── README.md                       # This documentation
```

Single-file architecture — HTML structure, Tailwind styling, Leaflet map and all Vanilla JS (`LogisticsOpenStreetMapApp` class) ship in one file with **zero compile step**.

## 🛠️ Built With

| Stack | Purpose | Source |
|---|---|---|
| 🟨 JavaScript (ES6) | All application logic — no frameworks | Inline |
| 🟦 HTML5 + Tailwind CSS | Responsive UI & styling | Tailwind CDN |
| 🍃 Leaflet 1.9 | Interactive map | unpkg CDN |
| 🗺️ OpenStreetMap / Esri / CARTO | Base-map tile styles | CDN |
| 🛣️ OSRM | Public driving-route API | `router.project-osrm.org` |
| 🅰️ Font Awesome 6.4 + Inter | Icons & typography | CDN |

## ✅ Notes & Limitations

- **Modern browser** recommended (Chrome, Edge, Firefox, Safari).
- KML export prefers the **File System Access API** (Chrome/Edge) with an automatic download fallback elsewhere.
- Preferences persist in `localStorage`.
- The public OSRM instance is shared — when busy it falls back to approximate straight-line distances (labelled *Approximate*).
- TGA logic is a best-effort implementation of common KSA driving-hours rules — always verify against official **Transport General Authority (TGA)** guidance.

## 🙌 Credits

- Map tiles © OpenStreetMap contributors, Esri, CARTO
- Routing powered by the open-source [OSRM](https://project-osrm.org/) project
- Icons by Font Awesome · Mapping library by [Leaflet](https://leafletjs.com/)

---

© 2026 Bernando Jr Minguita. All rights reserved.
