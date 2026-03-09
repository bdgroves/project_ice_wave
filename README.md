# 🦣 Project IceWave

![IceWave Flag](assets/icewave_flag.png)

![Python 3.12](https://img.shields.io/badge/python-3.12-blue) ![Pixi](https://img.shields.io/badge/pixi-enabled-green) ![West AUC](https://img.shields.io/badge/West%20AUC-0.890-%2300b4d8) ![East AUC](https://img.shields.io/badge/East%20AUC-0.846-%23f4a261) ![Sister Project](https://img.shields.io/badge/sister-PaleoWave-purple)

> *The ice came. The mammoths came with it. Then both disappeared — but the bones remain.*

During the Pleistocene epoch (2.6M–11,700 years ago), Columbian mammoths (*Mammuthus columbi*) and woolly mammoths (*Mammuthus primigenius*) roamed the Pacific Northwest and Great Basin. Ancient lakes filled Nevada's valleys. Glaciers carved Washington's channeled scablands. And everywhere they walked, they left bones.

**Project IceWave asks: where haven't we looked yet?**

---

## 🎯 Mission

Use a split-ecoregion machine learning approach on USGS terrain data, SGMC lithology, and merged PBDB + iDigBio occurrence records to predict undiscovered Pleistocene megafauna fossil localities across Washington, Oregon, Nevada, Idaho, and Montana.

---

## 🤖 Model v3 — Split Ecoregion

The Cascade Mountain crest (~-121.5°W) divides the study area into two independently trained Random Forest models. The v3 east model was retrained on a broad 5-state Pleistocene vertebrate harvest, improving AUC from 0.566 to 0.846.

| Model | Ecoregion | Training Points | CV AUC | Confidence | Delta |
|-------|-----------|-----------------|--------|------------|-------|
| **West RF** | Willamette Valley / Puget Sound / Coast Range | 35 | **0.890 ± 0.105** | ★ HIGH | +0.037 vs v1 |
| **East RF v3** | Columbia Plateau / Yakima / Basin & Range | 40 | **0.846 ± 0.073** | ◆ IMPROVED | +0.280 vs v2 |
| East RF v2 | Columbia Plateau / Basin & Range | 17 | 0.566 ± 0.121 | ○ retired | — |
| v1 Baseline | Combined WA/OR/NV | 78 | 0.853 ± 0.063 | Reference | — |

### Features (v3)
| Feature | Source | Role |
|---------|--------|------|
| Elevation | USGS 3DEP ~30m | Primary — valley floors preferred |
| TRI (Ruggedness) | Derived from DEM | Low roughness = open terrain |
| Slope | Derived from DEM | Flat to gentle preferred |
| Aspect | Derived from DEM | No strong directional preference |
| **Lith Score** | USGS SGMC | 20% of composite — clay/silt/alluvium = 1.0 |

**Composite score = 80% ML probability + 20% lithology favorability**

### Top Targets
| Rank | Location | Coordinates | Score | Model |
|------|----------|-------------|-------|-------|
| W01 | Tualatin Valley, OR | 45.1753°N, -122.8419°W | 1.000 | ★ WEST |
| E01 | Yakima Valley, WA | 46.3003°N, -120.1336°W | 1.000 | ◆ EAST |
| E02 | Palouse, WA | 46.6753°N, -117.7586°W | 1.000 | ◆ EAST |
| E03 | Palouse, WA | 46.6336°N, -117.3836°W | 0.994 | ◆ EAST |
| W02 | Willamette Valley, OR | 45.1336°N, -122.9114°W | 0.895 | ★ WEST |

---

## 🦣 Target Species

| Species | Common Name | Region |
|---------|-------------|--------|
| *Mammuthus columbi* | Columbian Mammoth | WA, OR, NV, ID |
| *Mammuthus primigenius* | Woolly Mammoth | WA, OR |
| *Mammut americanum* | American Mastodon | WA, OR |
| *Equus sp.* | Pleistocene Horse | WA, OR, NV, MT |
| *Camelops hesternus* | Yesterday's Camel | WA, OR, NV, ID |
| *Paramylodon harlani* | Harlan's Ground Sloth | OR, NV |
| *Bison sp.* | Pleistocene Bison | WA, OR, NV, ID |
| *Arctodus simus* | Short-faced Bear | WA, OR, NV, MT |
| *Canis sp.* | Pleistocene Wolf/Dog | OR, NV, ID, MT |
| *Odocoileus sp.* | Pleistocene Deer | OR, ID, MT |

---

## 📁 Project Structure

```
project_ice_wave/
├── notebooks/
│   ├── 01_pbdb_harvester.ipynb           # PBDB API → Pleistocene occurrences
│   ├── 01b_idigbio_harvester.ipynb       # iDigBio API → 304 additional records
│   ├── 01c_merge_and_enrich.ipynb        # Merge + dedupe + SGMC lith enrichment
│   ├── 02_terrain_analysis.ipynb         # USGS 3DEP DEM → terrain metrics
│   ├── 03_ml_model.ipynb                 # RF v1 — combined model (AUC 0.853)
│   ├── 03b_ml_model_v2.ipynb             # RF v2 — lith_score added (AUC 0.879)
│   ├── 03c_ml_model_v2_split.ipynb       # RF v2 split ecoregion (West 0.890 / East 0.566)
│   └── 04_east_model_improvement.ipynb   # RF v3 east — 5-state harvest (East 0.846)
├── data/
│   ├── pbdb/                             # PBDB occurrence CSVs
│   │   ├── icewave_occurrences_with_terrain.csv
│   │   └── icewave_east_expanded.csv     # v3 east training data (40 pts, 5 states)
│   ├── idigbio/                          # iDigBio occurrence CSV + GeoJSON
│   ├── merged/                           # Deduped merged occurrences + lith scores
│   ├── terrain/                          # DEM mosaic, slope, aspect, TRI rasters
│   ├── geology/                          # Quaternary deposits shapefile
│   │   └── SGMC/                         # USGS SGMC (gitignored — 1.5GB)
│   ├── paleolakes/                       # NHD waterbodies GeoJSON
│   └── model/
│       ├── icewave_rf_west.joblib        # West RF (AUC 0.890)
│       ├── icewave_rf_east_v3.joblib     # East RF v3 (AUC 0.846)
│       ├── icewave_v2_split_top50.csv    # v2 targets
│       └── icewave_v3_top50.csv          # v3 targets (current)
├── outputs/
│   ├── IceWave_Field_Report_v3.pdf       # Current field report
│   ├── icewave_v3_targets.kmz            # Google Earth — cyan=west ★, yellow=east ◆
│   └── icewave_v3_targets.gpx            # GPS waypoints — IW-W## / IW-E##
├── assets/
│   └── icewave_flag.png                  # Project flag — Cascadia tricolor + mammoth
└── pixi.toml                             # Reproducible environment
```

---

## ⚙️ Setup

```bash
git clone https://github.com/bdgroves/project_ice_wave.git
cd project_ice_wave
pixi install
pixi run lab
```

---

## 🗺️ Viewing the Data

| Tool | Best For | How |
|------|----------|-----|
| **Google Earth** | Field planning, sharing targets | Open `outputs/icewave_v3_targets.kmz` — cyan = west ★, yellow = east ◆ |
| **kepler.gl** | Interactive web map, sharing | Upload `data/model/icewave_v3_top50.csv` at [kepler.gl](https://kepler.gl) |
| **QGIS** | Full GIS analysis | Load KMZ + `quaternary_deposits.shp` + DEM rasters |
| **GPS device** | Field navigation | Load `outputs/icewave_v3_targets.gpx` — waypoints named IW-W## / IW-E## |

---

## 🗺️ Study Area

Five-state coverage: **Washington · Oregon · Nevada · Idaho · Montana**

Key target zones:
- **Willamette Valley, OR** — Missoula Flood sediments, Pleistocene lake clay/silt (★ top west zone)
- **Puget Sound Lowlands, WA** — Glacial outwash, river floodplains (★ high priority)
- **Yakima Valley, WA** — Broad intermontane alluvial basin (◆ top east zone, v3)
- **Palouse, WA/ID** — Loess over Quaternary alluvium (◆ high priority east)
- **Channeled Scablands, WA** — Post-glacial terrain
- **SE Oregon Basin & Range** — Ancient playa lakes and alluvial fans
- **Great Basin, NV** — Pleistocene lake beds (Lake Lahontan shorelines)

---

## 📊 Data Sources

| Source | Records | Use |
|--------|---------|-----|
| [Paleobiology Database](https://paleobiodb.org) | 539 (5-state broad harvest) | East v3 training |
| [iDigBio](https://idigbio.org) | 304 raw → 65 deduped | West training |
| [USGS 3DEP](https://www.usgs.gov/3d-elevation-program) | 25 DEM tiles | Terrain features |
| [USGS SGMC](https://www.usgs.gov/programs/national-cooperative-geologic-mapping-program/science/sgmc) | National geology | Lithology scoring |
| [USGS NHD](https://www.usgs.gov/national-hydrography) | Waterbodies | Paleolake proximity |

---

## ⚠️ Disclaimer

This is a research tool. Predicted localities do NOT guarantee fossil presence. **Vertebrate fossil collection on federal land requires a PRPA permit.** Verify land ownership before entering any site.

BLM Oregon/Washington: 503-808-6002 · BLM Nevada: 775-861-6400 · BLM Idaho: 208-373-4000 · BLM Montana: 406-896-5000

---

## 🔗 Sister Project

Built on the workflow developed for [Project PaleoWave](https://github.com/bdgroves/Project-PaleoWave) — ML-assisted ichthyosaur locality prediction in Nevada (AUC 0.906).

---

*Project IceWave — Because the ice age isn't over if you know where to look.* 🦣🧊
