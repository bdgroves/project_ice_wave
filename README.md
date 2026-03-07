# 🦣 Project IceWave

![IceWave Flag](assets/icewave_flag.png)

[![Python 3.12](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/)
[![Pixi](https://img.shields.io/badge/env-pixi-green.svg)](https://prefix.dev/)
[![Status](https://img.shields.io/badge/status-Phase%201%20In%20Progress-blue.svg)]()
[![Sister Project](https://img.shields.io/badge/sister%20project-PaleoWave-gold.svg)](https://github.com/bdgroves/Project-PaleoWave)

---

*The ice came. The mammoths came with it. Then both disappeared — but the bones remain.*

During the Pleistocene epoch (2.6M–11,700 years ago), Columbian mammoths (*Mammuthus columbi*) and woolly mammoths (*Mammuthus primigenius*) roamed the Pacific Northwest and Great Basin. Ancient lakes filled Nevada's valleys. Glaciers carved Washington's channeled scablands. And everywhere they walked, they left bones.

Project IceWave asks: **where haven't we looked yet?**

---

## 🎯 Mission

Use machine learning on USGS terrain data, Quaternary geology, and PBDB occurrence records to predict undiscovered Pleistocene megafauna fossil localities across **Washington, Oregon, and Nevada**.

---

## 🦣 Target Species

| Species | Common Name | Region |
|---------|-------------|--------|
| *Mammuthus columbi* | Columbian Mammoth | All three states |
| *Mammuthus primigenius* | Woolly Mammoth | WA, OR |
| *Mammut americanum* | American Mastodon | WA, OR |
| *Megalonyx jeffersonii* | Jefferson's Ground Sloth | WA, OR |
| *Equus* sp. | Pleistocene Horse | NV |
| *Camelops hesternus* | Yesterday's Camel | NV |

---

## 📁 Project Structure

```
project_ice_wave/
├── notebooks/
│   ├── 01_pbdb_harvester.ipynb       # PBDB API → Pleistocene occurrences
│   ├── 02_terrain_analysis.ipynb     # USGS 3DEP DEM → terrain metrics
│   └── 03_ml_model.ipynb             # Random Forest → prediction surface
├── data/
│   ├── pbdb/                         # Occurrence CSVs + GeoJSON
│   ├── dem/                          # USGS 3DEP tiles (gitignored)
│   ├── terrain/                      # Slope, aspect, TRI rasters
│   ├── geology/                      # Quaternary geology shapefiles
│   └── model/                        # Trained model + outputs
├── outputs/
│   ├── IceWave_Field_Report.pdf      # Field-ready target report
│   ├── icewave_targets.kmz           # Google Earth file
│   └── icewave_targets.gpx           # Garmin GPS file
├── qgis/                             # QGIS project files
├── assets/                           # Project imagery
└── pixi.toml                         # Reproducible environment
```

---

## ⚙️ Setup

```powershell
git clone https://github.com/bdgroves/project_ice_wave.git
cd project_ice_wave
pixi install
pixi run lab
```

---

## 🗺️ Study Area

Three-state coverage: **Washington · Oregon · Nevada**

Key target zones:
- **Columbia Basin, WA** — ancient Lake Lewis shorelines, loess deposits
- **Willamette Valley, OR** — Missoula Flood sediments, valley fill
- **Channeled Scablands, WA** — post-glacial terrain
- **Great Basin, NV** — Pleistocene lake beds (Lake Lahontan shorelines)

---

## ⚠️ Disclaimer

This is a research tool. Predicted localities do NOT guarantee fossil presence.
Vertebrate fossil collection on federal land requires a **PRPA permit**.
Verify land ownership before entering any site.

---

## 🔗 Sister Project

Built on the workflow developed for [Project PaleoWave](https://github.com/bdgroves/Project-PaleoWave) — ML-assisted ichthyosaur locality prediction in Nevada (AUC 0.906).

---

## 📚 Data Sources

- [Paleobiology Database](https://paleobiodb.org) — occurrence records
- [USGS 3DEP](https://www.usgs.gov/3d-elevation-program) — elevation data
- [USGS NGMDB](https://mrdata.usgs.gov/geology/state/) — state geology maps

---

*Project IceWave — Because the ice age isn't over if you know where to look.* 🦣🧊
