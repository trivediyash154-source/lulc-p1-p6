# Phase 1 — Data Foundation

## Purpose

Build a multi-sensor, multi-decadal, analysis-ready satellite data archive for the Pune district. Establish compositing pipelines for Landsat (1990–2026), Sentinel-2 (2018–2026), and Sentinel-1 (2017–2025) at three benchmark windows (30 m) and district-wide (240 m).

## Research Questions

- Can reproducible, co-registered, quality-assured seasonal and annual composites be built for Pune across 37 years?
- What is the observation depth and temporal support across sensors and seasons?
- What is the radiometric relationship between Sentinel-2 and Landsat?

## Inputs

- Landsat Collection 2, Level 2 scenes (L5 TM, L7 ETM+, L8 OLI, L9 OLI-2) from Planetary Computer
- Sentinel-2 Level 2A scenes from Planetary Computer
- Sentinel-1 RTC gamma0 from Planetary Computer
- SRTM 30 m DEM

## Methods

- Median compositing with cloud/shadow masking (Fmask/QA bits)
- Seasonal composites: dry season (Jan–Apr), post-monsoon (Oct–Dec), wet/monsoon (Jun–Sep)
- Cross-sensor co-registration verified by sub-pixel cross-correlation
- S2→OLI radiometric transform fitted via RMA on 19 same-day acquisition pairs

## Experiments

Not documented in the supplied Phase 1 archive as a numbered registry. Phase 1 was a data-building phase. Specific experiments were numbered starting in Phase 2.

## Results

- 37-year Landsat dry/annual composites for 3 benchmark windows: reproducible, co-registered to < 0.1 px
- S2 monthly series (2018–2026): ready for within-sensor temporal analysis
- S1 monthly series (2017–2025): ready where coverage exists
- **S2 vs Landsat radiometric offset discovered:** S2 systematically brighter in all bands (20–50% in blue); gain-type difference between Sen2Cor and LaSRC processors
- S2→OLI transform fitted: LOPO NDVI bias reduced from -0.024 to +0.004

## Supported Findings

- Composites are reproducible bit-for-bit across the 36-year archive
- Co-registration is sub-pixel (< 0.1 px) across sensors and decades
- S2-Landsat radiometric offset is real, gain-type, and must be corrected for fusion

## Unsupported / Rejected Findings

- JRC water-based registration test was invalid (replaced by image cross-correlation)
- WSF-Evolution decoding initially produced invalid results (GDAL warper bug — fixed)

## Limitations

- District-wide products only at 240 m (district 30 m never built in Phase 1)
- Monsoon season composites not usable (optical blindness)
- 693 of 702 products lacked proper code version tracking (fixed later)
- Which processor (Sen2Cor or LaSRC) is closer to truth is unknown (AERONET comparison PENDING)

## Important Decisions

- Three benchmark windows selected: khadakwasla_mutha, baramati_agri, mulshi_ghats
- Dry-season composites chosen as the primary temporal basis

## Protocol Status

Not documented in the supplied Phase 1 archive. Formal protocol freezing began in Phase 4.

## Key Artifacts

| File | Description |
|------|-------------|
| data/pune-eo-phd_core.zip | Core repository archive |
| data/pune-eo-phd_figures.zip | Phase 1 figures archive |
| data/pune-eo-phd_samples_sentinel_2.zip | Sentinel-2 sample data |
| data/pune-eo-phd_samples_landsat_corridor30m_1.zip | Landsat 30 m corridor samples |
| figures/satellite_availability_timeline_1.png | Satellite availability timeline |
| figures/harmonization_stability_2.png | Harmonisation stability assessment |
| figures/s2_field_phenology_pune_G10_baramati_agri_2024.png | S2 field phenology |

## Dependencies

- **Downstream:** Phase 2 uses the composites and feature cubes built here
- **Upstream:** None (this is the foundation)

## Reproducibility

The compositing pipeline is reproducible given the same scene catalog from Planetary Computer. Bit-identical outputs have been verified in later phases (Phase 5 pilot vs window composites: 3,846,463 cell-epochs, max difference 0).

## Historical Notes

The Phase 1 source folder contains a subdirectory `pune-eo-phd 9/` with Landsat composite GeoTIFF products for the khadakwasla_mutha window (1990 and 2026 dry season). These are preserved in `data/pune-eo-phd 9/` and `archive/original_structure/pune-eo-phd 9/`.
