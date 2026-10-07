# DATA PROVENANCE: PUNE EARTH OBSERVATION PhD

## Standardized Temporal Terminology

To prevent ambiguity across historical reports and documentation, all datasets in this repository are categorized under four precise temporal concepts:

- **ARCHIVE AVAILABILITY:** The complete historical period during which the satellite instrument or sensor fleet was operational and collected public data.
- **PROJECT ANALYSIS WINDOW:** The temporal period that this PhD research investigation specifically focuses on (**1990–2026**, 37 years).
- **ACTUALLY USED:** Satellite scenes, composites, and feature layers demonstrably processed, generated, and stored in repository experiments.
- **PLANNED:** Target datasets designed or registered for future processing upon acquiring requisite computational resources (e.g., cloud cluster execution).

---

## Satellite Observation Fleet

| Sensor / Dataset | Platform | Archive Availability | Project Analysis Window | Actually Used | Planned | Resolution | Access Source |
|:---|:---|:---|:---|:---|:---|:---|:---|
| **Landsat 5 TM** | Landsat 5 | 1984–2013 | 1990–2011 | Dry-season annual composites (3 windows + district 240m) | District 30m cube | 30 m | Planetary Computer (Collection 2 Level 2) |
| **Landsat 7 ETM+** | Landsat 7 | 1999–present (SLC-off post-May 2003) | 1999–2026 | Dry-season composites with QA gap masking | District 30m cube | 30 m | Planetary Computer (Collection 2 Level 2) |
| **Landsat 8 OLI** | Landsat 8 | 2013–present | 2013–2026 | Dry-season annual composites (3 windows + district 240m) | District 30m cube | 30 m | Planetary Computer (Collection 2 Level 2) |
| **Landsat 9 OLI-2**| Landsat 9 | 2021–present | 2021–2026 | Operational optical composites | District 30m cube | 30 m | Planetary Computer (Collection 2 Level 2) |
| **Sentinel-2 MSI** | S2A / S2B | 2015–present | 2018–2026 | Seasonal optical composites (resampled to 30 m) | Phenology metrics | 10–20 m (resampled to 30m) | Planetary Computer (Level 2A Sen2Cor) |
| **Sentinel-1 SAR** | S1A / S1B | 2014–present | 2017–2025 | RTC gamma0 VV/VH composites (Phase 3 experiments) | Wet-season flood proxies | ~10 m (resampled to 30m) | Planetary Computer (RTC gamma0) |
| **SRTM DEM** | Shuttle Endeavour | 2000 (static) | Static baseline | Elevation, slope, aspect, topographic wetness | District 30m cube | 30 m | Planetary Computer |
| **CHIRPS Rainfall**| Multi-satellite | 1981–present | 1981–2026 | Monthly precipitation series (5 km) | Drought analysis | 0.05° (~5 km) | Planetary Computer |

---

## Global Reference Datasets

| Product | Publishing Agency | Archive Availability | Study Usage Window | Spatial Resolution | Thematic Focus |
|:---|:---|:---|:---|:---|:---|
| **GHSL (GHS-BUILT-S)** | European Commission JRC | 1975–2030 epochs | 1990, 2000, 2014, 2018 | 10–100 m | Built-up surface comparison baseline |
| **WSF-Evolution** | German Aerospace Center (DLR) | 1985–2015 annual | 1990–2015 | 30 m | Historical urban settlement trajectory |
| **JRC Global Surface Water** | European Commission JRC | 1984–2020 | 1990–2020 | 30 m | Surface water persistence and seasonality |
| **ESA WorldCover** | European Space Agency | 2020, 2021 | 2020 | 10 m | Silver label consensus component |
| **Esri Land Cover** | Esri / Impact Observatory | 2017–2023 | 2020 | 10 m | Silver label consensus component |
| **GLC_FCS30D** | Chinese Academy of Sciences | 1990–2022 | 1990–2020 epochs | 30 m | Multi-epoch global comparison layer |

---

## Label Hierarchy & Provenance Tiers

| Label Tier | Type | Quantity / Scope | Status | Scientific Admissibility |
|:---|:---|:---|:---|:---|
| **Tier A (Human Gold Validation)** | Independent photo-interpreted points across high-resolution Google Earth historical imagery | 641 points (125 spatial blocks, 2 independent interpreters + adjudicator) | **NOT YET INTERPRETED** (Kits packaged in `phase6/data/`) | **Authoritative Ground Truth** (Mandatory for confirmatory validation) |
| **Tier A (Human T1 Training)** | Independent human-interpreted points for supervised classifier training | 699 points (with 2 km spatial buffer from validation blocks) | **NOT YET INTERPRETED** (Kits packaged in `phase6/data/`) | **Authoritative Training Baseline** |
| **Tier C (Silver Consensus)** | Multi-product spatial consensus (WorldCover + Esri + GHSL + WSF), restricted to interior pixel buffers | Window footprints and district pilot | **AVAILABLE** | **Exploratory Only** (Subject to historical backward-projection bias) |
| **Tier C (Provisional AI Labels)** | LLM/VLM interpretation of Phase 3 blind test kits | 437 points | **AVAILABLE** | **Exploratory Only** (Strictly barred from substituting for human gold) |

---

## End-to-End Processing Lineage

```text
Planetary Computer STAC Catalog (Raw Level 2 Scenes)
  │
  ▼
Cloud & Shadow QA Masking (Bit-mask fusion, SLC-off handling)
  │
  ▼
Seasonal & Annual Median Compositing (Dry season primary: Nov–March)
  │
  ▼
Radiometric Cross-Sensor Harmonisation (Local PIF RMA: TM/ETM+ → OLI)
  │
  ▼
QA Layer Construction (Observation count, clear fraction, uncertainty metrics)
  │
  ▼
Feature Engineering (NDVI, NDBI, MNDWI, SAR backscatter, SRTM topography)
  │
  ▼
Machine Learning Classification & Conformal Set Prediction
```
