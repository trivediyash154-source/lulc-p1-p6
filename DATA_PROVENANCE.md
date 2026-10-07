# DATA PROVENANCE

## Satellite Data Sources

| Source | Platform | Period | Resolution | Access |
|--------|----------|--------|------------|--------|
| Landsat Collection 2, Level 2 | L5 TM, L7 ETM+, L8 OLI, L9 OLI-2 | 1989–2026 | 30 m | Planetary Computer (anonymous) |
| Sentinel-2 Level 2A | S2A/S2B (Sen2Cor) | 2015–2026 | 10–20 m (resampled to 30 m) | Planetary Computer |
| Sentinel-1 RTC | S1A/S1B (gamma0, VV/VH) | 2017–2025 | ~10 m (resampled to 30 m) | Planetary Computer |
| SRTM | Shuttle Radar Topography Mission | 2000 (static) | 30 m | Planetary Computer |
| CHIRPS | Climate Hazards Rainfall | 1981–2026 | 5 km (monthly) | Planetary Computer |

## Reference Products

| Product | Source | Period | Resolution |
|---------|--------|--------|------------|
| GHSL GHS-BUILT-S/GHS-POP | European Commission JRC | 1975–2030 | 10–100 m |
| WSF-Evolution | DLR | 1985–2015 | 30 m |
| JRC Global Surface Water | European Commission JRC | 1984–2020 | 30 m |
| WorldCover | ESA | 2020, 2021 | 10 m |
| Esri Land Cover | Esri/Microsoft | 2017–2023 | 10 m |
| GLC_FCS30D | CAS (Zhang et al.) | 1990–2022 | 30 m |

## Labels

| Label type | Status | Source |
|------------|--------|--------|
| Silver (Tier C) | Available | 4-product consensus (WorldCover + Esri + GHSL + WSF), interior cells, 2020–21 |
| Provisional AI (Tier C) | Available (437 pts) | Claude/AI interpretation of Phase 3 blind kit |
| Human gold (Tier A) | NOT AVAILABLE | Designed (641 pts, 125 blocks); kits ready; 0 interpreted |
| Human T1 training (Tier A) | NOT AVAILABLE | Designed (699 pts); kits ready; 0 interpreted |

## Data Lineage

```
Raw scenes (STAC catalog) → Cloud masking → Compositing (seasonal/annual medians)
  → Harmonisation (L5→OLI via ETM+ identity; L7→OLI via local fit; S2→OLI via RMA)
  → QA layer generation (observation count, clear fraction, valid mask, uncertainty)
  → Feature engineering (spectral indices, terrain, temporal features)
```

All compositing steps produce `metadata.json` files with source scene IDs, platform, dates, per-file SHA-256, and code/config hashes (from Phase 2 onward).
