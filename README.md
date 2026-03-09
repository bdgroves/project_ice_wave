# 🦣 Project IceWave — Pleistocene Megafauna Locality Intelligence

**Random Forest ML model for predicting Pleistocene megafauna fossil sites across WA, OR, NV, ID, MT**

[![West AUC](https://img.shields.io/badge/West%20AUC-0.890%20★-00b4d8)](https://github.com/bdgroves/project_ice_wave)
[![East AUC](https://img.shields.io/badge/East%20AUC-0.846%20◆-f4a261)](https://github.com/bdgroves/project_ice_wave)
[![Validated](https://img.shields.io/badge/E02-McBones%20✓-4a9a4a)](https://mcbones.org)
[![LiDAR](https://img.shields.io/badge/LiDAR%20TPI-13%2F20%20Valley%20Floor-7b68ee)](https://github.com/bdgroves/project_ice_wave)

---

## ✅ Blind Validation — E02 = McBones Coyote Canyon

The v3 east model predicted the Kennewick, WA corridor as its **#1 east target** before any knowledge of existing sites. March 2026: active Columbian mammoth excavation confirmed at McBones Coyote Canyon near Kennewick. LiDAR TPI independently confirms valley floor geometry.

| | Latitude | Longitude | TPI | Source |
|---|---|---|---|---|
| **IceWave E02** | **46.3003°N** | **120.1336°W** | **-5.4m ▼** | ML model only |
| McBones dig | ~46.30°N | ~120.1°W | — | Active excavation |
| LiDAR confirm | 46.3003°N | 120.1336°W | -5.4m | USGS 3DEP 15m DTM |

Mammoth drowned in Missoula Flood ~17,000 years ago, deposited as floodwaters receded — exactly the slack-water geometry the model learned to find.

---

## 🛰️ LiDAR TPI Analysis — Notebook 05

USGS 3DEP 15m bare-earth DTM confirms terrain context for all east targets. The 30m DEM showed slope=0 / TRI=0 for nearly every east target — indistinguishable. TPI (Topographic Position Index) at a 3km neighborhood resolves this cleanly.

**13 of 20 east targets confirmed as valley floor / channel zones.**

| Tier | Targets | TPI | Channel Area | Field Action |
|------|---------|-----|---------|--------------|
| **1A** Valley floor + in channel | E02★, E15, E18, E19, E27, E34, E40 | < -5m | > 15 km² | Top priority |
| **1B** Valley floor + in channel | E03, E04, E16, E20, E21, E23 | < -5m | smaller | High priority |
| **2** Flat plain, channel nearby | E05, E25, E26 | ~0m | 8–16 km² | Monitor for exposure |
| **X** Ridge/slope artifact | E29, E31, E32, E35 | > +1m | small | **Deprioritized** |

**Extreme basins:** E21 (TPI -250m, Wenatchee corridor) · E20 (-98m) · E04 (-106m) · E03 (-86m)

---

## 📊 Model Performance

| Version | Region | AUC | n | Notes |
|---------|--------|-----|---|-------|
| v3 | West | 0.890 ± 0.105 | 35 | ★ High confidence |
| v3 | East | 0.846 ± 0.073 | 40 | ◆ +0.280 vs v2 |
| v2 | East | 0.566 ± 0.121 | 17 | retired |
| v1 | Both | 0.853 ± 0.063 | 78 | retired |

- **Cascade split:** -121.5°W — separate models for west (maritime) and east (semi-arid)
- **Composite score:** 80% ML probability + 20% SGMC lithology
- **East training:** PBDB 5-state harvest (WA, OR, NV, ID, MT), background ratio 8:1
- **Features:** elevation, slope, aspect, TRI, TWI, SGMC lithology (+ TPI in v4 retraining)

---

## 🗺️ Top Targets

### West ★ (AUC 0.890 — Willamette Valley / Puget Sound)

| Rank | Latitude | Longitude | Score | Waypoint |
|------|----------|-----------|-------|----------|
| W01 | 45.1753°N | 122.8419°W | 1.000 | IW-W01 |
| W02 | 45.2614°N | 122.9253°W | 0.993 | IW-W02 |
| W03 | 45.3003°N | 122.6753°W | 0.991 | IW-W03 |
| W04 | 45.3447°N | 122.6336°W | 0.989 | IW-W04 |
| W05 | 45.4253°N | 122.8836°W | 0.986 | IW-W05 |

### East ◆ (AUC 0.846 — Columbia Basin / Owyhee / Basin & Range)

| Rank | Latitude | Longitude | Score | TPI | Tier | Waypoint |
|------|----------|-----------|-------|-----|------|----------|
| **E02 ✓** | **46.3003°N** | **120.1336°W** | **1.000** | **-5.4m** | **1A** | IW-E02 |
| E03 | 46.6753°N | 117.7586°W | 1.000 | -86.3m | 1B | IW-E03 |
| E04 | 46.6336°N | 117.3836°W | 0.994 | -106.2m | 1B | IW-E04 |
| E05 | 46.3558°N | 120.5364°W | 0.994 | +0.1m | 2 | IW-E05 |
| E15 | 47.9531°N | 117.8003°W | 0.814 | -15.6m | 1A | IW-E15 |
| E16 | 47.9253°N | 117.3558°W | 0.814 | -6.7m | 1B | IW-E16 |
| E18 | 42.6892°N | 120.5364°W | 0.657 | -15.3m | 1A | IW-E18 |
| E19 | 42.4669°N | 117.9392°W | 0.616 | -3.2m | 1A | IW-E19 |
| E20 | 44.5503°N | 117.4253°W | 0.508 | -97.9m | 1B | IW-E20 |
| E21 | 47.5086°N | 121.1892°W | 0.478 | -250.1m | 1B | IW-E21 |

---

## 🗂️ Repository Structure

```
project_ice_wave/
├── notebooks/
│   ├── 01_data_harvest.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_west_model.ipynb
│   ├── 04_east_model_improvement.ipynb    # East RF v3, AUC 0.846
│   ├── 05_lidar_terrain_analysis.ipynb    # LiDAR TPI, 20/25 targets
│   └── 06_east_model_v4_tpi.ipynb         # Retrain with TPI feature (next)
├── data/
│   ├── model/
│   │   ├── icewave_v3_top50.csv
│   │   ├── icewave_v3_top50_lidar.csv     # + TPI columns
│   │   ├── icewave_rf_west.joblib
│   │   └── icewave_rf_east_v3.joblib
│   ├── pbdb/
│   │   └── icewave_east_expanded.csv      # 40 training points
│   └── lidar/
│       └── E##_dtm.tif                    # GeoTIFF DTMs (EPSG:4326)
└── outputs/
    ├── icewave_v3_targets.kmz             # Google Earth
    ├── icewave_v3_targets.gpx             # GPS waypoints
    ├── IceWave_Field_Report_v4.pdf
    └── lidar_E##.png                      # Hillshade/TPI/slope maps
```

---

## 🗺️ Viewing the Data

### Google Earth (easiest)
Open `outputs/icewave_v3_targets.kmz` — cyan = west ★, yellow = east ◆.

### kepler.gl (browser, no install)
1. Go to [kepler.gl](https://kepler.gl/demo)
2. Upload `data/model/icewave_v3_top50_lidar.csv`
3. Color by `composite_norm`, size by `tpi_15m` (invert: more negative = larger)

### QGIS (full GIS with LiDAR layers)

The DTM GeoTIFFs in `data/lidar/` are fully georeferenced (EPSG:4326) and load directly into QGIS at the correct location.

**Quick setup:**
1. `Layer → Add Raster Layer` → select any `E##_dtm.tif`
2. `Layer Properties → Symbology → Render type: Hillshade` for terrain view
3. Or `Render type: Singleband pseudocolor` → RdBu ramp, min=-50 max=+50 for TPI-style view
4. `Layer → Add Vector Layer` → load `icewave_v3_targets.kmz` for target pins on top
5. Add satellite basemap via QuickMapServices plugin

**Layer order (bottom to top):** Google Satellite → DTM rasters → KMZ targets → Labels

See full QGIS guide below in Field Notes.

### GPS Device
Load `outputs/icewave_v3_targets.gpx` into Garmin BaseCamp. Waypoints: `IW-W01`–`IW-W25`, `IW-E02`–`IW-E42`.

---

## ⚠️ Legal & Safety

**PRPA PERMIT REQUIRED** for vertebrate fossil collection on all federal land (up to $20,000 fine and/or imprisonment without permit).

- Verify land ownership: [BLM GeoCommunicator](https://geocommunicator.blm.gov)
- Do not disturb McBones Coyote Canyon (active dig — tours at mcbones.org · 509-438-9417)
- Remote terrain: 4L water/person/day, satellite communicator, trip plan

**BLM:** OR/WA 503-808-6002 · NV 775-861-6400 · ID 208-373-4000 · MT 406-896-5000

---

## 🦣 Target Species

Columbian mammoth · Woolly mammoth · American mastodon · Pleistocene horse · Camelops · Harlan's ground sloth · Pleistocene bison · Short-faced bear · Pleistocene wolf · Dire wolf

---

*Project IceWave v4 | March 2026 | West AUC 0.890 ★ | East AUC 0.846 ◆ | E02 Validated ✓ | LiDAR TPI 13/20 Valley Floor*
