# Phase 4 — final report (this session)

## What Phase 4 did

1. Complete data inventory and quality audit (`docs/phase4_data_inventory.md`, `phase4_data_quality.md`, `data/inventory/`).
2. Training- and validation-label audit (`phase4_training_labels.md`): silver labels are interior cells (edge share 7-25 % vs 65-72 %), some classes nearly absent, autocorrelation up to ~4 km.
3. Entry audit with weakness matrix and dependency graph (`phase4_audit.md`).
4. Evaluation protocol frozen (v1), independently verified, then corrected as v2 BEFORE any confirmatory run (selection rule, rainfall regions, 2 km training buffer, block-cluster bootstrap, YAML hashed); 15 unit tests; registry4 refuses confirmatory runs if code or protocol change (`phase4_evaluation_protocol.md`).
5. District validation design v2: 641 points in 125 independent blocks outside the Phase-3 windows, 13 strata oversampling hard landscapes and product disagreement; blind kit for two interpreters; design v1 superseded before use (`phase4_gold_protocol.md`).
6. Existing products brought onto the district grid and benchmarked (`existing_product_benchmark.md`): built-up area differs up to 4x between products in 2020; 1990 built-up agreement F1 0.32-0.43; all three land-cover products agree on 60 % of the district; disagreement is highest on 3-8° slopes, in the transition rainfall zone, near water and in recently settled areas (P4-X3).
7. Exploratory sensor-era test on pseudo-invariant targets (`phase4_sensor_era.md`): L5-dominated years show higher red and lower NDVI than L8/9-dominated years (confounded with time; few years per regime) — consistent with, not proof of, a residual sensor difference.
8. Earth Engine export pipeline for the district cube (`scripts/gee/export_district_cube.py`, dry-run tested).
9. Six confirmatory experiments pre-registered (now @v2 under protocol v2) before their gold data exist; P4-C2@v2 was motivated by exploratory P4-E1/X1/X2 and only its gold-based part is blind (recorded in the registry).

## Registry4

| id | kind | status | outcome | result / reason |
|---|---|---|---|---|
| P4-C1 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C1@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-C2 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C2@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-C3 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C3@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-C4 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C4@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-C5 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C5@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-C6 | confirmatory | SUPERSEDED | — | pre-registered before data exist: needs Tier-A district gold and the district 30 m cube |
| P4-C6@v2 | confirmatory | PENDING | — | needs Tier-A district gold (design v2) and the district 30 m cube |
| P4-E1 | exploratory | PASS | NA | 60 of 77 PIF x band x window contrasts differ between sensor regimes (CI excludes 0); median /diff/ reflectance 0.0100, NDVI 0.055 |
| P4-X1 | exploratory | PASS | NA | 2020 L1 agreement GLC~Esri 0.79, GLC~WorldCover 0.67, Esri~WorldCover 0.70; all three agree on 60% of the district; built 2015: of cells any product calls built, 37% are called bui |
| P4-X2 | exploratory | PASS | NA | P3-F3 vs GLC_FCS30D 4-class pixel agreement by epoch: khadakwasla_mutha 1990:0.44, 1995:0.43, 2000:0.40, 2005:0.58, 2010:0.46, 2015:0.60, 2020:0.62, 2022:0.61; baramati_agri 1990:0 |
| P4-X3 | exploratory | PASS | NA | district no-consensus 0.40; rainfall_zone: wet_west_ge1200 0.31, transition_700_1200 0.46, dry_east_lt700 0.40; slope: lt3deg 0.36, 3_8deg 0.61, ge8deg 0.34; distance_to_water: le3 |

## Decision

**NOT_READY** (`phase4_readiness_gate.md`). The blocking items are human gold and the district cube; both need resources outside this sandbox (interpreters; an Earth Engine project).
