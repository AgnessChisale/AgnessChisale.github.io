# GIS-Based Least Cost Path Analysis for Wildfire Evacuation Planning on Mudge Island, BC

## Overview
This project applies GIS-based least cost path analysis to identify optimal wildfire evacuation routes on Mudge Island, BC. Three cost factors — terrain slope, land cover, and road proximity — were reclassified into discrete movement cost categories and combined into a weighted cost surface. The surface was then used to model cumulative evacuation difficulty and delineate the least cost path from each residential structure to its nearest evacuation dock. The analysis was conducted entirely in ArcGIS Pro using the Spatial Analyst toolbox.

![Workflow Diagram](workflow_diagram.png)
*Figure 1. Sequential analytical workflow used to model wildfire evacuation movement difficulty and delineate optimal escape routes from residential structures to evacuation docks on Mudge Island, BC.*

---

## Objectives
- Pre-process and reclassify slope, land cover, and road proximity rasters into discrete movement cost categories
- Construct a weighted evacuation cost surface integrating all three cost factors (slope 10%, land cover 30%, road proximity 60%)
- Run distance accumulation analysis using evacuation docks as destination points
- Delineate least cost evacuation paths from each residential structure to its nearest dock
- Evaluate spatial patterns of evacuation difficulty across the island

---

## Data Sources & Tools

**Data**

| File | Description |
|------|-------------|
| DEM (BC Data Catalogue) | Digital Elevation Model clipped to Mudge Island boundary, used to derive slope in degrees |
| ESA WorldCover | Land cover raster reprojected to NAD 1983 UTM Zone 10N and clipped to study area |
| Road Network (OpenStreetMap) | Road network with manually digitized missing southern segment added prior to analysis |
| Evacuation Docks (Regional District Nanaimo) | Validated dock point locations used as evacuation destinations |
| Residential Structures (OpenStreetMap) | Building polygons converted to centroids to represent individual house locations |

**Software & Tools**

| Tool | Purpose |
|------|---------|
| ArcGIS Pro — Spatial Analyst | Slope derivation, reclassification, Euclidean distance, Raster Calculator |
| ArcGIS Pro — Distance Accumulation | Cumulative cost surface and back direction raster generation |
| ArcGIS Pro — Optimal Path as Line | Least cost path delineation from house centroids to nearest dock |

---

## Methods

A DEM was clipped to the Mudge Island boundary using the Extract by Mask tool and used to derive a continuous slope raster in degrees, with values ranging up to 51.5 degrees. The slope raster was reclassified into five discrete cost categories. The ESA WorldCover raster was reprojected and clipped before land cover classes were assigned movement cost values reflecting relative difficulty under wildfire evacuation conditions. The road network was reviewed against aerial imagery for completeness — a missing segment on the southern portion of the island was manually digitized and incorporated before Euclidean distance from the complete network was calculated and reclassified.

The reclassification schemes applied to each cost factor are summarised in the table below.

| Factor | Input Value/Range | Cost Value | Movement Difficulty |
|--------|------------------|------------|-------------------|
| Slope | 0 – 5° | 1 | Very Low |
| Slope | 5 – 10° | 2 | Low |
| Slope | 10 – 15° | 4 | Moderate |
| Slope | 15 – 20° | 8 | High |
| Slope | > 20° | 16 | Very High |
| Land cover | Built-up | 2 | Very Low |
| Land cover | Bare/Sparse | 3 | Low |
| Land cover | Grassland | 5 | Moderate |
| Land cover | Tree cover | 10 | Very High |
| Land cover | Water | NoData | Impassable |
| Road Proximity | 0 – 100 m | 1 | Very Low |
| Road Proximity | 100 – 200 m | 5 | Moderate |
| Road Proximity | > 200 m | 15 | High |

All three reclassified cost rasters were combined into a single weighted cost surface using the Raster Calculator expression:

```
(Slope_cost × 0.10) + (Landcover_cost × 0.30) + (Road_proximity_cost × 0.60)
```

The weighted surface was input to the Distance Accumulation tool alongside the DEM and dock points to produce a cumulative cost raster and back direction raster. Least cost paths were delineated for each residential structure using the Optimal Path as Line tool.

---

## Outputs

### Slope, Land Cover & Road Proximity Cost Rasters

![Slope Cost Raster](slope_cost_raster.png)
*Reclassified slope-based evacuation movement cost raster. Very low to low movement costs characterise flat to gently sloping terrain, while very high movement costs are concentrated on the steepest terrain along the southern and southeastern coastal margins.*

![Land Cover Cost Raster](landcover_cost_raster.png)
*Land cover classification (left) and corresponding reclassified movement cost raster (right). Tree cover across the island interior is assigned very high movement cost, while grassland receives moderate cost and built-up areas and bare ground receive very low to low costs.*

![Road Proximity Cost Raster](road_proximity_cost_raster.png)
*Euclidean distance from the road network (left) and corresponding reclassified road proximity cost raster (right). Very low to low movement costs follow the road corridors closely, while moderate to high costs extend into areas further from the road network.*

---

### Weighted Evacuation Cost Surface

![Weighted Cost Surface](weighted_cost_surface.png)
*Weighted evacuation movement cost surface for Mudge Island. Each cell value represents the relative difficulty of evacuation movement at that location, with higher values indicating greater movement difficulty represented by lighter areas, and lower values indicating easier movement represented by darker areas.*

---

### Distance Accumulation

![Distance Accumulation](distance_accumulation.png)
*Distance accumulation raster showing cumulative evacuation cost to the nearest dock from every location on Mudge Island. Green tones along the coastal perimeter indicate the lowest cumulative costs, progressing through yellow and orange to pink and white in the most difficult central interior zones.*

---

### Least Cost Evacuation Paths

![Least Cost Paths](least_cost_paths.png)
*Optimal least cost evacuation paths from residential structures to evacuation docks on Mudge Island. Each line represents one route per residential structure, delineated using the Optimal Path as Line tool in ArcGIS Pro.*

---

## Key Findings
- Road proximity was the dominant factor structuring evacuation routes, with most paths aligning closely with existing road corridors across the island
- Tree cover dominated nearly the entire island surface, making high-cost forested terrain unavoidable for most evacuation routes regardless of path trajectory
- Slope had a localised influence concentrated along the steep southern and eastern coastal margins, with minimal effect across the flat to gently sloping interior
- Coastal residents near docks faced significantly lower cumulative evacuation costs than interior residents, revealing a clear spatial disparity in evacuation accessibility
- The central island interior emerged as the most challenging zone for evacuation, with cumulative costs three to five times higher than coastal areas near docks

---

## Skills Demonstrated
- Raster pre-processing including clipping, reprojection, and slope derivation in ArcGIS Pro
- Manual feature digitizing to address road network completeness issues
- Multi-criteria weighted cost surface construction using the Raster Calculator
- Distance accumulation and least cost path analysis using ArcGIS Pro Spatial Analyst
- Cartographic layout design and map export in ArcGIS Pro
- Spatial interpretation of evacuation movement patterns across a complex island landscape

---

*Coordinate System: NAD 1983 UTM Zone 10N (EPSG: 26910) | Analysis conducted in ArcGIS Pro*
