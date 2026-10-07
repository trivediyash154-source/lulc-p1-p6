# Phase 2 — Baseline Modelling and Thematic Analysis

## Purpose

Train L1 land-cover classifiers on the Phase 1 data foundation, reconstruct historical LULC maps (1990–2026) via temporal smoothing, and perform thematic analyses: urban expansion, water body dynamics, vegetation stress, flood hazard proxies, change trajectories, and conversion drivers.

## Research Questions

- RQ-L1: Can a defensible L1 land-cover classification be trained on silver labels?
- RQ-LOWO: Does the model transfer to unseen landscapes? Does terrain help?
- RQ-MM: Do Sentinel-1 and Sentinel-2 red-edge add information?
- RQ-T: Can historical LULC be reconstructed defensibly (1990–2026)?
- RQ-U: How did built-up extent and expansion modes change?
- RQ-W: How have open-water extent and reservoir areas changed?
- RQ-V: Where has dry-season vegetation greenness changed?
- RQ-S: What are the rainfall–vegetation–reservoir associations?
- And others (see phase2/reports/phase2_results.md)

## Inputs

- Phase 1 composites (Landsat, S2, S1) for 3 benchmark windows
- Silver labels (4-product consensus: WorldCover + Esri + GHSL + WSF, 2020–21)
- CHIRPS monthly rainfall (1981–2026)
- Independent reference products (GHSL, WSF-Evolution, JRC GSW)

## Methods

- XGBoost, Random Forest, TempCNN, LSTM, Transformer classifiers
- 3 km spatial block cross-validation
- Leave-one-window-out (LOWO) evaluation
- HMM temporal smoothing for historical reconstruction
- Spectral drought confusion analysis
- Reservoir area–rainfall association analysis

## Experiments

35 experiments (EXP-L1-B-*, EXP-L1-LOWO-*, EXP-MM-*, EXP-LULC-*, EXP-AG-*, EXP-URB-*, EXP-WAT-*, EXP-STRESS-*, EXP-TRJ-*, EXP-UNC-*, etc.). Full registry in `reports/phase2_results.md`.

## Results

- **Within-window:** macro-F1 ≈ 0.979 (silver test blocks)
- **Unseen landscapes (LOWO):** macro-F1 0.636–0.879 (major transfer gap)
- **SAR contribution:** S1 adds 0.03–0.06 F1 on unseen windows
- **Temporal reconstruction:** HMM smoothing produces maps; false built-up reduced
- **Urban expansion:** characterized infill/edge/outlying modes
- **Water:** reservoir area tracks monsoon rainfall; Mutha river unresolvable at 30 m
- **Flood:** hazard proxy only; no S1 during cited events
- **Stress:** rainfall → crop greenness association (lag 1 year, ρ ≈ 0.34–0.62)
- **Agriculture:** Crop-type clusters failed (silhouette < 0.25)

## Supported Findings

- L1 classification achieves high within-window accuracy on silver labels
- SAR and terrain features improve unseen-window transfer
- HMM reduces temporal noise
- Rainfall–vegetation associations are detectable

## Unsupported / Rejected Findings

- Crop-type classification: PENDING (silhouette too low)
- District-wide 30 m maps: NOT READY (only 240 m)
- Gold-standard accuracy: NOT READY (0 gold labels)
- TerraClimate water balance: access failed (Planetary Computer zarr error)
- Mutha river width at 30 m: unresolvable

## Limitations

- **All accuracy is against silver/products — no human gold**
- Transfer gap of 0.1–0.35 macro-F1 between landscapes
- Historical maps not validated against independent gold
- Drought confounds classification in agricultural areas
- Single flood event with no SAR coverage

## Important Decisions

- Silver labels used for training and evaluation (no gold available)
- Block CV with 3 km blocks (autocorrelation extends beyond this)

## Protocol Status

No frozen protocol in Phase 2. Formal protocol freezing began in Phase 4.

## Key Artifacts

| File | Description |
|------|-------------|
| reports/phase2_results.md | Complete Phase 2 results (35 experiments) |
| reports/phase1_audit.md | Phase 1 audit with S2-Landsat findings |
| figures/fig_lulc_epochs_3.png | LULC epoch maps |
| figures/fig_drought_confusion.png | Drought confusion analysis |
| data/phase2_core.zip | Core Phase 2 archive |
| data/phase2_labels.zip | Label datasets |
| data/phase2_maps_*.zip | LULC maps per window |
| data/phase2_gold_chips_*.zip | Gold sample chip images |

## Dependencies

- **Upstream:** Phase 1 (composites, data foundation)
- **Downstream:** Phase 3 (systematic evaluation), all later phases

## Reproducibility

Experiment results are reproducible given the same composite archive and silver labels. Some experiments depend on random seeds (documented in registry). The compositing pipeline is deterministic.

## Historical Notes

The Phase 2 source contains a `pune-eo-phd_figures copy.zip` which appears to be a copy of Phase 1 figures. Both copies are preserved. The Phase 2 source also contains `phase1_audit.md` — the Phase 1 data audit performed at the start of Phase 2.
