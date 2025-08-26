# Mapping Food Security Hotspots in Nigeria
**Author:** Adam Joseph Egwu  
**GitHub:** [Adam49-Tech](https://github.com/Adam49-Tech)  
**Email:** egwuadam@yahoo.co.uk  
**Phone:** +234 803 740 3381  
**Date:** August 2025  
---
## 1. Abstract / Executive Summary
Food insecurity poses a major challenge in Nigeria, where rapid population growth, environmental variability, and socio-economic inequalities increase the vulnerability of communities.  
This project develops a **geospatial approach for mapping food security hotspots** at the LGA level, integrating datasets such as **population density, rainfall, elevation, agricultural land use, and administrative boundaries**.  
Using **Python-based geospatial analysis** (GeoPandas, Rasterio, RichDEM, Matplotlib, Folium) and **QGIS visualization**, we identified vulnerable zones where high population density and low agricultural capacity overlap.  
The results highlight specific LGAs as **food security hotspots**, enabling policymakers and NGOs to better target interventions. This research demonstrates the importance of **open geospatial data and reproducible workflows** for food security monitoring.
---
## 2. Introduction
### Food Security Issues in Nigeria
Nigeria faces increasing challenges of food insecurity, driven by:
- **Climate change:** erratic rainfall, flooding, and drought  
- **Land degradation** reducing productivity  
- **Conflict in farming regions** disrupting supply chains  
- **Economic inequality** limiting access to food  
### Study Area
Nigeria, West Africa’s largest country with **200+ million people**, is divided into **36 states and 774 Local Government Areas (LGAs)**.  
This study maps vulnerability at the LGA level to provide **actionable insights** for decision-making.
---
## 3. Problem Statement
Food insecurity is unevenly distributed across Nigeria. Policymakers and humanitarian organizations lack **fine-scale spatial information** to identify where food security risks are concentrated.  
This project addresses this gap by leveraging **geospatial data** to identify **food security hotspots at the LGA level**.
---
## 4. Data and Methods
### Data Sources
- **Population density:** WorldPop (2020) → raster (people/km²)  
- **Rainfall:** CHIRPS (1981–2020) → mean annual precipitation  
- **Elevation & slope:** SRTM DEM (30m) → terrain suitability  
- **Administrative boundaries:** GADM v4.1 → states & LGAs  
- **Agricultural land cover:** ESA WorldCover (2020) → % cropland  
### Methodological Steps
**1. Data Preparation**  
- Download datasets into `data/raw/`  
- Preprocess & clip to Nigeria extent using GeoPandas & Rasterio  
**2. Population Density Raster**  
- Extracted from WorldPop (via Google Earth Engine)  
- Exported as `nigeria_population_density.tif`  
**3. Slope & NDVI Derivation**  
- Slope generated from SRTM DEM (RichDEM)  
- NDVI calculated from Sentinel-2 imagery  
**4. Hotspot Analysis**  
- Normalize indicators: population density, slope, rainfall, NDVI, cropland  
- Weighted overlay → food security index (FSI)  
**5. Visualization**  
- Maps exported from Python scripts  
- Finalized in **QGIS** with north arrow, scale bar, legend  
---
## 5. Results
### Maps
- **Map 1:** Nigeria LGA boundaries + population density  
- **Map 2:** Agricultural land distribution  
- **Map 3:** Slope and terrain suitability  
- **Map 4 (Final):** Composite Food Security Hotspots  
📌 **Fallback Outputs** (generated in this project):  
- `outputs/maps/generate_final_map_fixed.png` → static hotspot map (quantile binning)  
- `outputs/maps/interactive_hotspot_map_fixed.html` → interactive web map  
### Graphs & Tables
- **Table:** Top 10 LGAs with highest food insecurity index  
- **Chart:** Correlation between rainfall and population density  
---
## 6. Discussion
- **Northern Nigeria** → high vulnerability due to **arid climate** and **conflict**  
- **Middle Belt** → mixed results; fertile land but **conflict-driven displacement**  
- **Southern states** → generally better, but **urban LGAs** (Lagos, Port Harcourt) face stress from high population density  
**Future Work:** integrate climate change projections into the FSI model.
---
## 7. Conclusion
This project demonstrates a **data-driven geospatial framework** for mapping food security hotspots in Nigeria.  
### Policy Implications:
1. Target **food aid & agricultural support** to hotspot LGAs  
2. Integrate **geospatial monitoring** into food security strategies  
3. Strengthen **resilience planning** through better land use and climate adaptation  
---
## 8. References
- WorldPop (2020). Global High-Resolution Population Denominators Project  
- CHIRPS Rainfall Data, UCSB Climate Hazards Group  
- GADM Database of Global Administrative Areas (v4.1)  
- ESA WorldCover (2020)  
- Libraries: GeoPandas, Rasterio, RichDEM, Matplotlib, QGIS, Folium