# Mapping Food Security Hotspots (Nigeria)
## Overview
This project focuses on Mapping Food Security Hotspots in Nigeria.
**Mapping Food Security Hotspots** identifies, classifies, and visualizes food-security hotspots across Nigeria using open geospatial data and Python (run in VS Code). The workflow integrates seven indicators (slope, NDVI, flow accumulation, rainfall, population density, market access, and accessibility) into a composite **Food Security Index (FSI)**, classifies hotspots, computes LGA-level statistics, and produces publication-ready maps.
This repository is designed for **reproducibility** and **clarity**:
- Scripts are plain Python (no notebooks required).
- Long rasters are processed **chunk-by-chunk** to avoid memory errors.
- CRS (coordinate reference systems) checks and auto-fixes are built in where possible.
- Final outputs are ready for reports (legend, scale bar, north arrow, locator inset).
---
## Problem Statement 
> Food insecurity remains a major challenge in Nigeria, driven by population growth, environmental change, conflict, and limited access to agricultural resources. Policymakers, NGOs, and researchers often lack spatially detailed insights into where food insecurity risks are highest. Without clear hotspot mapping, it is difficult to target interventions, allocate resources effectively, and design sustainable solutions.
This project — Mapping Food Security Hotspots — addresses the challenge by leveraging geospatial data (e.g., population density, rainfall, agricultural land use, elevation, etc.) and spatial analysis tools to identify vulnerable regions at state and LGA levels. The results provide actionable insights to support decision-making in food security planning and disaster risk management.
---
## Repository Structure
MAPPING_FOOD_SECURITY_HOTSPOTS/ ├── data/ │   ├── auxiliary/                         # Nigeria boundary & admin layers (GADM/GEE) │   │   ├── raw_nigeria_iga.cpg │   │   ├── raw_nigeria_iga.dbf │   │   ├── raw_nigeria_iga.fix │   │   ├── raw_nigeria_iga.geojson │   │   ├── raw_nigeria_iga.prj │   │   ├── raw_nigeria_iga.shp │   │   └── raw_nigeria_iga.shx │   ├── final/ │   │   ├── food_security_hotspots.tif     # classified hotspot raster (Low/Med/High) │   │   ├── food_security_index.tif        # final continuous FSI raster │   │   └── fsi_thresholds.json            # saved thresholds used for classification │   ├── interim/ │   │   └── base_grid.tif │   ├── maps/ │   │   ├── correlation_heatmap.png │   │   ├── food_security_index_map.png │   │   └── fsi_hotspots.png │   ├── normalized/ │   │   ├── accessibility_norm.tif │   │   ├── flow_accum_norm.tif │   │   ├── market_distance_norm.tif │   │   ├── ndvi_norm.tif │   │   ├── population_density_norm.tif │   │   ├── rainfall_norm.tif │   │   └── slope_norm.tif │   ├── processed/ │   │   ├── normalized/ │   │   │   └── accessibility_epsg26391.tif │   │   ├── base_grid.tif │   │   ├── correlation_matrix.csv │   │   ├── flow_accum.tif │   │   ├── lga_fsi_stats.csv              # LGA-level means & hotspot counts │   │   ├── lga_fsi_stats.geojson          # LGA polygons joined with stats │   │   ├── market_distance.tif │   │   ├── ndvi_epsg26391.tif │   │   ├── nigeria_dem_clean.tif │   │   ├── nigeria_lga_26391.geojson │   │   ├── Nigeria_LGA_clean_projected.geojson │   │   ├── Nigeria_LGA_clean.geojson │   │   ├── nigeria_markets.geojson │   │   ├── nigeria_slope.tif │   │   ├── population_density_epsg26391.tif │   │   └── rainfall_epsg26391.tif │   └── raw/ │       ├── nigeria_accessibility.tif │       ├── nigeria_dem.tif │       ├── nigeria_ndvi_2022.tif │       ├── nigeria_population_density.tif │       └── nigeria_rainfall_2020.tif ├── docs/ │   ├── notebooks/ │   │   └── 00_prepare_iga.si.ipynb │   ├── outputs/ │   │   ├── charts/ │   │   └── maps/ │   └── scripts/                           # scripts (if you prefer running from here) │       ├── classify_fsi.py │       ├── correlation_analysis.py │       ├── fetch_markets_osm.py │       ├── final_food_security_index.py │       ├── load_iga_boundaries.py │       ├── maps_outputs.py │       ├── normalize_indicators.py │       ├── prepare_base_grid.py │       ├── process_accessibility.py │       ├── process_flow_accum.py │       ├── process_market_distance.py │       ├── process_ndvi.py │       ├── process_population_density.py │       ├── process_rainfall.py │       ├── process_slope.py │       ├── reproject_iga.py │       ├── visualize_fsi.py │       ├── zonal_1_stats.py │       └── zonal_stats.py ├── src/ ├── venv/ └── README.md
> **Note:** In your runs you used `python scripts/...`. If your scripts live under `docs/scripts/`, just change the command to `python docs/scripts/<scriptname>.py`.
---
## Data Sources
- **Administrative Boundaries:** GADM (Country, State, LGA). Exported via Google Earth Engine (GEE) and saved to `data/auxiliary/`.
- **Population Density:** WorldPop (via GEE).
- **Rainfall:** CHIRPS or ERA5 (via GEE).
- **NDVI:** Sentinel/Landsat-derived NDVI (via GEE).
- **DEM & Slope / Flow Accumulation:** SRTM/HYDROSHEDS (processed locally).
- **Market Locations:** Extracted from OpenStreetMap (via OSMnx) → `data/processed/nigeria_markets.geojson`.
- **Accessibility / Travel Time:** Global accessibility raster (raw → normalized).
---
## Installation
1) **Clone & enter project**
```bash
git clone https://github.com/Adam49-Tech/mapping-food-security-hotspots.git
cd mapping-food-security-hotspots
2. Create & activate venv
python -m venv venv
venv\Scripts\activate         # Windows
# or
source venv/bin/activate      # macOS/Linux
3. Install dependencies
pip install -r requirements.txt
> If you don’t have requirements.txt, install core libs:
pip install geopandas rasterio shapely pyproj rtree numpy pandas matplotlib contextily osmnx matplotlib-scalebar scipy tqdm
---
End-to-End Workflow (VS Code, Python)
> Adjust scripts/ → docs/scripts/ if that’s where your files are.
0. (Optional) Fetch markets from OSM
python scripts/fetch_markets_osm.py
1. Normalize indicators (chunked/block reads)
python scripts/normalize_indicators.py
2. Final FSI (weighted overlay, chunk-safe)
python scripts/final_food_security_index.py
# Output: data/final/food_security_index.tif
3. Classify hotspots (Low/Medium/High)
python scripts/raster_hotspot.py
# Output raster: data/final/food_security_hotspots.tif
# Output map:    outputs/food_security_hotspots.png
4. Zonal statistics (LGA)
python scripts/zonal_stats.py
# Outputs: data/processed/lga_fsi_stats.csv + .geojson
5. Visualize FSI raster + LGA boundaries
python scripts/visualize_fsi.py
# Output: data/maps/food_security_index_map.png
6. Correlation analysis (indicators)
python scripts/correlation_analysis.py
# Output: data/maps/correlation_heatmap.png
7. Final report-ready map (legend, scale, north arrow, inset)
python scripts/final_report_map.py
# Output: outputs/final_report_hotspot_map.png
8. LGA summary chart (Top hotspots)
python scripts/lga_summary_chart.py
# Output: outputs/top10_highrisk_lgas.png
---
Key Outputs
FSI raster (continuous): data/final/food_security_index.tif
Hotspots raster (3 classes): data/final/food_security_hotspots.tif
Hotspot map (PNG): outputs/food_security_hotspots.png
FSI map (PNG): data/maps/fsi_hotspot_map.png
LGA statistics: data/processed/lga_fsi_stats.csv and .geojson
Correlation heatmap: data/maps/correlation_heatmap.png
Final report-ready map: outputs/final_report_hotspot_map.png
Top-10 high-risk LGAs chart: outputs/top10_highrisk_lgas.png
---
Troubleshooting
1) PROJ / pyproj CRS errors (Windows)
If you see:
PROJ: ... DATABASE.LAYOUT.VERSION.MINOR = 2 whereas a number >= 4 is expected...
pyproj.exceptions.ProjError: Error creating Transformer from CRS
Fixes:
Ensure pyproj finds its own proj.db:
In Python, quick check:
from pyproj import datadir
print(datadir.get_data_dir())
Option A: Set environment var before running:
setx PROJ_LIB "%CD%\venv\Lib\site-packages\pyproj\proj_dir\share\proj"
Avoid conflicts with external PostGIS/PROJ installs on PATH.
As a last resort, reinstall:
pip install --force-reinstall --no-cache-dir pyproj
2) MemoryErrors on large rasters
We use block-based reading/writing and windowed processing in scripts.
If still heavy, reduce block_size (e.g., 256) inside scripts.
3) Colorbar / Matplotlib deprecation
We switched to plt.colormaps.get_cmap("RdYlGn_r") and attach colorbars to the correct axes.
---
📍 Hotspot Mapping Outputs
As part of the Mapping Food Security Hotspots project, the following outputs were generated:
1. Final Report-Ready Maps
Script: scripts/final_report_map.py
Description: Generates polished static maps highlighting food security hotspots.
Outputs (saved in outputs/maps/):
final_report_hotspot_map.png (high-resolution image)
final_report_hotspot_map.svg (vector format for publications)
2. Interactive Hotspot Map
Script: scripts/interactive_hotspots.py
Description: Creates an interactive web-based map of food security hotspots for detailed exploration.
Output (saved in outputs/maps/):
interactive_hotspots.html
Example Usage
# Generate final static report-ready maps
python scripts/final_report_map.py  
# Generate interactive map
python scripts/interactive_hotspots.py
---
How to View Maps in VS Code
Open the Explorer > navigate to outputs/ or data/maps/
Right-click an image → Open Preview
Or open in the default OS viewer with start outputs\final_report_hotspot_map.png (Windows)
---
Citation
If this project supports your work, please cite:
> Egwu, A. J. (2025). Mapping Food Security Hotspots (Nigeria). GitHub: https://github.com/Adam49-Tech
---
License
Distributed under the MIT License. See LICENSE for details.
---
Contact
Author/GitHub: Adam49-Tech
Email: egwuadam@yahoo.co.uk
Phone: +234 803 740 3381