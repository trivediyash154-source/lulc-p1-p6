# Data Lineage

## Composite Data Lineage

```
USGS/NASA Landsat Collection 2 Level 2
  → Planetary Computer STAC catalog
  → Cloud/shadow masking (QA_PIXEL bits 0-5)
  → Seasonal median compositing (repository compositing engine)
  → Harmonisation (L5→OLI via ETM+ identity, L7→OLI via local RMA fit)
  → QA layer generation (observation count, clear fraction, valid mask, uncertainty)
  → Analysis-ready composites (phaseN/data/*.zip)
```

## Label Data Lineage

```
Independent products (WorldCover, Esri, GHSL, WSF)
  → 4-product consensus (purity ≥ 0.78, interior cells)
  → Silver labels (Tier C) for training/evaluation
  → Experiment results against silver standard
```

Human gold labels (Tier A): NOT YET CREATED. Designed (641 pts), kits prepared, 0 interpreted.

## Reference Product Lineage

All reference products are from their original public sources. No modifications applied. Used as-is for independent validation.
