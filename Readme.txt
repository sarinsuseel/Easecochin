# 🗺️ Kochi Route Finder — No Google Maps

A Proof of Concept (PoC) routing application for Kochi, Kerala built entirely with **free and open-source tools** — no Google Maps, no paid APIs, no usage limits.

---

## 📸 Demo

> Enter two locations in Kochi → Get a real driving route instantly

![Kochi Route Finder Demo](screenshots/demo.png)

---

## ✨ Features

- 🔍 **Smart Search** — Type any location in Kochi and get autocomplete suggestions
- 🗺️ **Click on Map** — Click anywhere on the map to pick Start or End location
- 🛣️ **Real Road Routing** — Routes follow actual Kochi roads using OSRM
- 📏 **Distance & Duration** — Shows exact distance (km) and travel time
- 📵 **Offline Detection** — Alerts user when internet connection is lost
- 🗑️ **Clear & Reset** — One click to clear everything and start again
- ❌ **Zero Google Maps** — Completely free, no API keys needed

---

## 🛠️ Tech Stack

| Tool | Purpose | Cost |
|---|---|---|
| [Leaflet.js](https://leafletjs.com/) | Interactive map display | Free |
| [OSRM](http://project-osrm.org/) | Road routing engine | Free & Open Source |
| [Nominatim](https://nominatim.org/) | Place name search (geocoding) | Free |
| [OpenStreetMap](https://www.openstreetmap.org/) | Map tiles & road data | Free |
| [Docker](https://www.docker.com/) | Run OSRM locally | Free |

**Monthly server cost for production: ~₹900/month (Hetzner) — unlimited requests**

---

## 🚀 How to Run Locally

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed
- [Git](https://git-scm.com/) installed

---

### Step 1 — Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/kochi-route-poc.git
cd kochi-route-poc
```

---

### Step 2 — Download Kochi Map Data

1. Go to 👉 [https://extract.bbbike.org/](https://extract.bbbike.org/)
2. Draw a box around Kochi city on the map
3. Select format: **Protocolbuffer (PBF)**
4. Download and place the `.osm.pbf` file inside the `osrm-data/` folder

---

### Step 3 — Process Map with OSRM

Open terminal inside the `osrm-data/` folder and run these 3 commands:

```bash
# 1. Extract roads for car routing
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-extract -p /opt/car.lua /data/YOUR_FILE.osm.pbf

# 2. Partition the road network
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-partition /data/YOUR_FILE.osrm

# 3. Customize weights
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-customize /data/YOUR_FILE.osrm
```

> Replace `YOUR_FILE` with your actual downloaded filename

---

### Step 4 — Start OSRM Server

```bash
docker run -t -i -p 5000:5000 -v "C:\osrm-kochi\osrm-data:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/planet_76.206,9.907_76.334,9.988.osrm
```

Server will start at `http://localhost:5000` ✅

---

### Step 5 — Open the App

Open `index.html` in your browser — that's it! No build tools, no npm install.

---

## 📁 Project Structure

```
kochi-route-poc/
│
├── index.html          ← Complete frontend (single file)
├── osrm-data/          ← Place your .osm.pbf file here
│   └── .gitkeep
├── screenshots/        ← Demo screenshots
│   └── demo.png
└── README.md           ← This file
```

---

## 🔄 Switching from Local to Production

When moving to a live server, change just **one line** in `index.html`:

```javascript
// Local development
const OSRM_BASE = 'http://localhost:5000';

// Production (your own Hetzner server)
const OSRM_BASE = 'https://your-server.com';
```

Everything else stays exactly the same.

---

## 🗺️ How It Works

```
User types location name
        │
        ▼
Nominatim API converts name → coordinates
        │
        ▼
OSRM calculates best driving route
        │
        ▼
Leaflet draws blue route line on map
        │
        ▼
Distance + Duration displayed
— All without Google Maps!
```

---

## 📍 Map Coverage

This PoC covers **Kochi City, Kerala, India**.
For production, the map data can be expanded to cover all of Kerala or India.

---

## 🔮 Future Roadmap

- [ ] Self-hosted Photon for faster search
- [ ] Weekly automatic OSM data updates
- [ ] Turn-by-turn navigation
- [ ] Traffic-aware routing
- [ ] Mobile app wrapper

---

## 👨‍💻 Author

Built as a Proof of Concept to demonstrate Google Maps-free routing for Kerala.

---

## 📄 License

MIT License — free to use and modify.